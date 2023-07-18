# Zenith.Care API Documentation

This document provides a detailed description of the Zenith.Care API endpoints. The Zenith.Care API enables developers to interact with the Zenith.Care platform programmatically.

## Overview

The Zenith.Care API is organized around REST principles. Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. All communication with the API is done using JSON.

## Base URL

All API endpoints are relative to the base URL:
https://api.zenith.care


## Authentication

Most Zenith.Care API endpoints require authentication. We use bearer token authentication. To authenticate an API request, you should provide your API token in the `Authorization` header of the HTTP request:

Authorization: Bearer YOUR_API_TOKEN

Replace `YOUR_API_TOKEN` with your actual API token.

## Errors

The Zenith.Care API uses standard HTTP status codes to indicate the success or failure of an API request. In general, codes in the `2xx` range indicate success, codes in the `4xx` range indicate an error that resulted from the provided information (e.g., a required parameter was missing), and codes in the `5xx` range indicate a server error.

If an error occurs, the API will return an error object. This object will include an `error` field with a short description of the error, and optionally an `error_description` field with a more detailed explanation.

```json
{
    "error": "invalid_request",
    "error_description": "The request is missing a required parameter."
}
```

Status Codes

The Zenith.Care API uses the following HTTP status codes:

**200 OK**: The request was successful.

**201 Created**: The request was successful, and a new resource was created as a result.

**204 No Content**: The request was successful, but there is no representation to return (i.e., the response is empty).

**400 Bad Request**: The request could not be understood or was missing required parameters.

**401 Unauthorized**: Authentication failed or was not provided.

**403 Forbidden**: Authentication succeeded, but the authenticated user does not have access to the requested resource.

**404 Not Found**: The requested resource could not be found.

**429 Too Many Requests**: Too many requests have been sent in a given amount of time.

**500 Internal Server Error**: An error occurred on the server.


## Rate Limiting

Rate limiting is a technique for preventing abuse and ensuring fair usage of the Zenith.Care API. We impose limits on the number of API requests that can be made in a certain time frame.

The standard rate limit for the Zenith.Care API is 1000 requests per hour per IP address. This applies to all endpoints, regardless of the HTTP method used.

When a limit is exceeded, the API will respond with a `429 Too Many Requests` HTTP status code and a JSON body detailing the error:

```json
{
    "message": "Rate limit exceeded. Please wait and try your request again later."
}
```

To help developers track their usage, the API includes the following headers in every response:

X-RateLimit-Limit: The maximum number of requests you're permitted to make per hour.
X-RateLimit-Remaining: The number of requests remaining in the current rate limit window.
X-RateLimit-Reset: The time at which the current rate limit window resets in UTC epoch seconds.

We encourage developers to handle 429 Too Many Requests responses in their API client code. A good strategy is to implement an exponential backoff for your retry logic and to respect the Retry-After header, if present.


## Endpoints

### `/health-data`

The `/health-data` endpoint is used to manage a user's health data in the Zenith.Care platform.

#### GET `/health-data/{userId}`

This endpoint retrieves the health data for a specific user.

- **Parameters**
  - `userId` (path, required): The ID of the user whose health data you want to retrieve.

- **Example Request**

```
GET /health-data/12345
Authorization: Bearer YOUR_API_TOKEN
```


- **Example Response**

```json
{
  "userId": "12345",
  "weight": 70,
  "height": 175,
  "sleepHours": 8,
  "steps": 10000,
  "heartRate": 70
}
```

Error Messages
404 Not Found: If there is no health data for the specified userId, the API will return a 404 Not Found status code.
POST /health-data

This endpoint allows a user to submit their health data to the Zenith.Care platform.


**Request Body**

**userId (required)**: The ID of the user.

**weight (optional)**: The user's weight in kilograms.

**height (optional)**: The user's height in centimeters.

**sleepHours (optional)**: The number of hours the user sleeps per night.

**steps (optional)**: The number of steps the user takes per day.

**heartRate (optional)**: The user's average heart rate.


Example Request

```POST /health-data
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json

{
  "userId": "12345",
  "weight": 70,
  "height": 175,
  "sleepHours": 8,
  "steps": 10000,
  "heartRate": 70
}
```

**Example response**

```{
  "message": "Health data submitted successfully."
}
```

Error Messages
400 Bad Request: If the request body is missing required fields or includes invalid data, the API will return a 400 Bad Request status code.

### `/user-rewards`

The `/user-rewards` endpoint is used to manage a user's rewards in the Zenith.Care platform.

#### GET `/user-rewards/{userId}`

This endpoint retrieves the rewards for a specific user.

- **Parameters**
  - `userId` (path, required): The ID of the user whose rewards you want to retrieve.

- **Example Request**

```GET /user-rewards/12345
Authorization: Bearer YOUR_API_TOKEN
```


- **Example Response**

```json
{
  "userId": "12345",
  "rewards": [
    {
      "type": "step",
      "description": "10000 steps walked",
      "reward": 10
    },
    {
      "type": "sleep",
      "description": "8 hours of sleep",
      "reward": 5
    }
  ]
}
```

Error Messages

**404 Not Found**: If there are no rewards for the specified userId, the API will return a 404 Not Found status code.

POST /user-rewards

