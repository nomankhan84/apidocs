# User Validation API

## Endpoint

**URL:** `/user/validate`
**Method:** `POST`

## Description
This API is used to validate a username or an email address. It checks whether the given input is valid and whether it is already in use.

## Request Parameters

| Parameter   | Type   | Required | Description |
|------------|--------|----------|-------------|
| `mixed`    | `string` | Yes      | The email or username to be validated. If it contains '@' and '.', it is treated as an email; otherwise, it is treated as a username. |
| `app_name` | `string` | Yes      | The name of the application to check the validity against. |

## Headers

No authentication headers are required.

## Request Example

### Validating an email
```json
{
    "mixed": "user@example.com",
    "app_name": "MyApp"
}
```

### Validating a username
```json
{
    "mixed": "username123",
    "app_name": "MyApp"
}
```

## Response Format

### Success Responses

#### Valid Email
```json
{
    "status": "success",
    "message": "Email is valid"
}
```

#### Valid Username
```json
{
    "status": "success",
    "message": "Username is valid"
}
```

### Error Responses

#### Missing Parameters
```json
{
    "status": "failed",
    "message": "Mixed parameter is required"
}
```

#### Invalid App Name
```json
{
    "status": "failed",
    "message": "Invalid app_name parameter! Kindly contact Empowerverse team"
}
```

#### Invalid Email Format
```json
{
    "status": "error",
    "message": "Invalid Email address."
}
```

#### Email Already in Use
```json
{
    "status": "failed",
    "message": "Email is already in use"
}
```

#### Invalid Username Format
```json
{
    "status": "error",
    "message": "Username can only have alphanumeric characters and underscores"
}
```

#### Username Too Short or Long
```json
{
    "status": "error",
    "message": "Username must be between 4 and 20 characters, inclusive."
}
```

#### Username Contains Prohibited Words
```json
{
    "status": "error",
    "message": "Username cannot contain: [prohibited word]"
}
```

#### Username Already Taken
```json
{
    "status": "failed",
    "message": "Username is already taken"
}
```

## Notes
- The `app_name` parameter is required to verify the validity of the username or email within a specific project.
- The API ensures usernames are alphanumeric and between 4 and 20 characters long.
- The API checks if the username or email already exists in the database for the given `app_name`.
- Some words may be prohibited from being used in usernames.

## Status Codes
- `200 OK` – Validation successful
- `400 Bad Request` – Invalid parameters or already in use

