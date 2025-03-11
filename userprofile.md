# User Profile API

This Symfony controller endpoint is used to fetch a public profile for a given username. It validates the request, retrieves user details based on the provided username and app name, and returns an extensive set of public profile data.

## Endpoint Overview

- **URL:** `/profile/{username}`
- **Method:** `GET`
- **Purpose:** To retrieve public profile information of a user by their username.
- **Authentication:**  
  The endpoint attempts to authenticate the request. If authentication is successful, additional context such as viewer-specific information (e.g., follow status) may be returned. If not authenticated, the endpoint still processes the request using a default app name.

## How It Works

1. **Authentication & App Name Resolution:**
   - The controller starts by authenticating the request.
   - It extracts the logged-in user (referred to as the *viewer*) and determines the `appName` either from the authentication context or via a request parameter (defaulting to `"empowerverse"`).
   - It verifies if the `appName` is permitted using a project list check. If not allowed, an error is returned.

2. **Username Validation:**
   - The provided username (from the URL) is trimmed and its length is checked to ensure it falls within acceptable limits (3 to 50 characters).
   - If the username is too short or too long, the request is redirected to a "not found" route.

3. **User Lookup:**
   - Depending on the `appName`, the system searches for a user record with the matching username (and app name if necessary).
   - If no user is found, a JSON error response is returned.

4. **Profile Data Assembly:**
   - Once the user is found, the controller extracts the user's profile.
   - It builds an array containing public profile data including social media links (Instagram, TikTok, YouTube, Twitter), wallet information, online status, user identity details, referral codes, and statistics (like follower and post counts).
   - Additional checks are performed:
     - If the *viewer* is the same as the profile owner, referral codes are generated (if missing) and a daily login task is dispatched.
     - If the *viewer* is different from the profile owner, the response may include information such as whether the viewer is following or has blocked the user, and the existence of a chat session between them.

5. **Response:**
   - The assembled public profile data is returned as a JSON response.

## Testing with Postman

To test the `/profile/{username}` endpoint using Postman, follow these steps:

1. **Setup the Request:**
   - **Method:** `GET`
   - **URL:**  
     Replace `{username}` with the target user's username. For example:  
     `http://<your-domain>/profile/johndoe`
   - If you need to specify an application name other than the default, add a query parameter:  
     `http://<your-domain>/profile/johndoe?app_name=yourAppName`

2. **Headers:**
   - Add a header for content type:  
     `Accept: application/json`
   - Include any authentication headers required by your API if you are testing with an authenticated user.

3. **Sending the Request:**
   - Click the **Send** button.
   - **Expected Outcomes:**
     - **Success:**  
       You will receive a JSON response with detailed public profile data of the requested user.
     - **Error:**  
       If the username is invalid, the user is not found, or the `appName` is not permitted, the API will return an appropriate error message along with an HTTP status code (e.g., 400 for invalid parameters or 404 for user not found).

## Additional Notes

- **Referral Code Generation:**  
  If the authenticated user (viewer) does not have a referral code, one will be generated, saved, and then included in the response.
  
- **Logging & Background Tasks:**  
  The controller logs key actions (such as successful login and profile views) and dispatches a daily login task if the profile owner and the viewer are the same.

This endpoint is designed to serve both authenticated and unauthenticated users with detailed profile information, while ensuring proper validation and tailored data based on the context of the request.

---
