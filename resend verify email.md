# UserController API Documentation

This document provides details for the `resendVerificationEmail` endpoint in the UserController. It explains what the API does, how to call it, and how to test it using Postman.

---

## POST /user/verification/resend

### Overview

This endpoint is used to resend a verification email to a user. It:
- **Validates** the provided `app_name` and `mixed` (email or username).
- **Checks** whether the user exists.
- **Ensures** that the user has not already verified their email.
- **Generates** a verification link and sends it to the user's registered email.

If the request fails any of these checks, an appropriate error response is returned.

---

### Endpoint Details

- **HTTP Method:** `POST`
- **Endpoint URL:** `/user/verification/resend`
- **Request Body Parameters:**
  - `mixed` (required, string): The username or email of the user requesting verification.
  - `app_name` (required, string): The application name requesting verification.
- **Authorization:**  
  Not required.

---

### Steps to Test in Postman

1. **Create a New Request:**
   - Open Postman and create a new request.
   - Set the request method to `POST`.

2. **Set the Request URL:**
```
http://your-api-base-url/user/verification/resend
```

3. **Configure Headers:**
- `Content-Type: application/json`

4. **Set the Request Body:**
- **Example JSON Payload (Using Email)**
  ```json
  {
    "mixed": "user@example.com",
    "app_name": "empowerverse"
  }
  ```
- **Example JSON Payload (Using Username)**
  ```json
  {
    "mixed": "johndoe",
    "app_name": "empowerverse"
  }
  ```

5. **Send the Request:**
- Click the **Send** button.
- Observe the JSON response in the response body.

6. **Review the Response:**
- **Successful Response (200 OK):**  
  ```json
  {
    "status": "success",
    "message": "Verification email sent"
  }
  ```
- **Error Responses:**
  - **Missing App Name (400 Bad Request):**  
    ```json
    {
      "status": "failed",
      "message": "App name is required"
    }
    ```
  - **Invalid App Name (400 Bad Request):**  
    ```json
    {
      "status": "failed",
      "message": "Invalid app_name parameter! Kindly contact Empowerverse team"
    }
    ```
  - **Invalid Mixed Parameter (400 Bad Request):**  
    ```json
    {
      "status": "failed",
      "message": "Invalid mixed parameter"
    }
    ```
  - **User Not Found (404 Not Found):**  
    ```json
    {
      "status": "failed",
      "message": "User not found"
    }
    ```
  - **User Already Verified (200 OK):**  
    ```json
    {
      "status": "failed",
      "message": "User is already verified"
    }
    ```

---

### Request Details

- **Body Parameters:**
- `mixed`: The username or email of the user.
- `app_name`: The application name making the request.

- **Headers:**
- `Content-Type: application/json`

- **Additional Considerations:**
- The API verifies whether the user exists before proceeding.
- If the user has already verified their email, the response will indicate so.
- If the user is valid, a verification email is sent with a unique verification link.

---

### Sample Verification Link Sent to User

When successful, the API generates and logs a verification link, formatted as:

