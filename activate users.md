# Active Users API Documentation

## Overview

This API endpoint retrieves a list of active users within the last 30 days for the authenticated user’s application. It excludes the currently logged-in user and filters out users whose usernames (or first/last names) contain certain substrings (e.g., "test", "dummy", "testing"). The result is paginated and the list is randomized before being returned.

## Endpoint

- **URL:** `/users/active`
- **Method:** `GET`

## Description

- **Purpose:** To fetch a paginated list of active users who have logged in within the last 30 days.
- **Functionality:**
  - Authenticates the request using an authentication token.
  - Validates the `app_name` associated with the logged-in user.
  - Accepts an optional query parameter `page` (defaults to 1) to support pagination.
  - Excludes the logged-in user and users whose names contain substrings such as "test", "dummy", or "testing".
  - Retrieves users ordered by their last login date (most recent first).
  - Randomizes the list of users before sending the response.

## Request Headers

Since this endpoint requires authentication, include the following headers:

- **Authorization:** `Bearer {your_authentication_token}`  
  *(Replace `{your_authentication_token}` with your actual token.)*
- **Content-Type:** `application/json`

## Query Parameters

| Parameter | Type    | Required | Description                                         |
|-----------|---------|----------|-----------------------------------------------------|
| page      | Number  | No       | The page number for pagination. Defaults to 1. Must be a valid numeric value. |

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and set the request method to `GET`.
   - Enter the full URL for the endpoint (e.g., `http://your-domain.com/users/active`).

2. **Configure Request Headers:**
   - In the **Headers** tab, add:
     - `Authorization: Bearer {your_authentication_token}`
     - `Content-Type: application/json`

3. **Set Query Parameters:**
   - Click on the **Params** tab.
   - Add the key `page` with a numeric value (for example, `1`) if you want to retrieve a specific page.

4. **Send the Request:**
   - Click the **Send** button.
   - You will receive a JSON response with the active users list and pagination details.

## Response Format

### Success Response

- **HTTP Status:** `200 OK`
- **Example Response:**

  ```json
  {
    "status": "success",
    "message": "Active users fetched successfully",
    "page": 1,
    "max_page_size": 20,
    "page_size": 15,
    "users": [
      {
        "is_following": false,
        "username": "jane_doe",
        "first_name": "Jane",
        "last_name": "Doe",
        "profile_picture_url": "https://ui-avatars.com/api/?size=128&name=Jane+Doe&color=fff&background=10b981"
      },
      {
        "is_following": true,
        "username": "john_smith",
        "first_name": "John",
        "last_name": "Smith",
        "profile_picture_url": "https://ui-avatars.com/api/?size=128&name=John+Smith&color=fff&background=10b981"
      }
      // ... more user records
    ]
  }
