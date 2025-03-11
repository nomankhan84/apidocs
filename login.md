# User Login API Documentation

## Overview

This API endpoint is used to authenticate a user. It accepts a single parameter (`mixed`) that can be either a username or an email address, along with a password and an optional application name (`app_name`, defaulting to "empowerverse"). The API validates the credentials, ensures the user's email is verified, updates login activity, and returns an authentication token along with detailed user information.

## Endpoint

- **URL:** `/user/login`
- **Method:** `POST`

## Description

- **Purpose:** To authenticate a user using either their username or email address combined in the `mixed` parameter.
- **Functionality:**
  - Validates the `app_name` against allowed projects.
  - Ensures the password meets a minimum length of 6 characters.
  - Determines whether `mixed` is an email (if it contains "@" and ".") or a username.
  - Validates the email format if applicable.
  - Searches for the user record using the provided credentials (username/email and password).
  - Checks if the user’s email is verified.
  - Updates the user's last login timestamp and manages login activity (including push notifications for certain apps).
  - Returns an authentication token along with user details upon successful login.

## Request Headers

- **Content-Type:** `application/json`

## Request Body Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| mixed     | String | Yes      | A combined parameter representing either the user's email or username. The API treats it as an email if it contains "@" and ".", otherwise as a username. |
| password  | String | Yes      | The user's password (minimum 6 characters). |
| app_name  | String | No (default: "empowerverse") | The application name. This parameter is used to validate the project and may trigger additional behaviors such as push notifications. |

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and select `POST` as the method.
   - Enter the endpoint URL (e.g., `http://your-domain.com/user/login`).

2. **Configure Request Headers:**
   - Under the Headers tab, add:
     - `Content-Type: application/json`

3. **Prepare the Request Body:**
   - Click on the Body tab, select `raw`, and choose `JSON` from the dropdown.
   - Use the following example JSON structure:

   ```json
   {
     "mixed": "john_doe",  // Alternatively, use "john@example.com" for email-based login.
     "password": "secret123",
     "app_name": "empowerverse"
   }
