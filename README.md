**This is a helper for the SBL API endpoint docs, which can be found [here](https://spacebots.gitbook.io/tutorial-en/api/stats 'Official SBL docs')**

# Endpoints

## Stat retrieval

### GET `/api/bots/:id`

Returns information about a single bot whose user ID matches the `id` url param\

#### Response

A JSON structures as:

```ts
{
  id: string;                // The bot's user ID
  prefix: string;            // The bot's prefix
  owner: Array<string>;      // An array of the bot's owners' IDs, can sometimes be more than one ID
  library: string;           // The library the bot uses
  description: string;       // The bot's short description
  longDescription: string;   // The bot's long description
  certified: boolean;        // Whether the bot is certified or not
  pageURL: string;           // The bot's page on SBL
  website?: string;          // A URL leading to the bot's website
  support?: string;          // The code for the invite to the bot's support server
  github?: string;           // The bot's GitHub repository
  servers?: number;          // The bot's server count, (sometimes doesn't exist)
  users?: number;            // The bot's user count, (sometimes doesn't exist)
  votes: number;             // The amount of users that voted for the bot on the website, can be 0
}
```

#### Error

##### When there's no bot with the specified ID

Status code: 404 Not Found\
Response:

```json
{
  "error": {
    "code": 404,
    "message": "A client with the given ID does not exist!"
  }
}
```

## Stat posting

### POST `/api/bots/:id`

Posts stats for the bot whose user ID matches the `id` url param\

#### Headers

- `Authorization` | The SBL API key of the bot

#### Body

Should be a JSON structured as:

```ts
{
  guilds: number; // Bot's server count (required, can be 0)
  users?: number; // Bot's user count (optional, can't be 0)
}
```

#### Response

A JSON structured as:

```ts
{
  servers: number; // The newly-posted server count
  users?: number;  // The newly-posted / already stored user count, if exists
}
```

#### Error

##### When there's no bot with the specified ID

Status code: 404 Not Found\
Response:

```json
{
  "error": {
    "code": 404,
    "message": "A client with the given ID does not exist!"
  }
}
```

##### When there's no `Authorization` header

Status code: 401 Unauthorized\
Response:

```json
{
  "error": {
    "code": 401,
    "message": "No API key in headers!"
  }
}
```

##### When the `Authorization` header doesn't match the bots API key

Status code: 403 Forbidden\
Response:

```json
{
  "error": {
    "code": 403,
    "message": "The provided API key is invalid!"
  }
}
```

##### When there's no guild count (`guilds` field) in the requests body

Status code: 400 Bad Request\
Response:

```json
{
  "error": {
    "code": 400,
    "message": "No guild count in the request's body!"
  }
}
```

##### When the guild count (`guilds` field) in the requests body is not a number

Status code: 400 Bad Request\
Response:

```json
{
  "error": {
    "code": 400,
    "message": "Invallid guild count in the request's body!"
  }
}
```

##### When the (optional) user count (`users` field) in the requests body is present and is not a number

Status code: 400 Bad Request\
Response:

```json
{
  "error": {
    "code": 400,
    "message": "Invallid user count in the request's body!"
  }
}
```
