# 💎 [How to Graphql with Ruby on Rails](https://www.howtographql.com/graphql-ruby)

##### Each tutorial step is broken down in this repo's [commit history](https://github.com/ztratify/howtographql-rails/commits/main)

1. [Getting Started](https://www.howtographql.com/graphql-ruby/1-getting-started/)
2. Queries
3. Mutations
4. Authentication
5. Connecting Nodes
6. Error Handling
7. Filtering
8. Pagination
9. Summary

## Installation

Install dependencies:

```
bundle install

rails db:setup
```

Starting the server:

```
rails server
```

Opening the application:

```
open http://localhost:3000/
```

## Interesting Files:

- [GraphqlController](https://github.com/howtographql/graphql-ruby/blob/master/app/controllers/graphql_controller.rb) - GraphQL controller (api entry point)
- [GraphqlTutorialSchema](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/graphql_tutorial_schema.rb) - the schema definition
- [Mutations](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/mutation_type.rb) - root mutations
- [Queries](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/query_type.rb) - root queries
- [UserType](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/user_type.rb) - record type
- [VoteType](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/vote_type.rb) - record type
- [LinkType](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/link_type.rb) - record type
- [DateTimeType](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/types/date_time_type.rb) - scalar type
- [LinksSearch](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/resolvers/links_search.rb) - complex search resolver and its [tests](https://github.com/howtographql/graphql-ruby/blob/master/test/graphql/resolvers/links_search_test.rb)
- [CreateLink](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/mutations/create_link.rb) - mutation and its [tests](https://github.com/howtographql/graphql-ruby/blob/master/test/graphql/mutations/create_link_test.rb)
- [CreateUser](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/mutations/create_user.rb) - mutation and its [tests](https://github.com/howtographql/graphql-ruby/blob/master/test/graphql/mutations/create_user_test.rb)
- [CreateVote](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/mutations/create_vote.rb) - mutation and its [tests](https://github.com/howtographql/graphql-ruby/blob/master/test/graphql/mutations/create_vote_test.rb)
- [SignInUser](https://github.com/howtographql/graphql-ruby/blob/master/app/graphql/mutations/sign_in_user.rb) - mutation and its [tests](https://github.com/howtographql/graphql-ruby/blob/master/test/graphql/mutations/sign_in_user_test.rb)

## Sample GraphQL Queries

List first 5 links containing "watch":

```graphql
{
  searchLinks(
    first: 5,
    skip: 0,
    filter:{
      descriptionContains: "watch"
    }
  ) {
    url
    description
    postedBy {
      name
    }
  }
}

```

Creates new user:

```graphql
mutation {
  createUser(
      name: "Testy McTester",
      authProvider: {
        credentials: {
          email: "test@gmail.com",
          password: "password"
        }
      }
  ) {
    email
    id
    name
  }
}
```

Creates new user token:

```graphql
mutation {
  loginUser(
    credentials: {
    		email: "test@gmail.com",
      	password: "password"
  		}
  	) {
    token
    user {
      name
    }
  }
}
```

Creates new link:

```graphql
mutation {
  createLink(
    description: "Watch NHL 🏒",
    url: "http://nhl.com"
  ) {
    description
    url
    id
    postedBy {
      id
    }
  }
}
```

Creates new vote:

```graphql
mutation {
  createVote(
    linkId: "4"
  ) {
    link {
      description
      url
    }
  }
}
```

Get all links and votes, with the voter and their vote history:

```graphql
{
  allLinks {
    url
    description
    id
    votes {
      user {
        name
        votes {
          link {
            url
            description
            id
          }
        }
      }
    }
  }
}
```