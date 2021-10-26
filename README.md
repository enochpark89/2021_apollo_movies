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