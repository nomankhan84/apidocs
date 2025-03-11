# Profile Block API

This endpoint allows an authenticated user to block another user by their username. When a block is initiated, the endpoint also removes any existing follow relationship between the blocker and the blocked user.

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

The **Profile Block API** enables a logged-in user to block another user by specifying their username. It ensures that:
- The requester is authenticated.
- The app context is valid.
- The target user exists and is not the requester themselves.
- A block record is created if it does not already exist.
- Any existing follow relationship between the two users is removed, updating the follower/following counts accordingly.

## Endpoint Details

- **URL:** `/profile/block/{username}`
- **HTTP Method:** POST
- **Route Name:** `profile.block`
- **URL Parameter:** `username` – The username of the user to be blocked.

## Code Explanation

### Authentication & Project Validation
- **Authentication:**  
  The endpoint starts by authenticating the incoming request. If authentication fails, the user is redirected to a `forbidden` route.
  
- **Project Validation:**  
  The application name (`appName`) is extracted and validated against allowed projects using `ProjectList::isAllowedProject()`. If invalid, a JSON error response is returned.

### User Retrieval and Validations
- **Blocked User Retrieval:**  
  The user to be blocked is retrieved from the repository using the provided username and processed application name.
  
- **Self-block Prevention:**  
  The code checks if the authenticated user is attempting to block themselves and returns an error if so.
  
- **Existing Block Check:**  
  Before creating a new block, the code checks if a blocking relationship already exists. If found, it returns a success message indicating that the user is already blocked.

### Creating the Block Relationship
- **Block Creation:**  
  A new block entity is instantiated with the authenticated user as the blocker and the target user as the blocked user.
  
- **Persistence:**  
  The new block record is persisted to the database using Doctrine's Entity Manager.

### Removing Follow Relationship
- **Follow Relationship Check:**  
  The code checks if the blocked user is currently following the blocker.
  
- **Deletion:**  
  If such a follow relationship exists, it is removed. Additionally, the follower's following count and the followee's follower count are decremented and persisted to ensure data consistency.

### Final Response
- **Success Message:**  
  After all operations, the endpoint returns a JSON response confirming that the user has been blocked successfully.

## Request Parameters

### URL Parameter
- **username** (required):  
  The username of the user you want to block.  
  - **Validation:** Must be a valid username corresponding to an existing user.

### Headers
- **Authentication:**  
  Include the necessary authentication headers (e.g., Bearer Token or session cookies) to ensure the request is made by a logged-in user.

## Response Format

### Success Response

- **HTTP Status:** 200
- **Body:**

  ```json
  {
      "status": "success",
      "message": "User blocked successfully!"
  }
