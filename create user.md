# User Create API

## Endpoint

- **URL:** `/user/create`
- **Method:** `POST`

## Description

This API endpoint creates a new user account. It validates the user data—including first name, last name, username, password, email, and referral code (if applicable)—and registers the user. Upon successful registration, a verification email is sent. Depending on the application (`app_name`), additional actions like push notifications and referral tracking are triggered.

## Request Parameters

| Parameter       | Type   | Required                            | Description |
| --------------- | ------ | ----------------------------------- | ----------- |
| `first_name`    | String | Yes                                 | User's first name. Must be at least 2 characters and contain only alphabets. |
| `last_name`     | String | Yes                                 | User's last name. Must be at least 2 characters and contain only alphabets. |
| `username`      | String | Yes                                 | Desired username. Must be between 4 and 20 characters, can contain only alphanumeric characters and underscores, and must not include any prohibited substrings. |
| `password`      | String | Yes                                 | User's password. Must be at least 6 characters long. |
| `email`         | String | Yes                                 | User's email address in a valid email format. |
| `app_name`      | String | Yes (default: `empowerverse`)       | Name of the application. This value must match an allowed project. |
| `referral_code` | String | Conditionally (required for certain apps, e.g., when `app_name` is `vible`) | Referral code for joining the platform. It is also used for referral tracking for some apps. |

## Headers

- **Content-Type:** `application/json`

## Request Examples

### Example Request (with Referral Code)

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "username": "john_doe",
  "password": "secret123",
  "email": "john@example.com",
  "app_name": "empowerverse",
  "referral_code": "REF123"
}
