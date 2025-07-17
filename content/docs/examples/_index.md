---
title: "Examples"
weight: 4
---

### Example 1: Testing and Mocking a User API

1.  **Initialize Project:**
    ```bash
    apify init
    ```

2.  **Create a Mock for `GET /api/users/:id`:**
    Use `apify create:mock users.getById` and define it like this in `.apify/users/getById.mock.json`:
    ```json
    {
      "Name": "Mock User by ID",
      "Method": "GET",
      "Endpoint": "/api/users/:id",
      "Responses": [
        {
          "Condition": "path.id == \"1\"",
          "StatusCode": 200,
          "ResponseTemplate": { "id": 1, "name": "Mocked Alice", "email": "alice.mock@example.com" }
        },
        {
          "Condition": "default",
          "StatusCode": 404,
          "ResponseTemplate": { "error": "MockUser not found" }
        }
      ]
    }
    ```

3.  **Start the Mock Server:**
    ```bash
    apify server:mock --port 8080
    ```
    (In a separate terminal)

4.  **Create an API Test for `GET /api/users/1`:**
    Use `apify create:request users.getUser1` and define it in `.apify/users/getUser1.json`:
    ```json
    {
      "Name": "Get User 1",
      "Url": "{{baseUrl}}/users/1", // baseUrl will be http://localhost:8080/api
      "Method": "GET",
      "Tests": [
        { "Title": "Status 200", "Case": "Assert.Response.StatusCodeIs(200)" },
        { "Title": "ID is 1", "Case": "Assert.Equals(1, Response.Json.id)" },
        { "Title": "Name is Mocked Alice", "Case": "Assert.Equals(\"Mocked Alice\", Response.Json.name)" }
      ]
    }
    ```

5.  **Run the Test:**
    ```bash
    apify call users.getUser1 --verbose
    # This will hit your running mock server.
    ```