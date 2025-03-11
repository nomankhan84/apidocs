# UserController API Documentation

This document provides details for the `getUserPosts` endpoint in the UserController. It explains what the API does, how to call it, and how to test it using Postman.

---

## GET /users/{username}/posts

### Overview

This endpoint retrieves a paginated list of posts for a specified user. It performs the following actions:
- **Authenticates** the requester.
- **Validates** that the authenticated user's application is allowed to access this endpoint.
- **Verifies** the provided page number.
- **Fetches** posts for the given username while excluding posts blocked for the requester.

If any validations fail (e.g., invalid app name, invalid page parameter, or user not found), the API returns an appropriate error response.

---

### Endpoint Details

- **HTTP Method:** `GET`
- **Endpoint URL:** `/users/{username}/posts`  
  Replace `{username}` with the target user's username.
- **Query Parameter:**
  - **page** (required, number): The page number for pagination. Must be numeric and greater than or equal to 1.
- **Authorization:**  
  Yes – include an authorization header (e.g., `Authorization: Bearer <access_token>`) if your API requires it.

---

### Steps to Test in Postman

1. **Create a New Request:**
   - Open Postman and create a new request.
   - Set the request method to `GET`.

2. **Set the Request URL:**
   - Enter the URL:  
     ```
     http://your-api-base-url/users/{username}/posts?page=1
     ```
   - Replace `{username}` with the desired user's username and adjust the `page` query parameter as needed.

3. **Configure Headers:**
   - Add the header:  
     ```
     Authorization: Bearer <your_access_token>
     ```
   - Optionally, add `Content-Type: application/json` if necessary.

4. **Send the Request:**
   - Click the **Send** button.
   - Observe the JSON response in the response body.

5. **Review the Response:**
   - **Successful Response (200 OK):**  
     You should receive a JSON object containing pagination details and an array of posts.
   - **Error Responses:**
     - **Invalid App Name (400 Bad Request):**  
       ```json
       {
         "status": "failed",
         "message": "Invalid app_name parameter! Kindly contact Empowerverse team"
       }
       ```
     - **Invalid Page Parameter:**  
       ```json
       {
         "status": "failed",
         "message": "Invalid page parameter"
       }
       ```
     - **User Not Found (404 Not Found):**  
       ```json
       {
         "status": "error",
         "message": "User not found"
       }
       ```

---

### Request Details

- **URL Parameter:**
  - `username`: The username of the user whose posts are being retrieved.

- **Query Parameter:**
  - `page`: A numeric value representing the page number to retrieve (e.g., `page=1`).

- **Headers:**
  - `Authorization`: Required if your API needs authentication. Example:  
    `Authorization: Bearer <your_access_token>`

- **Additional Considerations:**
  - The API checks whether the authenticated user's application (via the access token) is allowed.
  - The `page` parameter must be provided and must be a valid number.
  - Posts are filtered to exclude those marked as deleted (`is_deleted: false`) and unprocessed posts (`is_processed: true`).
  - If the logged-in user has blocked certain posts, those will not be included in the response.

---

### Sample Success Response

```json
{
  "page": 1,
  "records_per_page": 12,
  "max_page_size": 12,
  "page_size": 10,
  "posts": [
    {
      "id": 123,
      "title": "Post title",
      "content": "Post content",
      "created_at": "2025-03-09T12:34:56Z",
      "user": "johndoe"
    }
    // ... additional post objects
  ]
}
