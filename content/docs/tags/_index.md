---
title: "Tags (Variables & Expressions)"
weight: 3
description: "Use tags to manage variables and expressions in Apify for API testing and mocking."
---

Apify provides a powerful templating engine that allows you to use variables and expressions to make your API tests and mocks dynamic. There are two types of tags you can use:

- `{{ obj.var }}`: For referencing variables defined in your environment or project.
- `{# expression #}`: For evaluating ES6 expressions that can include variables, functions, and more.

#### `{{ obj.var }}` - Variable Reference

This syntax is used to reference variables defined in your environment or project. It allows you to dynamically insert values into your API requests, headers, and response bodies.

**Example:**

```json
{
  "Name": "Get User",
  "Url": "{{env.baseUrl}}/users/{{vars.userId}}",
  "Method": "GET"
}
```

In this example, `{{env.baseUrl}}` and `{{vars.userId}}` will be replaced with their respective values before the request is sent.

#### `{# expression #}` - Expression Evaluation

This tag is used to execute JavaScript (ES6) code. This allows you to perform complex operations, such as generating random data, performing calculations, or even making assertions.

The expression engine supports all ES6 features and provides a set of reserved objects that you can use to interact with Apify's core functionalities.

**Example:**

```json
{
  "Name": "Mock User by ID",
  "Method": "GET",
  "Endpoint": "/api/users/{id}",
  "Responses": [
    {
      "Condition": "true",
      "StatusCode": 200,
      "ResponseTemplate": {
        "id": "{# $.path.id #}",
        "name": "{# $.faker.person.fullName() #}",
        "email": "{# $.faker.internet.email() #}",
        "createdAt": "{# (new Date()).toISOString() #}"
      }
    }
  ]
}
```

In this example, the `ResponseTemplate` uses expressions to generate a random user's name and email, and to set the current date as the `createdAt` field.

##### Reserved Objects

The expression engine provides several reserved objects that you can use within your expressions:

- `$.path`: Represents the request path parameters.
- `$.query`: Represents the query string parameters.
- `$.body`: Represents the request body.
- `$.headers`: Represents the request headers.
- `$.faker`: Provides access to the [Faker.js](https://fakerjs.dev/) library for generating random data.
- `$.env`: Accesses environment variables defined in your Apify configuration.
- `$.vars`: Accesses variables defined in your Apify configuration.
- `$.request`: Represents the entire request object.
- `$.response`: Represents the response object.
- `$.assert`: Provides methods for making assertions in tests.

All reserved objects are available in the global scope of your expressions, but their availability depends on the context in which the expression is evaluated. You can leverage these objects, along with all ES6 features—such as variables, functions, and more—to create dynamic and powerful API tests and mocks in Apify.

> All reserved objects are accessible under the `apify` root object. The `$` symbol serves as a convenient alias for `apify`, so you can use either `apify` or `$` to reference these objects in your expressions.
