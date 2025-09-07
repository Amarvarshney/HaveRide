# User Registration API Documentation

## Endpoint

`POST /users/register`

## Description

Registers a new user. Requires first name, email, and password. Password is securely hashed before storing. Returns a JWT token and the created user object (excluding password).

## HTTP Method
POST

## Request Body

Send as JSON:

```json
{
  "fullname": {
    "firstname": "Rahul",      // required, string, min 3 chars
    "lastname": "Kumar"        // optional, string, min 3 chars
  },
  "email": "rahul@example.com", // required, valid email
  "password": "yourPassword"    // required, string, min 6 chars
}
```

## Responses

- **201 Created**
  - Success. Returns:
    ```json
    {
      "token": "<jwt_token>",
      "user": {
        "_id": "...",
        "fullname": {
          "firstname": "Rahul",
          "lastname": "Kumar"
        },
        "email": "rahul@example.com",
        "socketId": null
      }
    }
    ```
- **400 Bad Request**
  - Validation failed. Returns:
    ```json
    {
      "errors": [
        {
          "msg": "First name must be at least 3 characters long",
          "param": "fullname.firstname",
          "location": "body"
        }
        // ...other errors
      ]
    }
    ```
- **500 Internal Server Error**
  - Unexpected error or duplicate email.

## Validation Rules

- `fullname.firstname`: required, string, min 3 characters
- `fullname.lastname`: optional, string, min 3 characters
- `email`: required, valid email, unique
- `password`: required, string, min 6 characters

---

# User Login API Documentation

## Endpoint

`POST /users/login`

## Description

Authenticates an existing user using email and password. Returns a JWT token and the user object (excluding password).

## HTTP Method
POST

## Request Body

Send as JSON:

```json
{
  "email": "rahul@example.com", // required, valid email
  "password": "yourPassword"    // required, string, min 6 chars
}
```

## Responses

- **200 OK**
  - Success. Returns:
    ```json
    {
      "token": "<jwt_token>",
      "user": {
        "_id": "...",
        "fullname": {
          "firstname": "Rahul",
          "lastname": "Kumar"
        },
        "email": "rahul@example.com",
        "socketId": null
      }
    }
    ```
- **400 Bad Request**
  - Validation failed. Returns:
    ```json
    {
      "errors": [
        {
          "msg": "Enter a valid email",
          "param": "email",
          "location": "body"
        }
        // ...other errors
      ]
    }
    ```
- **401 Unauthorized**
  - Invalid email or password. Returns:
    ```json
    {
      "message": "Invalid email or password"
    }
    ```

## Validation Rules

- `email`: required, valid email
- `password`: required, string, min 6 characters

---

# User Profile API Documentation

## Endpoint

`GET /users/profile`

## Description

Returns the authenticated user's profile information. Requires a valid JWT token (sent via cookie or Authorization header).

## HTTP Method
GET

## Authentication

- Requires authentication (JWT token).

## Responses

- **200 OK**
  - Success. Returns:
    ```json
    {
      "_id": "...",
      "fullname": {
        "firstname": "Rahul",
        "lastname": "Kumar"
      },
      "email": "rahul@example.com",
      "socketId": null
    }
    ```
- **401 Unauthorized**
  - Missing or invalid token.

---

# User Logout API Documentation

## Endpoint

`GET /users/logout`

## Description

Logs out the authenticated user by blacklisting the JWT token and clearing the cookie.

## HTTP Method
GET

## Authentication

- Requires authentication (JWT token).

## Responses

- **200 OK**
  - Success. Returns:
    ```json
    {
      "message": "Logged out successfully"
    }
    ```
- **401 Unauthorized**
  - Missing or invalid token.

---

# Captain Registration API Documentation

## Endpoint

`POST /captains/register`

## Description

Registers a new captain (driver) with vehicle details. Requires personal and vehicle information. Password is securely hashed before storing. Returns the created captain object (excluding password).

## HTTP Method
POST

## Request Body

Send as JSON:

```json
{
  "fullname": {
    "firstname": "Amit",         // required, string, min 3 chars
    "lastname": "Singh"          // required, string, min 3 chars
  },
  "email": "amit@example.com",   // required, valid email
  "password": "yourPassword",    // required, string, min 6 chars
  "vehicle": {
    "color": "Red",              // required, string, min 3 chars
    "plate": "MH12AB1234",       // required, string, min 3 chars
    "capacity": 4,               // required, integer, min 1
    "vehicleType": "car"         // required, one of: car, motorcycle, auto
  }
}
```

## Responses

- **201 Created**
  - Success. Returns:
    ```json
    {
      "_id": "...",
      "fullname": {
        "firstname": "Amit",
        "lastname": "Singh"
      },
      "email": "amit@example.com",
      "vehicle": {
        "color": "Red",
        "plate": "MH12AB1234",
        "capacity": 4,
        "vehicleType": "car"
      }
    }
    ```
- **400 Bad Request**
  - Validation failed. Returns:
    ```json
    {
      "errors": [
        {
          "msg": "First name must be at least 3 characters long",
          "param": "fullname.firstname",
          "location": "body"
        }
        // ...other errors
      ]
    }
    ```
- **500 Internal Server Error**
  - Unexpected error or duplicate email.

## Validation Rules

- `fullname.firstname`: required, string, min 3 characters
- `fullname.lastname`: required, string, min 3 characters
- `email`: required, valid email, unique
- `password`: required, string, min 6 characters
- `vehicle.color`: required, string, min 3 characters
- `vehicle.plate`: required, string, min 3 characters
- `vehicle.capacity`: required, integer, min 1
- `vehicle.vehicleType`: required, one of: car, motorcycle, auto

---

## Related Files

- [`routes/captain.routes.js`](./routes/captain.routes.js)
- [`controllers/captain.controller.js`](./controllers/captain.controller.js)
