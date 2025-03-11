User Notification API
This endpoint is designed to retrieve notifications for a logged-in user. It performs authentication, verifies the project name, supports pagination, and formats notifications with detailed user and actor information.

Table of Contents
Overview
Endpoint Details
Code Explanation
Request Parameters
Response Format
Error Handling
Testing with Postman
Conclusion
Overview
The code defines a GET endpoint at /user/notification that:

Authenticates the user.
Validates if the user is part of an allowed project.
Retrieves notifications from the database with support for pagination.
Formats the notification details including user and actor information.
Endpoint Details
URL: /user/notification
Method: GET
Route Name: user.notifications.get
Code Explanation
Authentication:

The endpoint starts by authenticating the user with the request.
If authentication fails (i.e., no user info is found), the user is redirected to a forbidden route.
Project Validation:

Once authenticated, it extracts the user and the application name.
It checks if the provided application name is in the list of allowed projects. If not, it returns a JSON error message with a 400 status code.
Pagination:

The code retrieves page_size and page query parameters from the request.
Default values are used if these parameters are missing.
It also validates that the provided parameters are numeric and calculates the number of records to skip for pagination.
Notification Retrieval:

Notifications are fetched from the repository based on the criteria (user, project, not deleted).
If the user has an admin role (role === "A") and the app_name query parameter is set to "all", notifications for all projects are retrieved.
Formatting Notification Data:

For each notification, both the user and the actor (the entity causing the notification) are formatted.
Each user's model includes an ID, username, first name, last name, full name, and a profile URL.
Additionally, if the user is different from the actor, it checks if one is following the other.
Response:

The endpoint returns a JSON response containing:
Status of the request.
Current page and page size information.
A list of notifications with details.
Request Parameters
page_size (optional): Number of notifications per page. Default is defined by self::RESPONSE_PER_PAGE.
page (optional): The page number to retrieve. Default is defined by self::PAGE_NUMBER.
app_name (optional): When provided, it determines the scope of notifications. For admin users (role === "A"), setting this parameter to "all" retrieves notifications across all projects.
Response Format
The successful response is a JSON object containing:

status: Indicates the success (e.g., "success").
page: The current page number.
max_page_size: The maximum page size as determined by the request.
page_size: The number of notifications returned in this request.
notifications: An array of notifications, where each notification includes:
id: Notification identifier.
user: Information about the user receiving the notification.
actor: Information about the actor who triggered the notification.
action_type: Type of action that generated the notification.
has_seen: Timestamp when the notification was seen (if applicable).
content: The content of the notification.
target_id: Target identifier related to the notification.
content_avatar_url: URL for any avatar associated with the content.
created_at: Timestamp when the notification was created.
Error Handling
Common error responses include:

Authentication Failure:

Redirects the user to a forbidden route if authentication fails.
Invalid App Name:

Returns a JSON response with a 400 status code if the app name is invalid.
Invalid Pagination Parameters:

Returns a JSON response with a 400 status code if page or page_size are not numeric.
Testing with Postman
Follow these steps to test the endpoint using Postman:

Set Up the Request:

Method: GET
URL: http://your-api-domain.com/user/notification
Replace your-api-domain.com with your actual domain or localhost if testing locally.
Add Query Parameters:

Click on the Params tab in Postman.
Add the following parameters as needed:
page (e.g., 1)
page_size (e.g., 10)
app_name (optional, e.g., "all" for admin users)
Authentication:

Since the endpoint requires the user to be authenticated, ensure you have the appropriate authentication headers (such as a token or session cookie) set in Postman.
For example, under the Authorization tab, choose the authentication type (e.g., Bearer Token) and enter the token.
Send the Request:

Click the Send button.
Review the JSON response in the body of the response.
Verify the Response:

A successful request returns a JSON object with a status of "success" along with the notifications.
In case of errors (authentication failure, invalid parameters), review the returned error message and status code.
Conclusion
This endpoint provides a structured approach to retrieving user notifications with pagination and robust error handling. Use the instructions above to test and verify the endpoint functionality via Postman.

