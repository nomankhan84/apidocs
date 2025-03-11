# API Endpoints Documentation

This document provides a detailed explanation of the user management API endpoints. Each endpoint is described with its HTTP method, purpose, process, required request parameters, and expected responses.

---

## Table of Contents

1. [User Balance](#user-balance)
2. [Update Username](#update-username)
3. [Upload Profile Picture](#upload-profile-picture)
4. [Get Profile Picture](#get-profile-picture)
5. [Delete User](#delete-user)
6. [Get User Data](#get-user-data)
7. [Verify Email](#verify-email)
8. [Reset Endpoint](#reset-endpoint)
9. [Logout](#logout)
10. [Logout Everywhere](#logout-everywhere)
11. [Active Users](#active-users)
12. [Verify User Profile](#verify-user-profile)
13. [Verified Users](#verified-users)
14. [Get All Users](#get-all-users)
15. [User Posts](#user-posts)
16. [Resend Verification Email](#resend-verification-email)
17. [Validate Username and Email](#validate-username-and-email)
18. [Create User](#create-user)
19. [User Login](#user-login)

---

## 1. User Balance

- **Endpoint:** `/user/balance`
- **Method:** GET
- **Description:** Retrieves the balance of the authenticated user.
- **Process:**
  - Authenticates the user using the authenticator service.
  - Checks if the user is authenticated and if the project is allowed.
  - Returns the user's balance in JSON format.
- **Request:**
  - **Headers:** Authentication token must be provided.
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "balance": <user_balance> }
    ```
  - **Failure:** Redirects to the forbidden route or returns an error message.

---

## 2. Update Username

- **Endpoint:** `/user/username`
- **Method:** POST
- **Description:** Updates the username of the authenticated user.
- **Process:**
  - Authenticates the user.
  - Validates the new username.
  - Checks if the new username is already taken or contains prohibited words.
  - Updates the username and persists the changes.
- **Request:**
  - **Headers:** Authentication token.
  - **Body:**  
    ```json
    { "new_username": "<new_username>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Username updated successfully", "username": "<new_username>" }
    ```
  - **Failure:** Returns an error message.

---

## 3. Upload Profile Picture

- **Endpoint:** `/user/picture/upload`
- **Method:** POST
- **Description:** Uploads a profile picture for the authenticated user.
- **Process:**
  - Authenticates the user.
  - Validates the uploaded file.
  - Uploads the file to an S3 bucket.
  - Updates the user's profile picture URL and persists the changes.
- **Request:**
  - **Headers:** Authentication token.
  - **Body:** Form-data with the file field named `avatar`.
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Profile picture updated successfully", "profile_picture_url": "<profile_picture_url>" }
    ```
  - **Failure:** Returns an error message.

---

## 4. Get Profile Picture

- **Endpoint:** `/user/picture`
- **Method:** GET
- **Description:** Retrieves the profile picture URL of the authenticated user.
- **Process:**
  - Authenticates the user.
  - Returns the user's profile picture URL in JSON format.
- **Request:**
  - **Headers:** Authentication token.
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "profile_picture_url": "<profile_picture_url>" }
    ```
  - **Failure:** Returns an error message.

---

## 5. Delete User

- **Endpoint:** `/user`
- **Method:** DELETE
- **Description:** Deletes a user (soft delete).
- **Process:**
  - Authenticates the user.
  - Checks if the user has admin privileges.
  - Deletes the specified user and persists the changes.
- **Request:**
  - **Headers:** Authentication token.
  - **Body:**  
    ```json
    { "username": "<username_to_delete>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "user deleted successfully" }
    ```
  - **Failure:** Returns an error message.

---

## 6. Get User Data

- **Endpoint:** `/user/data`
- **Method:** GET
- **Description:** Retrieves data of the authenticated user.
- **Process:**
  - Authenticates the user.
  - Returns the user's data in JSON format.
- **Request:**
  - **Headers:** Authentication token.
- **Response:**
  - **Success:**  
    ```json
    { "username": "<username>", "id": "<user_id>" }
    ```
  - **Failure:** Returns an error message.

---

## 7. Verify Email

- **Endpoint:** `/user/verify`
- **Method:** POST
- **Description:** Verifies the user's email using a verification code.
- **Process:**
  - Validates the verification code.
  - Updates the user's email verification status and persists the changes.
- **Request:**
  - **Body:**  
    ```json
    { "username": "<username>", "code": "<verification_code>", "app_name": "<app_name>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "<username>, was verified!" }
    ```
  - **Failure:** Returns an error message.

---

## 8. Reset Endpoint

- **Endpoint:** `/user/reset`
- **Method:** POST
- **Description:** Redirects to a not found route (not in use).
- **Process:**  
  - Redirects to the not found route.
- **Request:** None.
- **Response:**  
  - Redirects to the not found route.

---

## 9. Logout

- **Endpoint:** `/user/logout`
- **Method:** POST
- **Description:** Logs out the authenticated user.
- **Process:**
  - Authenticates the user.
  - Deletes the user's authentication token.
- **Request:**
  - **Headers:** Authentication token.
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "<first_name>, you were logged out!" }
    ```
  - **Failure:** Returns an error message.

---

## 10. Logout Everywhere

- **Endpoint:** `/user/logout-everywhere`
- **Method:** POST
- **Description:** Logs out the authenticated user from all sessions.
- **Process:**
  - Authenticates the user.
  - Deletes all authentication tokens of the user.
- **Request:**
  - **Headers:** Authentication token.
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "<first_name>, you were logged out from everywhere!" }
    ```
  - **Failure:** Returns an error message.

---

## 11. Active Users

- **Endpoint:** `/users/active`
- **Method:** GET
- **Description:** Retrieves a list of active users.
- **Process:**
  - Authenticates the user.
  - Retrieves users who have logged in within the last 30 days.
  - Returns the list of active users in JSON format.
- **Request:**
  - **Headers:** Authentication token.
  - **Query Parameters:** `page` (optional)
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Active users fetched successfully", "page": <page>, "max_page_size": <max_page_size>, "page_size": <page_size>, "users": [<user_data>] }
    ```
  - **Failure:** Returns an error message.

---

## 12. Verify User Profile

- **Endpoint:** `/user/profile/verify`
- **Method:** POST
- **Description:** Verifies a user's profile.
- **Process:**
  - Authenticates the user.
  - Checks if the user has admin privileges.
  - Verifies the specified user's profile and persists the changes.
- **Request:**
  - **Headers:** Authentication token.
  - **Body:**  
    ```json
    { "username": "<username_to_verify>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "<username> verified successfully!" }
    ```
  - **Failure:** Returns an error message.

---

## 13. Verified Users

- **Endpoint:** `/user/profile/verified`
- **Method:** GET
- **Description:** Retrieves a list of verified users.
- **Process:**
  - Authenticates the user.
  - Retrieves users with verified profiles.
  - Returns the list of verified users in JSON format.
- **Request:**
  - **Headers:** Authentication token.
  - **Query Parameters:** `page` (optional), `page_size` (optional)
- **Response:**
  - **Success:**  
    ```json
    { "page": <page>, "max_page_size": <max_page_size>, "page_size": <page_size>, "users": [<user_data>] }
    ```
  - **Failure:** Returns an error message.

---

## 14. Get All Users

- **Endpoint:** `/users/get_all`
- **Method:** GET
- **Description:** Retrieves a list of all users.
- **Process:**
  - Authenticates the user.
  - Checks if the user has admin privileges.
  - Retrieves all users with pagination.
  - Returns the list of users in JSON format.
- **Request:**
  - **Headers:** Authentication token.
  - **Query Parameters:** `page` (optional), `page_size` (optional)
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Users fetched successfully", "page": <page>, "max_page_size": <max_page_size>, "page_size": <page_size>, "users": [<user_data>] }
    ```
  - **Failure:** Returns an error message.

---

## 15. User Posts

- **Endpoint:** `/users/{username}/posts`
- **Method:** GET
- **Description:** Retrieves posts of a specific user.
- **Process:**
  - Authenticates the user.
  - Validates the requested page parameter.
  - Retrieves posts of the specified user with pagination.
  - Returns the list of posts in JSON format.
- **Request:**
  - **Headers:** Authentication token.
  - **Query Parameters:** `page` (optional)
- **Response:**
  - **Success:**  
    ```json
    { "page": <page>, "records_per_page": 12, "max_page_size": 12, "page_size": <page_size>, "posts": [<post_data>] }
    ```
  - **Failure:** Returns an error message.

---

## 16. Resend Verification Email

- **Endpoint:** `/user/verification/resend`
- **Method:** POST
- **Description:** Resends the verification email to the user.
- **Process:**
  - Validates the mixed parameter (email or username).
  - Retrieves the user by email or username.
  - Sends a verification email to the user.
- **Request:**
  - **Body:**  
    ```json
    { "mixed": "<email_or_username>", "app_name": "<app_name>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Verification email sent" }
    ```
  - **Failure:** Returns an error message.

---

## 17. Validate Username and Email

- **Endpoint:** `/user/validate`
- **Method:** POST
- **Description:** Validates the username and email.
- **Process:**
  - Validates the mixed parameter (email or username).
  - Checks if the email or username is already in use.
  - Returns the validation result in JSON format.
- **Request:**
  - **Body:**  
    ```json
    { "mixed": "<email_or_username>", "app_name": "<app_name>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Username and email are valid" }
    ```
  - **Failure:** Returns an error message.

---

## 18. Create User

- **Endpoint:** `/user/create`
- **Method:** POST
- **Description:** Creates a new user.
- **Process:**
  - Validates the input parameters.
  - Checks if the username or email is already in use.
  - Creates a new user and persists the changes.
  - Sends a verification email to the user.
- **Request:**
  - **Body:**  
    ```json
    { "first_name": "<first_name>", "last_name": "<last_name>", "username": "<username>", "password": "<password>", "email": "<email>", "app_name": "<app_name>", "referral_code": "<referral_code>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    { "status": "success", "message": "Email verification sent to <email>!" }
    ```
  - **Failure:** Returns an error message.

---

## 19. User Login

- **Endpoint:** `/user/login`
- **Method:** POST
- **Description:** Logs in the user.
- **Process:**
  - Validates the input parameters.
  - Authenticates the user by email or username and password.
  - Generates an authentication token for the user.
  - Returns the user's data and authentication token in JSON format.
- **Request:**
  - **Body:**  
    ```json
    { "mixed": "<email_or_username>", "password": "<password>", "app_name": "<app_name>" }
    ```
- **Response:**
  - **Success:**  
    ```json
    {
      "status": "success",
      "balance": <balance>,
      "token": "<token>",
      "username": "<username>",
      "email": "<email>",
      "first_name": "<first_name>",
      "last_name": "<last_name>",
      "isVerified": <isVerified>,
      "role": "<role>",
      "owner_id": "<owner_id>",
      "referral_code": "<referral_code>",
      "wallet_address": "<wallet_address>",
      "has_wallet": <has_wallet>,
      "last_login": "<last_login>",
      "profile_picture_url": "<profile_picture_url>"
    }
    ```
  - **Failure:** Returns an error message.

---

## Conclusion

This documentation covers a wide range of endpoints that manage user-related operations including authentication, profile management, balance retrieval, and more. Each endpoint has been detailed with its process, expected inputs, and outputs to facilitate easy integration and troubleshooting.

For further information or contributions, please refer to the project's contribution guidelines.
