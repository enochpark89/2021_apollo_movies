# Apollo 2021

- Movie app built with React, Apollo and GraphQL.

# Requirement

- Styled-components
- React-router-DOM
- React apollo.
```bash
# run below
npm add styled-components react-router-dom 
npm install apollo-boost @apollo/react-hooks graphql
```
*What is apollo?*
- Apollos is the best way to consume graphQL API.
- GraphQL API is not like restAPI that you get JSON from URL. 

- Reset-css


# Connect with Github

- Get started on the router

# Development process

# 1.0 Apollo Client

- RestAPI and GraphQL APIs are different. While RestAPI respond with a JSON file. 
- We have apollo that helps us resolve this complication.
- In order to set up Apollo, you need to set up a client.

src/apollo.js
```js
import ApolloClient from "apollo-boost";

const client = new ApolloClient({
  uri: "https://movieql.now.sh/"
});

export default client;
```

- wrap the application around it.

src/index.js
```js
// Import apollo
import { ApolloProvider } from "@apollo/react-hooks";
import client from "./apollo";

ReactDOM.render(
    // wrap apolloProvider and a client
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById("root")
);
```

# 2.0 GET MOVIES query

- If you want to query the movie data, you want to get the id and the posters.
- Backend view:
```graphql
{
    movies {
        id
        medium_cover_image
    }
}
```

- We want to load the graphql command from the Home.js route. 

src/routes/Home.js
```js
import React from "react";
import {gql} from "apollo-boost";
import {useQuery} from "@apollo/react-hooks";

// use gql to create a query
const GET_MOVIES = gql`
  {
    movies {
      id
      medium_cover_image
    }
  }
`;

// use useQuery hook to get data from the server
export default () => {
    const {loading, error, data} = useQuery(GET_MOVIES);
    console.log(loading, error, data);
    return <h1>Home</h1>;
}

```

Final Home.js:
```js
import { gql } from "apollo-boost";
import { useQuery } from "@apollo/react-hooks";

const GET_MOVIES = gql`
  {
    movies {
      id
      medium_cover_image
    }
  }
`;

// you will map only id of the data retrieved.
export default () => {
  const { loading, data } = useQuery(GET_MOVIES);
  if (loading) {
    return "loading...";
  }
  if (data && data.movies) {
    return data.movies.map(m => <h1>{m.id}</h1>);
  }
};
```

# 3.0 GET MOVIE Query

- css addition to style the page. 
home.js
```js
const Container = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
`;
const Header = styled.header`
  background-image: linear-gradient(-45deg, #d754ab, #fd723a);
  height: 45vh;
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
`;
const Title = styled.h1`
  font-size: 60px;
  font-weight: 600;
  margin-bottom: 20px;
`;
const Subtitle = styled.h3`
  font-size: 35px;
`;
const Loading = styled.div`
  font-size: 18px;
  opacity: 0.5;
  font-weight: 500;
  margin-top: 10px;
`;
```

Final:
```js
import React from "react";
import { gql } from "apollo-boost";
import { useQuery } from "@apollo/react-hooks";
import styled from "styled-components";
import Movie from "../components/Movie";

// use gpl query.
const GET_MOVIES = gql`
  {
    movies {
      id
      medium_cover_image
    }
  }
`;

