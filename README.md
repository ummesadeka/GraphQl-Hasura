# GraphQl-Hasura
## GraphQL client-server flow:
* Note that the GraphQL query is not really JSON; it looks like the shape of the JSON you want. So when we make a 'POST' request to send our GraphQL query to the server, it is sent as a "string" by the client.
* The server gets the JSON object and extracts the query string. As per the GraphQL syntax and the graph data model (GraphQL schema), the server processes and validates the GraphQL query.
* Just like a typical API server, the GraphQL API server then makes calls to a database or other services to fetch the data that the client requested.
* The server then takes the data and returns it to the client in a JSON object.

The most common way to browse a GraphQL API is to use GraphiQL. GraphiQL is a tool built by Facebook, (pronounced "graphical") that makes it easy to explore any GraphQL API.

When you connect GraphiQL to a GraphQL endpoint, it queries the server for its GraphQL schema and gives you a UI to browse and test queries, and that powers its amazing autocomplete!
Open GraphiQL at: hasura.io/learn/graphql/graphiql

## Adding parameters (arguments) to GraphQL queries
### Basic argument: Fetch 10 todos

query {
  todos(limit: 10) {
    id
    title
  }
}

### Multiple arguments on multiple fields: Fetch 1 user and 5 most recent todos for each user
query {
  users (limit: 1) {
    id
    name
    todos(order_by: {created_at: desc}, limit: 5) {
      id
      title
    }
  }
}

### GraphQL variables: Passing arguments to your queries dynamically

var limit = getMaxTodosFromUserInput();
var query = "query { todos (limit: " + limit.toString() + ") {id title} }";

### Fetch $limit number of todos

query ($limit: Int!) {
  todos(limit: $limit) {
    id
    title
  }
}

### Create a todo
mutation {
  insert_todos(objects: [{title: "new todo"}]) {
    returning {
      id
    }
  }
}

### Returning data after the mutation
mutation {
  insert_todos(objects: [{title: "new todo"}]) {
    returning {
      id
      title
      is_completed
      is_public
      created_at
    }
  }
}
### Parameterise what you insert
The parameterised GraphQL mutation
mutation($todo: todos_insert_input!){
  insert_todos(objects: [$todo]) {
    returning {
      id
    }
  }
}

As a query variable
{
  "todo": {
    "title": "A new dynamic todo"
  }
}
