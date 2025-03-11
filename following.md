# Profile Following API

This endpoint retrieves a list of users that a specified user (identified by their username) is following. It also indicates whether the logged-in user (viewer) is following each user in the list.

## Table of Contents
- [Overview](#overview)
- [Endpoint Details](#endpoint-details)
- [Code Explanation](#code-explanation)
- [Request Parameters](#request-parameters)
- [Response Format](#response-format)
- [Error Handling](#error-handling)
- [Testing with Postman](#testing-with-postman)
- [Conclusion](#conclusion)

## Overview

The **Profile Following API** allows clients to fetch the following list of a user by providing their username. The endpoint:
- Authenticates the request (if a viewer is logged in).
- Validates the application context.
- Ensures the username adheres to a strict alphanumeric pattern (with underscores).
- Retrieves the list of connections (users being followed).
- Determines if the logged-in user is following each user in the list.
- Returns the list as a JSON array.

## Endpoint Details

- **URL:** `/profile/following/{username}`
- **HTTP Method:** GET
- **Route Name:** `profile.following`
- **URL Parameter:** `username` – The username of the user whose following list is requested.

## Code Explanation

### Authentication & App Validation
- **Authentication:**  
  The endpoint starts by authenticating the user using the request data. If a user is logged in, their information is stored in the `$viewer` variable; otherwise, it remains `null`. A default application name (`empowerverse`) is used when the user is not authenticated.
  
- **Project Validation:**  
  The provided or default `appName` is validated against allowed projects using `ProjectList::isAllowedProject($appName)`. If invalid, an error JSON is returned with a 400 status.

### Username Validation
- The `username` parameter is validated using a regular expression to ensure it only contains alphanumeric characters and underscores.  
- If the validation fails, the endpoint returns an error message indicating that the username format is invalid.

### Retrieving the User and Their Connections
- The code attempts to retrieve the user from the database using the provided username and processed app name.  
- If the user is not found, it redirects to a `notfound` route.
- The list of connections (followings) is retrieved using the connection helper.
- The IDs of all followee users are extracted for further processing.

### Mutual Following Status
- The private method `getIsFollowingConnectionsMap` is used to determine if the logged-in user (viewer) is following each user in the list.
- It queries the database to build a mapping of followee IDs to a boolean indicating if they are followed by the viewer.

### Response Formatting
- For each connection, the endpoint builds an array containing:
  - `is_following`: A boolean flag indicating if the viewer follows the user.
  - `username`: The followee's username.
  - `first_name`: The followee's first name.
  - `last_name`: The followee's last name.
  - `profile_picture_url`: URL to the followee's profile picture.
- The final response is returned as a JSON array of these objects.

## Request Parameters

### URL Parameter
- **username** (required):  
  The username of the user whose following list is requested.  
  - **Validation:** Must only include alphanumeric characters and underscores.

### Headers
- **Authentication:**  
  Include necessary authentication headers (e.g., Bearer Token) if the request is made by a logged-in user.

## Response Format

### Success Response (HTTP 200)
A successful response returns a JSON array of followee objects. For example:

```json
[
  {
    "is_following": true,
    "username": "johndoe",
    "first_name": "John",
    "last_name": "Doe",
    "profile_picture_url": "https://example.com/path/to/profile.jpg"
  },
  {
    "is_following": false,
    "username": "janedoe",
    "first_name": "Jane",
    "last_name": "Doe",
    "profile_picture_url": "https://example.com/path/to/profile.jpg"
  }
]
