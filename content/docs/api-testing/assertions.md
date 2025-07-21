---
title: "Test Assertions"

---

Apify provides comprehensive assertion capabilities using C# expressions to validate API responses. Assertions are defined in the `Tests` array of your API test definition.

## What are Assertions?

Assertions are logical statements that verify whether the actual API response matches the expected outcome. They help ensure your API behaves as intended by checking response status codes, headers, body content, and more.

## Defining Assertions

You can define assertions in your test definition file under the `Tests` array. Each assertion uses a C#/TypeScript like expression that evaluates to `true` or `false`. If an assertion fails, the test is marked as failed.

**Example:**

```json
{
    "Tests": [
        {
            "Description": "Status code is 200",
            "Assert": "response.StatusCode == 200"
        },
        {
            "Description": "Response contains expected property",
            "Assert": "response.Body.ContainsKey(\"result\")"
        }
    ]
}
```

### Fields 

- **`Title`**: A descriptive name for the assertion.
- **`Case`**: A C# expression to be evaluated. The expression should return a boolean value. You can use the `Assert` object and its methods to perform assertions.

## Common Assertion Scenarios

- **Status Code Validation:**  
    Ensure the API returns the correct HTTP status code.
    ```csharp
    Response.StatusCode == 200
    ```

- **Header Validation:**  
    Check if a specific header exists or has the expected value.
    ```csharp
    Response.Headers["Content-Type"] == "application/json"
    ```

- **Body Content Validation:**  
    Verify the response body contains expected data.
    ```csharp
    Assert.IsTrue(Response.IsSuccessful)
    ```

- **Array and Collection Checks:**  
    Assert that a collection in the response has the expected length.
    ```csharp
    Response.Json["items"].Count == 10
    ```



## Handling Assertion Failures

When an assertion fails, Apify provides detailed error messages indicating which assertion failed and why. This helps you quickly identify and fix issues in your API.

## Available Assertions

There are several built-in assertions you can use to validate API responses. Here are some common ones:

### List of Assert Methods

