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

## Related Files

- [`routes/user.routes.js`](./routes/user.routes.js)
- [`controllers/user.controller.js`](./controllers/user.controller.js)
- [`services/user.service.js`](./services/user.service.js)
- [`models/user.model.js`](./models/user.model.js)