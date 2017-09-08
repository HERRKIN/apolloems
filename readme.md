# Easy Modularization Scheme for Apollo-Server (Graphql)

This package allows you modularize your Graphql schema easily on your server.

### Prerequisites

 ```apollo-server```

 All of your schema types must be placed in
 `/schema/types` and the resolvers in `/schema/resolvers`

### Installing

It's simple. With npm:

```
npm install --save apolloems
```

### How to use

This module reads the files in two folders. `/schema/resolvers/` and `/schema/types/`. this will automatically  generate a Graphql Schema for you.

there is an `/example`folder if so you can see how it works.

1. Install this package
2. In your server.js (or equivalent) require easy-modularization-scheme and call the functon ems to generate the schema

```
const { graphqlExpress, graphiqlExpress } = require('apollo-server-express') ;
const { makeExecutableSchema } = require('graphql-tools');
const ems = require('easy-modularization-scheme')

const schema = makeExecutableSchema(ems());

const GRAPHQL_PORT = 3000;

const graphQLServer = express();

graphQLServer.use('/graphql', bodyParser.json(), graphqlExpress({ schema }));
```

3. You need to put all types in this way:

/schema/types/Author.js
```
const Post = `
type Post {
  id: Int!
  title: String
  author: Author
  votes: Int
}
`

const RootQuery = `
  posts: [Post]
`

const MutatAll the = `
  upvotePost  must be placed in the (
    postId:
  ): Post
`

const Type = () =>  directlyPbst]
module.exports = {
  Type,
  RootQuery,
  Mutation
}
```

**The most important part is this.**
```
...
const Type = () => [Post]
module.exports = {
  Type,
  RootQuery,
  Mutation
}
```

All the types in the file must be placed in the Type function. You could include several types directly but its cleaner to use one variable per type (If you want to declare a Type called Books for example, i suggest use ```const Book = `type Book ...```)

If you don't use RootQuery or Mutation you can avoid their declaration.

4. This is how resolvers work:
```
const { find, filter } = require('lodash');
// example data
const posts = [
  { id: 1, authorId: 1, title: 'Introduction to GraphQL', votes: 2 },
  { id: 2, authorId: 2, title: 'Welcome to Meteor', votes: 3 },
  { id: 3, authorId: 2, title: 'Advanced GraphQL', votes: 1 },
  { id: 4, authorId: 3, title: 'Launchpad is Cool', votes: 7 },
];

module.exports = {
  RootQuery: {
    posts: () => posts
  },
  Mutation: {
    upvotePost: (_, { postId }) => {
      const post = find(posts, { id: postId });
      if (!post) {
        throw new Error(`Couldn't find post with id ${postId}`);
      }
      post.votes += 1;
      return post;
    },
  },
  Author: {
    posts: (author) => filter(posts, { authorId: author.id }),
  }
};
```

Use always RootQuery instead of Query.

5. Done! Run your server!

## Tests

Sorry. No test yet.

## History

V1.0.0 First version

## Todo

- [ ] Allow custom scalars
- [ ] Subscriptions
- [ ] Allow Comments in RootQuery and mutation

## License

This project is licensed under the MIT License

## Acknowledgments

* Apollo GraphQL staff
* My FullStack devs Facebook Group
* React Community
