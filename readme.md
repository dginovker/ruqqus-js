# ruqqus-js
A Node.js wrapper of the Ruqqus API; designed for Ruqqus Applications.

## Installation
```sh
npm install --save ruqqus-js
```


## Setup
If you do not already have an authorized Ruqqus Application, check step one on the [official help page](https://ruqqus.com/help/oauth).

All API endpoints require an Access Token. This Access Token expires every hour; fortunately, the OAuth2 grant also comes with a Refresh Token. All of this is taken care of automatically. The OAuth2 grant checks for an authentication code before providing the Access Token.

Once again, you can learn how to acquire the authorization code by following steps two and three on the help page, linked above. Additional info can be found in the Bot Development section of the [Unofficial Ruqqus API Guide](https://drive.google.com/file/d/1ym_SeMsRJGzGFoWOX8SK798WUye7y0hg/view?usp=sharing).


## Example
```js
const Ruqqus = require("ruqqus-js");

// Create a new Client instance
const client = new Ruqqus.Client({
  id: "CLIENT_ID",
  token: "CLIENT_SECRET",
  code: "AUTHCODE"
});

// Login event
client.on("login", async () => {
  // Get the +RuqqusAPI guild and submit a post
  client.guilds.get("RuqqusAPI").post("Hey guys!", "I just posted this with a bot!");

  // Asynchronously fetch post data from a post ID and log it to the console 
  console.log(await client.posts.fetchData("2v0b"));

  // Log the client's user data
  console.log(client.user.data);
});

// Post event
client.on("post", (post, data) => {
  // Comment on the post
  post.comment("Hello!");

  // Log the post data
  console.log(data);
});

// Comment event
client.on("comment", (comment) => {
  // Reply to the comment
  comment.reply("Very interesting.");

  // Upvote the comment
  comment.upvote();
});
```


## Documentation

### `Client(options)`
Creates a new Ruqqus Client instance.

#### Parameters

- **Object** `options`: An object containing the following fields:
  - `id` (String): The Application ID
  - `token` (String): The Application secret
  - `code` (String): The one-time use authorization code
  - `agent` (String): Optional custom `user_agent`
  - `refresh` (String): Optional refresh token (overrides authorization code)

### `Client.guilds.get(name)`
Gets a guild with the specified name.

#### Parameters

- **String** `name`: The guild name

#### Return

- **Guild**

### `Client.guilds.fetchData(name)`
Fetches the data from a guild with the specified name. (Asynchronous)

#### Parameters

- **String** `name`: The guild name

#### Return

- **Object**: The guild data

### `Client.guilds.isAvailable(name)`
Fetches whether or not a guild with the specified name is available. (Asynchronous)

#### Parameters

- **String** `name`: The guild name

#### Return

- **Boolean**

### `Client.posts.get(id)`
Gets a post with the specified ID.

#### Parameters

- **String** `id`: The post ID

#### Return

- **Post**

### `Client.posts.fetchData(id)`
Fetches the data from a post with the specified name. (Asynchronous)

#### Parameters

- **String** `id`: The post ID

#### Return

- **Object**: The post data

### `Client.comments.get(id)`
Gets a comment with the specified ID.

#### Parameters

- **String** `id`: The comment ID

#### Return

- **Comment**

### `Client.comments.fetchData(id)`
Fetches the data from a comment with the specified name. (Asynchronous)

#### Parameters

- **String** `id`: The comment ID

#### Return

- **Object**: The comment data

### `Client.users.get(username)`
Gets a user with the specified username.

#### Parameters

- **String** `username`: The user's name

#### Return

- **User**

### `Client.users.fetchData(username)`
Fetches the data from a user with the specified username. (Asynchronous)

#### Parameters

- **String** `username`: The user's name

#### Return

- **Object**: The user data

### `Client.users.isAvailable(username)`
Fetches whether or not a user with the specified username is available. (Asynchronous)

#### Parameters

- **String** `username`: The user's name

#### Return

- **Boolean**

### `Client.on(event)`
Adds an event listener function. When an event is emitted, runs the function.

#### Parameters

- **String** `event`: The event to emit. Valid events:
  - `login`: Emitted when first refresh key obtained
  - `post`: Emitted when a new post is made. Parameters:
    - **Post**: The post
    - **Object**: The post data
  - `comment`: Emitted when a new comment is made. Parameters:
    - **Comment**: The comment
    - **Object**: The comment data

### `Client.user.data`
The client's user data object.

### `Guild.post(title, body)`
Submits a post to the guild.

#### Parameters

- **String** `title`: The title of the post
- **String** `body`: The body of the post. Can include HTML and Markdown

### `Guild.fetchPosts(sort, limit)`
Fetches an array of post objects from the guild.

#### Parameters

- **String** `sort`: The post sorting method. Defaults to "new"
- **Number** `limit`: The amount of post objects to return. Defaults to 24

#### Return

- **Array**: The post objects

### `Guild.fetchComments(sort, limit)`
Fetches an array of comment objects from the guild.

#### Parameters

- **String** `sort`: The comment sorting method. Defaults to "new"
- **Number** `limit`: The amount of comment objects to return. Defaults to 24

#### Return

- **Array**: The comment objects

### `Post.comment(body)`
Submits a comment to the post.

#### Parameters

- **String** `body`: The body of the comment

### `Post.upvote()`
Upvotes the post.

### `Post.downvote()`
Downvotes the post.

### `Post.removeVote()`
Removes the client's vote from the post.

### `Comment.reply(body)`
Submits a reply to the comment.

#### Parameters

- **String** `body`: The body of the reply

### `Comment.upvote()`
Upvotes the comment.

### `Comment.downvote()`
Downvotes the comment.

### `Comment.removeVote()`
Removes the client's vote from the comment.