# Update Username API Documentation

## Overview

This API endpoint allows an authenticated user to update their username. The user must be logged in using a valid authentication token. The API validates the new username for length, allowed characters, and prohibited substrings, and ensures that the username is not already taken before updating it in the database.

## Endpoint

- **URL:** `/user/username`
- **Method:** `POST`

## Description

- **Purpose:** Update the username of the currently authenticated user.
- **Functionality:**
  - Authenticates the request using an authentication token.
  - Verifies that the user's associated `app_name` is allowed.
  - Accepts a new username (`new_username`) from the request body.
  - Validates that the new username is between 4 and 20 characters.
  - Ensures that the username contains only alphanumeric characters and underscores.
  - Checks that the username does not contain any prohibited substrings.
  - Verifies that the new username is not already taken.
  - Updates the user's username in the database.
  - Returns a success message with the updated username.

## Request Headers

Since this endpoint requires authentication, include the following headers:

- **Authorization:** `Bearer {your_authentication_token}`  
  Replace `{your_authentication_token}` with the token obtained during login.
- **Content-Type:** `application/json`

## Request Body Parameters

| Parameter    | Type   | Required | Description                                                                                 |
|--------------|--------|----------|---------------------------------------------------------------------------------------------|
| new_username | String | Yes      | The new username to set for the user. Must be between 4 and 20 characters, can include only alphanumeric characters and underscores, and must not include any prohibited substrings. |

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and set the request method to `POST`.
   - Enter the full URL for the endpoint (e.g., `http://your-domain.com/user/username`).

2. **Configure Request Headers:**
   - Go to the **Headers** tab and add:
     - `Authorization: Bearer {your_authentication_token}`
     - `Content-Type: application/json`

3. **Prepare the Request Body:**
   - Click on the **Body** tab, select `raw`, and choose `JSON` from the dropdown.
   - Use the following example JSON structure:

   ```json
   {
     "new_username": "newUsername123"
   }
