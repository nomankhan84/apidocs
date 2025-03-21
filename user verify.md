# User Verification API Documentation

## Overview

This API endpoint verifies a user's email address by confirming that the provided verification code is correct. The code is generated by taking the MD5 hash of the concatenation of the username and a secret salt (`EMAIL_VERIFICATION_SALT`). Once verified, the user's email is marked as verified, and additional referral-related logic may be executed.

## Endpoint

- **URL:** `/user/verify`
- **Method:** `POST`

## Description

- **Purpose:** To verify a user's email address by confirming the provided verification code.
- **Functionality:**
  - Accepts a username, a verification code, and an optional `app_name` (default is "empowerverse").
  - Checks that the `app_name` is allowed.
  - Validates that the username and code are provided and conform to expected patterns (alphanumeric and underscores).
  - Compares the provided code with the expected code (calculated as `md5(username + EMAIL_VERIFICATION_SALT)`).
  - If the user is not found or the verification fails, an error is returned.
  - If the user is already verified, a message is returned indicating that.
  - If verification is successful, the user's email is marked as verified and their profile is updated with a referral code.
  - Referral-related processes may be triggered based on the application's rules.

## Request Headers

- **Content-Type:** `application/json`

## Request Body Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| username  | String | Yes      | The user's username. Must contain only alphanumeric characters and underscores. |
| code      | String | Yes      | The verification code, generated as `md5(username + EMAIL_VERIFICATION_SALT)`. |
| app_name  | String | No (default: "empowerverse") | The application name. Must be an allowed project. |

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and set the request method to `POST`.
   - Enter the endpoint URL (e.g., `http://your-domain.com/user/verify`).

2. **Configure Request Headers:**
   - Under the **Headers** tab, add:
     - `Content-Type: application/json`

3. **Prepare the Request Body:**
   - Select the **Body** tab, choose **raw**, and set the format to **JSON**.
   - Use a JSON structure similar to the example below:

   ```json
   {
     "username": "john_doe",
     "code": "5f4dcc3b5aa765d61d8327deb882cf99", // Example MD5 hash; replace with actual code
     "app_name": "empowerverse"
   }
