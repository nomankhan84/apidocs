# User Picture Upload API Documentation

## Overview

This API endpoint allows an authenticated user to update their profile picture. The endpoint accepts an image file (only `png`, `jpeg`, `jpg`, and `webp` formats are allowed) via a form-data parameter, uploads the image to an S3 bucket, and updates the user's profile with the new picture URL.

## Endpoint

- **URL:** `/user/picture/upload`
- **Method:** `POST`

## Description

- **Purpose:** Update the profile picture for the authenticated user.
- **Functionality:**
  - Authenticates the user using an authentication token.
  - Validates that the user's associated `app_name` is allowed.
  - Expects a file upload in the form-data with the key `avatar`.
  - Checks that the file is valid, readable, and of an allowed MIME type (`image/png`, `image/jpeg`, `image/jpg`, or `image/webp`).
  - Uploads the image to an S3 bucket with a uniquely generated filename.
  - Updates the user's profile with the URL of the uploaded image.
  - Optionally, triggers a reward mechanism if the user is eligible.

## Request Headers

- **Authorization:** `Bearer {your_authentication_token}`  
  *(Replace `{your_authentication_token}` with your actual token.)*
- **Content-Type:** `multipart/form-data`

## Request Body Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| avatar    | File | Yes      | The image file to upload. Must be one of the following types: `png`, `jpeg`, `jpg`, or `webp`. |

## How to Test in Postman

1. **Set Up the Request:**
   - Open Postman and set the method to `POST`.
   - Enter the endpoint URL (e.g., `http://your-domain.com/user/picture/upload`).

2. **Configure Request Headers:**
   - In the **Headers** tab, add:
     - `Authorization: Bearer {your_authentication_token}`

3. **Prepare the Request Body:**
   - Switch to the **Body** tab.
   - Select **form-data**.
   - Add a key named `avatar`, change the type to **File**, and select an image file from your computer.

4. **Send the Request:**
   - Click the **Send** button.
   - If successful, you will receive a JSON response indicating that the profile picture was updated, along with the new picture URL.
   - In case of errors (e.g., missing file, invalid file type, or authentication issues), an appropriate error message will be returned.

## Response Format

### Success Response

- **HTTP Status:** `200 OK`
- **Example:**

  ```json
  {
    "status": "success",
    "message": "Profile picture updated successfully",
    "profile_picture_url": "https://assets.socialverseapp.com/profile/your_generated_picture_name.png"
  }
