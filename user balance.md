# User Balance API Documentation

## Overview

This API endpoint is used to retrieve the current balance of the authenticated user. It requires the user to be logged in via an authentication token. If the token is missing or invalid, or if the associated `app_name` is not allowed, an error response is returned.

## Endpoint

- **URL:** `/user/balance`
- **Method:** `GET`

## Description

- **Purpose:** Fetch the current balance of the authenticated user.
- **Functionality:**
  - Authenticates the request using an authentication token.
  - Verifies that the user's associated `app_name` is allowed.
  - Returns the user’s balance if authentication and validation are successful.
  - Redirects to a forbidden route or returns an error if authentication fails.

## Request Headers

Since this endpoint requires authentication, include the following header:

- **Authorization:** `Bearer {your_authentication_token}`  
  (Replace `{your_authentication_token}` with the token obtained during login.)

Other required header:

- **Content-Type:** `application/json`

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and set the method to `GET`.
   - Enter the full URL (e.g., `http://your-domain.com/user/balance`).

2. **Configure Request Headers:**
   - Under the **Headers** tab, add:
     - `Authorization: Bearer {your_authentication_token}`
     - `Content-Type: application/json`

3. **Send the Request:**
   - Click the **Send** button.
   - A successful request returns the user's balance.
   - If the token is invalid or missing, you will receive an error response or be redirected.

## Response Format

### Success Response

- **HTTP Status:** `200 OK`
- **Response Body Example:**

  ```json
  {
    "status": "success",
    "balance": 150.75
  }