This endpoint allows a user to claim a reward on the Zenith.Care platform.

Request Body

**userId (required)**: The ID of the user.

**rewardType (required)**: The type of reward the user is claiming (e.g., "step", "sleep").

Example Request

```
POST /user-rewards
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json

{
  "userId": "12345",
  "rewardType": "step"
}
```

**Example response**:

```{
  "message": "Reward claimed successfully."
}
```

Error Messages

**400 Bad Request**: If the request body is missing required fields or includes invalid data, the API will return a 400 Bad Request status code.


### `/dao-proposals`

The `/dao-proposals` endpoint is used to manage proposals in the Zenith.Care DAO (Decentralized Autonomous Organization).

#### GET `/dao-proposals`

This endpoint retrieves all the proposals in the DAO.

- **Example Request**

```GET /dao-proposals
Authorization: Bearer YOUR_API_TOKEN
```


- **Example Response**

```json
[  {    "id": 1,    "title": "Proposal 1",    "description": "This is the first proposal.",    "status": "active"  },  {    "id": 2,    "title": "Proposal 2",    "description": "This is the second proposal.",    "status": "closed"  }]
```

POST /dao-proposals

This endpoint allows a user to submit a new proposal to the DAO.

Request Body

**title (required)**: The title of the proposal.
**description (required)**: A detailed description of the proposal.

Example Request

POST /dao-proposals
**Authorization**: Bearer YOUR_API_TOKEN

**Content-Type**: application/json

```{
  "title": "New Proposal",
  "description": "This is a new proposal."
}
```

**Example response**

```{
  "id": 3,
  "title": "New Proposal",
  "description": "This is a new proposal.",
  "status": "pending"
}
```

Error Messages

**400 Bad Request** If the request body is missing required fields or includes invalid data, the API will return a 400 Bad Request status code.
PUT /dao-proposals/{proposalId}

This endpoint allows a user to vote on a proposal in the DAO.

Parameters

**proposalId (path, required)**: The ID of the proposal to vote on.

Request Body

**userId (required)**: The ID of the user.

**vote (required)**: The user's vote (either "yes" or "no").

Example Request

```PUT /dao-proposals/1
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json

{
  "userId": "12345",
  "vote": "yes"
}
```

Example Response

```
{
  "message": "Vote submitted successfully."
}
```

Error Messages

**400 Bad Request**: If the request body is missing required fields or includes invalid data, the API will return a 400 Bad Request status code.

**404 Not Found**: If the specified proposalId does not exist, the API will return a 404 Not Found status code.


### `/supplements`

The `/supplements` endpoint is used to manage a user's personalized supplements in the Zenith.Care platform.

#### GET `/supplements/{userId}`

This endpoint retrieves the supplement recommendations for a specific user.

- **Parameters**
  - `userId` (path, required): The ID of the user whose supplement recommendations you want to retrieve.

- **Example Request**

```GET /supplements/12345
Authorization: Bearer YOUR_API_TOKEN
```


- **Example Response**

```json
{
  "userId": "12345",
  "supplements": [
    {
      "name": "Vitamin D",
      "dosage": "1000 IU",
      "frequency": "daily"
    },
    {
      "name": "Omega-3",
      "dosage": "1000 mg",
      "frequency": "daily"
    }
  ]
}
```

Error Messages

**404 Not Found**: If there are no supplement recommendations for the specified userId, the API will return a 404 Not Found status code.
POST /supplements

This endpoint allows a user to submit their supplement preferences to the Zenith.Care platform.

Request Body

**userId (required)**: The ID of the user.

**supplements (required)**: An array of supplement preferences. Each object in the array should have a name (the name of the supplement) and a preference (the user's preference for that supplement, e.g., "yes", "no", "maybe").

Example Request

POST /supplements
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json

POST /supplements

**Authorization**: Bearer YOUR_API_TOKEN

**Content-Type**: application/json

```{
  "userId": "12345",
  "supplements": [
    {
      "name": "Vitamin D",
      "preference": "yes"
    },
    {
      "name": "Omega-3",
      "preference": "no"
    }
  ]
}
```

Example Response

```{
  "message": "Supplement preferences submitted successfully."
}
```

Error Messages

**400 Bad Request**: If the request body is missing required fields or includes invalid data, the API will return a 400 Bad Request status code.


## Error Handling

The Zenith.Care API uses standard HTTP status codes to indicate the success or failure of an API request. In case of an error, the API responds with a relevant HTTP status code and a JSON body that contains more information about the error.


**Here's an example of an error response:**

```json
{
    "error": "invalid_request",
    "message": "The request is missing a required parameter."
}
```
In this example, the error field contains a short, machine-readable description of the error, and the message field contains a human-readable description of the problem.


**Here's a list of possible error codes and their meanings**:

**invalid_request**: The request is malformed or missing required parameters.

**unauthorized**: The request requires authentication and either failed to authenticate or no authentication information was provided.

**forbidden**: The authenticated user does not have access to the requested resource.

**not_found**: The requested resource could not be found.

**rate_limit_exceeded**: The rate limit of the API has been exceeded.

**internal_server_error**: An error occurred on the server.

Each error response will also include the HTTP status code in the response status line. We recommend that client applications check the HTTP status code in the response to determine the type of error and handle it accordingly.