// Styled Components
const Container = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
`;
const Header = styled.header`
  background-image: linear-gradient(-45deg, #d754ab, #fd723a);
  height: 45vh;
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
`;
const Title = styled.h1`
  font-size: 60px;
  font-weight: 600;
  margin-bottom: 20px;
`;
const Subtitle = styled.h3`
  font-size: 35px;
`;
const Loading = styled.div`
  font-size: 18px;
  opacity: 0.5;
  font-weight: 500;
  margin-top: 10px;
`;

// you will map only id of the data retrieved.
export default () => {
    const { loading, data } = useQuery(GET_MOVIES);
    return (
      <Container>
        <Header>
          <Title>Apollo 2021</Title>
          <Subtitle>I love GraphQL</Subtitle>
        </Header>
        {loading && <Loading>Loading...</Loading>}
        {!loading &&
          data.movies &&
          data.movies.map(m => <Movie key={m.id} id={m.id} />)}
      </Container>
    );
  };
```

# 4.0 Cache and Styles

- If Apollo gets something, it is going to keep it there. 
- User is going to see less and less loading stage. 
- If you use the Redux, you have to do everything else but when you click on the detail and go back to the menu, the page is cached so that user can access again using the cache without having to request data.

- we are going to style Container and Poster. Get the image
- Following pages has been updated:
  - Movie.js: Creates a Container, Poster, and Link to the Detail view
  - Detail.js: Retrieve language and rating data. Creates styled-components called Container, Column, Title, Subtitle, Description, and Poster. The components are displayed on the detailed page. 
  - Home.js: Creates a Movies in grid so that each movie will display as a organized grid. When the loading is done and data is retrieved, the data is going to be mapped and gets sent to Movie.js.

# 5.0 Data & Apollo Dev Tool,s

- You always have to ask for data immediately because sometimes, data does not get sent. 
- If the data is undefined or unable to retrieve, it will show the error and not lead the page. 
  - Use terniary condition to route users and handle undefined data.
- In this section, we will update the Detail.js page to show the **language** and **description** of the movies.
- update Poster to check the existence of data. If data exist......

- you can see the tab called Apollo and see that there are many cache saved for users. Once the user enter the detailed site, apollo will add the data inside the cache so that user can use that data when they go back to that specific data.

# 6.0 Suggestions

- If you go to the playground, you see the suggestions.
- The suggestions will have the data of its own about the movie. 
- You can use optional chaining to check whether the data exist or not as below instead of using the terniary operator.

Detail.js
```js
......(skiped)
        <Subtitle>
          {data?.movie?.language} · {data?.movie?.rating}
        </Subtitle>
        <Description>{data?.movie?.description_intro}</Description>
......(skiped)
        <Poster bg={data?.movie?.medium_cover_image}></Poster>
```

Home.js
```js
      <Movies>
        {data?.movies?.map(m => (
          <Movie key={m.id} id={m.id} bg={m.medium_cover_image} />
        ))}
      </Movies>
```

# 7.0 - Local State

- Modify data that came by.
- If you need to do something with the data you got, create dynamics and interactivity for users. 
- For example, user can click like button and it will add like property inside the data. 

**Make sure that this is happening on a localhost**

- Create a resolver on apollo.js

src/apollo.js
```js
  uri: "http://localhost:4000",
  resolvers: {
    Movie: {
      isLiked: () => false
    }
  }
```

- From a backend, update isLiked property and send it to presenters.

Home.js
```js
// add gql
const GET_MOVIES = gql`
  {
    movies {
      id
      medium_cover_image
      isLiked @client
    }
  }
`;

// send data + updated data to Movie presenter
          <Movie
            key={m.id}
            id={m.id}
            isLiked={m.isLiked}
            bg={m.medium_cover_image}
          />


```

Movie.js
```js
// add like button on Movie.js
export default ({ id, bg, isLiked }) => (
    <Container>
    <Link to={`/${id}`}>
      <Poster bg={bg} />
    </Link>
    <button>{isLiked ? "Unlike" : "Like"}</button>
  </Container>
);
```

# 8.0 How do you make an interactivity to a like button.

- You can use mutation to update isLike.

apollo.js
```js

// resolver creates isLike() that you can use
resolvers: {
    Movie: {
      isLiked: () => false
    },
    // Mutation will get the cache and change to true.
    Mutation: {
      likeMovie: (_, { id }, { cache }) => {
        cache.writeData({
          id: `Movie:${id}`,
          data: {
            isLiked: true,
            medium_cover_image: "lalalalal"
          }
        });
      }
    }
  }
});
```

- Incorporate Mutation to src/components/Movie.js

```ts
import React from "react";
import { Link } from "react-router-dom";
import styled from "styled-components";
// mutation is imported.
import { gql } from "apollo-boost";
import { useMutation } from "@apollo/react-hooks";

const LIKE_MOVIE = gql`
  mutation likeMovie($id: Int!) {
    likeMovie(id: $id) @client
  }
`;

const Container = styled.div`
  height: 400px;
	@@ -19,11 +27,18 @@ const Poster = styled.div`
  border-radius: 7px;
`;

export default ({ id, bg, isLiked }) => {
  const [likeMovie] = useMutation(LIKE_MOVIE, {
    variables: { id: parseInt(id) }
  });
  return (
    <Container>
      <Link to={`/${id}`}>
        <Poster bg={bg} />
      </Link>
        {/* Call the mutation; show the current status of the isLike*/}

      <button onClick={isLiked ? null : likeMovie}>
        {isLiked ? "Unlike" : "Like"}
      </button>
    </Container>
  );
};
```

# 9.0 Connecting Detail and Home

- Write unlikeMovie or toggleLikeMovie

src/apollo.js
```js
//toggleLikeMovie will change isLiked from true to false or false to true depending on a current state.
  Mutation: {
      toggleLikeMovie: (_, { id, isLiked }, { cache }) => {
        cache.writeData({
          id: `Movie:${id}`,
          data: {
            isLiked: !isLiked
          }
        });
      }
```

src/components/Movie.js

```js
import { useMutation } from "@apollo/react-hooks";

const LIKE_MOVIE = gql`
  mutation toggleLikeMovie($id: Int!, $isLiked: Boolean!) {
    toggleLikeMovie(id: $id, isLiked: $isLiked) @client
  }
`;

const Poster = styled.div`
`;

// Button will now activate toggleMovie mutation.

export default ({ id, bg, isLiked }) => {
  const [toggleMovie] = useMutation(LIKE_MOVIE, {
    variables: { id: parseInt(id), isLiked }
  });
  return (
    <Container>
      <Link to={`/${id}`}>
        <Poster bg={bg} />
      </Link>
      <button onClick={toggleMovie}>{isLiked ? "Unlike" : "Like"}</button>
    </Container>
  );
};
```

- Change the detail page so that it would show on the detail.

src/routes/Detail.js
```js
const GET_MOVIE = gql`
  query getMovie($id: Int!) {
    movie(id: $id) {
      id
      title
      medium_cover_image
      language
      rating
      description_intro
      isLiked @client
    }
    suggestions(id: $id) {
      id
      medium_cover_image
    }
  }
`;
```