- [Assert.Equals](#assertequals)
- [Assert.NotEquals](#assertnotequals)
- [Assert.IsTrue](#assertistrue)
- [Assert.IsFalse](#assertisfalse)
- [Assert.IsNull](#assertisnull)
- [Assert.IsNotNull](#assertisnotnull)
- [Assert.IsEmpty](#assertisempty)
- [Assert.IsNotEmpty](#assertisnotempty)
- [Assert.IsGreaterThan](#assertisgreaterthan)
- [Assert.IsLessThan](#assertislessthan)
- [Assert.IsGreaterThanOrEqual](#assertisgreaterthanorequal)
- [Assert.IsLessThanOrEqual](#assertislessthanorequal)
- [Assert.IsBetween](#assertisbetween)
- [Assert.IsNotBetween](#assertisnotbetween)
- [Assert.Contains](#assertcontains)
- [Assert.Response.StatusCodeIs](#assertresponsestatuscodeis)
- [Assert.Response.StatusCodeIsNot](#assertresponsestatuscodeisnot)
- [Assert.Response.HasHeader](#assertresponsehasheader)
- [Assert.Response.HeaderValueContains](#assertresponseheadervaluecontains)
- [Assert.Response.ContentTypeIs](#assertresponsecontenttypeis)
- [Assert.Response.BodyContains](#assertresponsebodycontains)
- [Assert.Response.BodyMatchesRegex](#assertresponsebodymatchesregex)
- [Assert.Response.RedirectsTo](#assertresponseredirectsto)
- [Assert.Response.IsRedirected](#assertresponseisredirected)

There are another two objects available for the `Case` field, `Request` and `Response`, you can use them to access the request and response data directly. For example, you can access the request URL with `Request.Body.Json.name` or the response body with `Response.Body`.

- [Request](/docs/api-testing/create-request/#fields-explained)
- [Response](#response)

### Assert
Assertions are made using the `Assert` object, which provides methods to validate various aspects of the response. All the `Assert` methods support accepting messages that will be displayed if the assertion fails in last parameter and all methods return a boolean indicating success or failure. Here are some common assertions you can use:

#### Assert.Equils
Checks if two values are equal.
```csharp
Assert.Equals(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if name is John",
  "Case": "Assert.Equals(Response.Json['name'], 'John')"
}
```

#### Assert.NotEquals
Checks if two values are not equal.
```csharp
Assert.NotEquals(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if name is not John",
  "Case": "Assert.NotEquals(Response.Json['name'], 'John')"
}
```

#### Assert.IsTrue
Checks if a condition is true.
```csharp
Assert.IsTrue(condition)
```
**Example:**
```json
{
  "Title": "Check if user is active",
  "Case": "Assert.IsTrue(Response.Json['isActive'])"
}
```

#### Assert.IsFalse
Checks if a condition is false.
```csharp
Assert.IsFalse(condition)
```
**Example:**
```json
{
  "Title": "Check if user is not active",
  "Case": "Assert.IsFalse(Response.Json['isActive'])"
}
```

#### Assert.IsNull
Checks if a value is null.
```csharp
Assert.IsNull(value)
```
**Example:**
```json
{
  "Title": "Check if user data is null",
  "Case": "Assert.IsNull(Response.Json['userData'])"
}
```

#### Assert.IsNotNull
Checks if a value is not null.

```csharp
Assert.IsNotNull(value)
```
**Example:**
```json
{
  "Title": "Check if user data is not null",
  "Case": "Assert.IsNotNull(Response.Json['userData'])"
}
```

#### Assert.IsEmpty
Checks if a collection is empty.
```csharp
Assert.IsEmpty(collection)
```
**Example:**
```json
{
  "Title": "Check if items array is empty",
  "Case": "Assert.IsEmpty(Response.Json['items'])"
}
```

#### Assert.IsNotEmpty
Checks if a collection is not empty.
```csharp
Assert.IsNotEmpty(collection)
```
**Example:**
```json
{
  "Title": "Check if items array is not empty",
  "Case": "Assert.IsNotEmpty(Response.Json['items'])"
}
```

#### Assert.IsGreaterThan
Checks if a value is greater than another.
```csharp
Assert.IsGreaterThan(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if user age is greater than 18",
  "Case": "Assert.IsGreaterThan(Response.Json['age'], 18)"
}
```


#### Assert.IsLessThan
Checks if a value is less than another.
```csharp
Assert.IsLessThan(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if user age is less than 65",
  "Case": "Assert.IsLessThan(Response.Json['age'], 65)"
}
```

#### Assert.IsGreaterThanOrEqual
Checks if a value is greater than or equal to another.
```csharp
Assert.IsGreaterThanOrEqual(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if user age is greater than or equal to 18",
  "Case": "Assert.IsGreaterThanOrEqual(Response.Json['age'], 18)"
}
```

#### Assert.IsLessThanOrEqual
Checks if a value is less than or equal to another.
```csharp
Assert.IsLessThanOrEqual(actualValue, expectedValue)
```
**Example:**
```json
{
  "Title": "Check if user age is less than or equal to 65",
  "Case": "Assert.IsLessThanOrEqual(Response.Json['age'], 65)"
}
```

#### Assert.IsBetween
Checks if a value is between two other values.
```csharp
Assert.IsBetween(actualValue, lowerBound, upperBound)
```
**Example:**
```json
{
  "Title": "Check if user age is between 18 and 65",
  "Case": "Assert.IsBetween(Response.Json['age'], 18, 65)"
}
``` 

#### Assert.IsNotBetween
Checks if a value is not between two other values.
```csharp
Assert.IsNotBetween(actualValue, lowerBound, upperBound)
```
**Example:**
```json
{
  "Title": "Check if user age is not between 18 and 65",
  "Case": "Assert.IsNotBetween(Response.Json['age'], 18, 65)"
}
```

#### Assert.Contains
Checks if a collection contains a specific value.
```csharp
Assert.Contains(collection, item)
```
**Example:**
```json
{
  "Title": "Check if items array contains 'item1'",
  "Case": "Assert.Contains(Response.Json['items'], 'item1')"
}
```


#### Assert.Response.StatusCodeIs
Checks if the HTTP status code matches.
```csharp
Assert.Response.StatusCodeIs(int code)
```
**Example:**
```json
{
  "Title": "Status code is 200 OK",
  "Case": "Assert.Response.StatusCodeIs(200)"
}
```

#### Assert.Response.StatusCodeIsNot
Checks if the HTTP status code matches.
```csharp
Assert.Response.StatusCodeIsNot(int code)
```
**Example:**
```json
{
  "Title": "Status code is 200 OK",
  "Case": "Assert.Response.StatusCodeIsNot(200)"
}
```

#### Assert.Response.HasHeader
Checks if a specific header exists.
```csharp
Assert.Response.HasHeader(string name)
``` 
**Example:**
```json
{
  "Title": "Header 'Content-Type' exists",
  "Case": "Assert.Response.HasHeader('Content-Type')"
}
```
#### Assert.Response.HeaderValueContains
Checks if a header contains a specific value.
```csharp
Assert.Response.HeaderValueContains(string name, string value)
```
**Example:**
```json
{
  "Title": "Header 'Content-Type' contains 'application/json'",
  "Case": "Assert.Response.HeaderValueContains('Content-Type', 'application/json')"
}
```

#### Assert.Response.ContentTypeIs
Checks if the response content type matches.
```csharp
Assert.Response.ContentTypeIs(string contentType)
```
**Example:**
```json
{
  "Title": "Content-Type is application/json",
  "Case": "Assert.Response.ContentTypeIs('application/json')"
}
```

#### Assert.Response.BodyContains
Checks if the response body contains a specific string.
```csharp
Assert.Response.BodyContains(string value)
```
**Example:**
```json
{
  "Title": "Response body contains 'success'",
  "Case": "Assert.Response.BodyContains('success')"
}
```

#### Assert.Response.BodyMatchesRegex
Checks if the response body matches a regular expression.
```csharp
Assert.Response.BodyMatchesRegex(string pattern)
```
**Example:**
```json
{
  "Title": "Response body matches regex",
  "Case": "Assert.Response.BodyMatchesRegex('^\\{.*\\}$')"
}
```

#### Assert.Response.RedirectsTo
Checks if the response redirects to a specific URL.
```csharp
Assert.Response.RedirectsTo(string url)
```
**Example:**
```json
{
  "Title": "Response redirects to 'https://example.com'",
  "Case": "Assert.Response.RedirectsTo('https://example.com')"
}
```

#### Assert.Response.IsRedirected
Checks if the response is a redirect.
```csharp
Assert.Response.IsRedirected()
```
**Example:**
```json
{
  "Title": "Response is redirected",
  "Case": "Assert.Response.IsRedirected()"
}
```

### Response
The `Response` object provides access to the API response data, allowing you to perform assertions on the response status, headers, body content, and more. You can use it in your assertions to validate the API behavior.

#### Properties/Methods
- `IsSuccessful: bool`: Returns `true` if the response status code is in the 2xx range.
- `StatusCode: int`: The HTTP status code of the response.
- `Headers: Dictionary<string, string>`: The response headers. You can access specific headers using `Response.Headers["Header-Name"]`.
- `ContentType: string`: The content type of the response.
- `Body: string`: The raw response body as a string.
- `Json: object`: The parsed JSON response body (if applicable). You can access properties using dot notation, e.g., `Response.Json.propertyName`.
- `ResponseTimeMs: int`: The time taken for the response in milliseconds.
- `ErrorMessage: string`: The error message if the request failed.
- `GetHeader(string name): string`: Returns the value of a specific header.
- `HasHeader(string name): bool`: Checks if a specific header exists.


##### Tips for Writing Assertions

- Use clear and descriptive assertion descriptions.
- Combine multiple conditions using logical operators (`&&`, `||`).