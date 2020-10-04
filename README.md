# GraphQL
This repo is the product of following [this](https://www.digitalocean.com/community/tutorials/a-practical-graphql-getting-started-guide-with-nodejs) tutorial for implementing a GraphQL server using Express

### Local setup
* fork and clone
* install [Node](https://backend.turing.io/module4/lessons/express_knex) - select 'recommended for most users' option
* start the server by running `node server.js`
* visit `localhost:4000/graphql`

### Queries:
* Find a user:  
    * Query:  
      ```graphql
      query getSingleUser($userID: Int!) {
        user(id: $userID) {
          name
          age
          shark
        }
      }
      ```  
    * Query variables:  
      ```graphql
      {
        "userID": 1
      }
      ``` 
    * Output:
      ```graphql
      {
        "data": {
          "user": {
            "name": "Brian",
            "age": 21,
            "shark": "Great White Shark"
          }
        }
      }
      ```
* Find multiple users using aliases:  
    * Query:  
      ```graphql
      query getUsersWithAliases($userAID: Int!, $userBID: Int!) {
        userA: user(id: $userAID) {
          name
          age
          shark
        },
        userB: user(id: $userBID) {
          name
          age
          shark
        }
      }
      ```  
    * Query variables:  
      ```graphql
      {
        "userAID": 1,
        "userBID": 2
      }
      ``` 
    * Output:
      ```graphql
      {
        "data": {
          "userA": {
            "name": "Brian",
            "age": 21,
            "shark": "Great White Shark"
          },
          "userB": {
            "name": "Kim",
            "age": 22,
            "shark": "Whale Shark"
          }
        }
      }
      ```
* Find multiple users using aliases and fragments:  
    * Query:  
      ```graphql
      query getUsersWithFragments($userAID: Int!, $userBID: Int!) {
        userA: user(id: $userAID) {
          ...userFields
        },
        userB: user(id: $userBID) {
          ...userFields
        }
      }

      fragment userFields on Person {
        name
        age
        shark
      }
      ```  
    * Query variables:  
      ```graphql
      { 
        "userAID": 1,
        "userBID": 2
      }
      ``` 
    * Output:
      ```graphql
      {
        "data": {
          "userA": {
            "name": "Brian",
            "age": 21,
            "shark": "Great White Shark"
          },
          "userB": {
            "name": "Kim",
            "age": 22,
            "shark": "Whale Shark"
          }
        }
      }
      ```
* Retrieve users using directives - include id & skip age:  
    * Query:  
      ```graphql
      query getUsers($shark: String, $age: Boolean!, $id: Boolean!) {
        users(shark: $shark){
          ...userFields
        }
      }

      fragment userFields on Person {
        name
        age @skip(if: $age)
        id @include(if: $id)
      }
      ```  
    * Query variables:  
      ```graphql
      {
        "shark": "Hammerhead Shark",
        "age": true,
        "id": true
      }
      ``` 
    * Output:
      ```graphql
      {
        "data": {
          "users": [
            {
              "name": "Faith",
              "id": 3
            },
            {
              "name": "Joy",
              "id": 5
            }
          ]
        }
      }
      ```

### Mutations:
* Find a user:  
    * Initial user details:
      ```
      {
        "data": {
          "user": {
            "name": "Brian",
            "age": 21,
            "shark": "Great White Shark"
          }
        }
      }
      ```
    * Mutation:  
      ```graphql
      mutation updateUser($id: Int!, $name: String!, $age: String) {
        updateUser(id: $id, name:$name, age: $age){
          ...userFields
        }
      }

      fragment userFields on Person {
        name
        age
        shark
      }
      ```  
    * Query variables:  
      ```graphql
      {
        "id": 1,
        "name": "Keavin",
        "age": "27"
      }
      ``` 
    * Output:
      ```graphql
      {
        "data": {
          "updateUser": {
            "name": "Keavin",
            "age": 27,
            "shark": "Great White Shark"
          }
        }
      }
      ```
