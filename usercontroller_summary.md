# UserController Documentation

The `UserController` is a PHP class that extends Symfony's `AbstractController` to manage user-related operations within the application. It leverages Symfony's routing and dependency injection to handle HTTP requests and interact with various services, repositories, and external systems.

## Table of Contents

- [Overview](#overview)
- [Dependencies and Imports](#dependencies-and-imports)
- [Class Properties](#class-properties)
- [Constructor](#constructor)
- [Routes and Methods](#routes-and-methods)
  - [Balance](#balance)
  - [Update Username](#update-username)
  - [Upload Picture](#upload-picture)
  - [Get Picture](#get-picture)
  - [Delete User](#delete-user)
  - [Get Data](#get-data)
  - [Verify Email](#verify-email)
  - [Reset](#reset)
  - [Logout](#logout)
  - [Logout Everywhere](#logout-everywhere)
  - [Active Users](#active-users)
  - [Verify User Profile](#verify-user-profile)
  - [Get Verified Users](#get-verified-users)
  - [Get All Users](#get-all-users)
- [Helper Methods](#helper-methods)
- [Summary](#summary)
- [Contributing](#contributing)
- [License](#license)

## Overview

The `UserController` handles a variety of user-related operations, such as retrieving a user’s balance, updating user information, managing profile pictures, email verification, and user session management. This controller is designed to work within a Symfony-based framework and utilizes dependency injection to integrate with multiple services and repositories.

## Dependencies and Imports

The controller is part of the `App\Controller` namespace and imports several essential classes and services, including:

- **Entity Classes:** `LoginActivity`, `UserReward`, `Verification`, `Profile`, and `User`.
- **Enum Classes:** `LimitedReferralEnum`, `ProjectList`, and `TaskShortCodes`.
- **Message Classes:** `SendPushNotificationMessage` and `SendReward`.
- **Repository Classes:** `PostRepository`, `UserRepository`, `ProfileRepository`, `LimitedReferralRepository`, and `ReferralRepository`.
- **Service Classes:** `Authenticator`, `PostHelper`, `VerificationEmail`, `WebSocketService`, `ConnectionHelper`, `ProfileHelper`, `ReferralService`, `LimitedReferralService`, and `ProjectService`.
- **AWS SDK Classes:** `AwsClient` and `S3Client`.
- **Symfony Components:** `Request`, `RequestStack`, `Response`, `Route`, and `LoggerInterface`.

## Class Properties

The class defines several constants and properties that are critical to its functionality:

- **Constants:**  
  - Password salt  
  - Email verification salt  
  - Record limits  
  - Allowed apps
- **Properties:**  
  Various repositories, services, and other dependencies are injected via the constructor. This setup ensures that the controller can interact with different parts of the application and external services efficiently.

## Constructor

The constructor of the `UserController` initializes its properties with the dependencies provided by Symfony's dependency injection container. This design promotes modularity and makes the controller easily testable and maintainable.

## Routes and Methods

The controller exposes multiple endpoints, each responsible for a specific user-related operation:

### Balance

- **Route:** `/user/balance`  
- **Method:** `GET`  
- **Function:** Retrieves the balance of the authenticated user.

### Update Username

- **Route:** `/user/username`  
- **Method:** `POST`  
- **Function:** Updates the username of the authenticated user.

### Upload Picture

- **Route:** `/user/picture/upload`  
- **Method:** `POST`  
- **Function:** Uploads a profile picture for the authenticated user.

### Get Picture

- **Route:** `/user/picture`  
- **Method:** `GET`  
- **Function:** Retrieves the profile picture URL of the authenticated user.

### Delete User

- **Route:** `/user`  
- **Method:** `DELETE`  
- **Function:** Performs a soft delete on the user account.

### Get Data

- **Route:** `/user/data`  
- **Method:** `GET`  
- **Function:** Retrieves detailed data of the authenticated user.

### Verify Email

- **Route:** `/user/verify`  
- **Method:** `POST`  
- **Function:** Verifies the user's email using a verification code.

### Reset

- **Route:** `/user/reset`  
- **Method:** `POST`  
- **Function:** Redirects to a not found route (currently not in use).

### Logout

- **Route:** `/user/logout`  
- **Method:** `POST`  
- **Function:** Logs out the authenticated user from the current session.

### Logout Everywhere

- **Route:** `/user/logout-everywhere`  
- **Method:** `POST`  
- **Function:** Logs out the authenticated user from all active sessions.

### Active Users

- **Route:** `/users/active`  
- **Method:** `GET`  
- **Function:** Retrieves a list of active users.

### Verify User Profile

- **Route:** `/user/profile/verify`  
- **Method:** `POST`  
- **Function:** Verifies a user's profile.

### Get Verified Users

- **Route:** `/user/profile/verified`  
- **Method:** `GET`  
- **Function:** Retrieves a list of verified users.

### Get All Users

- **Route:** `/users/get_all`  
- **Method:** `GET`  
- **Function:** Retrieves a complete list of users.

## Helper Methods

In addition to the endpoints, the `UserController` includes helper methods for JSON encoding and decoding. These methods facilitate the processing of HTTP requests and responses by standardizing the data format.

## Summary

The `UserController` class is a central component of the user management system in the application. It provides a comprehensive suite of endpoints for managing user accounts, including operations for authentication, profile updates, balance management, email verification, and session management. Its use of dependency injection and modular design makes it a robust and scalable solution within the Symfony framework.

## Contributing

Contributions to this project are welcome. Please follow standard Git practices and submit pull requests for any improvements or bug fixes.

## License

This project is licensed under the [MIT License](LICENSE).
