---
title: "Test Assertions"
---

Apify provides comprehensive assertion capabilities using ES6(JavaScript) expressions to validate API responses. Assertions are defined in the `Tests` array of your API test definition.

## What are Assertions?

Assertions are logical statements that verify whether the actual API response matches the expected outcome. They help ensure your API behaves as intended by checking response status codes, headers, body content, and more.

## Defining Assertions

You can define assertions in your test definition file under the `Tests` array. Each assertion uses a ES6(JavaScript) expression that evaluates to `true` or `false`. If an assertion fails, the test is marked as failed.

**Example:**

```json
{
  "Tests": [
    {
      "Description": "Status code is 200",
      "Assert": "$.response.getStatusCode() == 200"
    },
    {
      "Description": "Response contains expected property",
      "Assert": "$.response.getJson()?.name ? true : false"
    }
  ]
}
```

### Fields

- **`Title`**: A descriptive name for the assertion.
- **`Case`**: Any ES6(JavaScript) to be evaluated. The expression should return a boolean value, or you can use the `$.assert` object and its methods to perform assertions.

## Common Assertion Scenarios

- **Status Code Validation:**  
   Ensure the API returns the correct HTTP status code.

  ```js
  $.response.getStatusCode() == 200;
  ```

- **Header Validation:**  
   Check if a specific header exists or has the expected value.

  ```js
  $.response.getJson()?.name ? true : false;
  ```

- **Body Content Validation:**  
   Verify the response body contains expected data.

  ```js
  $.assert.isTrue($.response.getJson()?.active);
  ```

- **Array and Collection Checks:**  
   Assert that a collection in the response has the expected length.

  ```js
  $.response.getJson()?.items?.length == 10;
  ```

- **Handling Assertion Failures:**
  When an assertion fails, it throws an error with a message indicating the failure. You can catch these errors in your test runner to handle them gracefully.

  ```js
  $.assert.isTrue($.response.getJson()?.active);
  ```

When an assertion fails, Apify provides detailed error messages indicating which assertion failed and why. This helps you quickly identify and fix issues in your API.

## Available Assertions

There are several built-in assertions you can use to validate API responses. Here are some common ones:

### List of Assert Methods

- [$.assert.equals](#assertequals)
- [$.assert.notEquals](#assertnotequals)
- [$.assert.isTrue](#assertistrue)
- [$.assert.isFalse](#assertisfalse)
- [$.assert.isNull](#assertisnull)
- [$.assert.isNotNull](#assertisnotnull)
- [$.assert.isEmpty](#assertisempty)
- [$.assert.isNotEmpty](#assertisnotempty)
- [$.assert.isArray](#assertisarray)
- [$.assert.isObject](#assertisobject)
- [$.assert.isString](#assertisstring)
- [$.assert.isNumber](#assertisnumber)
- [$.assert.isBoolean](#assertisboolean)
- [$.assert.isGreaterThan](#assertisgreaterthan)
- [$.assert.isLessThan](#assertislessthan)
- [$.assert.isGreaterThanOrEqual](#assertisgreaterthanorequal)
- [$.assert.isLessThanOrEqual](#assertislessthanorequal)
- [$.assert.isBetween](#assertisbetween)
- [$.assert.isNotBetween](#assertisnotbetween)
- [$.assert.contains](#assertcontains)
- [$.assert.notContains](#assertnotcontains)
- [$.assert.matchesRegex](#assertmatchesregex)
- [$.assert.notMatchesRegex](#assertNotMatchesRegex)

There are another two objects available for the `Case` field, `$.request` and `$.response`, you can use them to access the request and response data directly. For example, you can access the request URL with `$.request.getBod().json.name` or the response body with `$.response.getJson().name`.

- [Request](/docs/api-testing/create-request/#fields-explained)
- [Response](#response)

### Assert

Assertions are made using the `$.assert` object, which provides methods to validate various aspects of the response. All the `$.assert` methods support accepting messages that will be displayed if the assertion fails in last parameter and all methods return a boolean indicating success or failure. Here are some common assertions you can use:

### Assert Method Reference

Below are the commonly used assertion methods available for validating API responses. Each method returns a boolean indicating success or failure and can accept an optional message as the last parameter.

#### $.assert.equals

Checks if two values are equal.

```js
$.assert.equals(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if name is John",
  "Case": "$.assert.equals(Response.Json['name'], 'John')"
}
```

#### $.assert.notEquals

Checks if two values are not equal.

```js
$.assert.notEquals(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if name is not John",
  "Case": "$.assert.notEquals(Response.Json['name'], 'John')"
}
```

#### $.assert.isTrue

Checks if a condition is true.

```js
$.assert.isTrue(condition);
```

**Example:**

```json
{
  "Title": "Check if user is active",
  "Case": "$.assert.isTrue(Response.Json['isActive'])"
}
```

#### $.assert.isFalse

Checks if a condition is false.

```js
$.assert.isFalse(condition);
```

**Example:**

```json
{
  "Title": "Check if user is not active",
  "Case": "$.assert.isFalse(Response.Json['isActive'])"
}
```

#### $.assert.isNull

Checks if a value is null.

```js
$.assert.isNull(value);
```

**Example:**

```json
{
  "Title": "Check if user data is null",
  "Case": "$.assert.isNull(Response.Json['userData'])"
}
```

#### $.assert.isNotNull

Checks if a value is not null.

```js
$.assert.isNotNull(value);
```

**Example:**

```json
{
  "Title": "Check if user data is not null",
  "Case": "$.assert.isNotNull(Response.Json['userData'])"
}
```

#### $.assert.isEmpty

Checks if a collection is empty.

```js
$.assert.isEmpty(collection);
```

**Example:**

```json
{
  "Title": "Check if items array is empty",
  "Case": "$.assert.isEmpty(Response.Json['items'])"
}
```

#### $.assert.isNotEmpty

Checks if a collection is not empty.

```js
$.assert.isNotEmpty(collection);
```

**Example:**

```json
{
  "Title": "Check if items array is not empty",
  "Case": "$.assert.isNotEmpty(Response.Json['items'])"
}
```

#### $.assert.isArray

Checks if a value is an array.

```js
$.assert.isArray(value);
```

**Example:**

```json
{
  "Title": "Check if items is an array",
  "Case": "$.assert.isArray(Response.Json['items'])"
}
```

#### $.assert.isObject

Checks if a value is an object.

```js
$.assert.isObject(value);
```

**Example:**

```json
{
  "Title": "Check if user data is an object",
  "Case": "$.assert.isObject(Response.Json['userData'])"
}
```

#### $.assert.isString

Checks if a value is a string.

```js
$.assert.isString(value);
```

**Example:**

```json
{
  "Title": "Check if name is a string",
  "Case": "$.assert.isString(Response.Json['name'])"
}
```

#### $.assert.isNumber

Checks if a value is a number.

```js
$.assert.isNumber(value);
```

**Example:**

```json
{
  "Title": "Check if age is a number",
  "Case": "$.assert.isNumber(Response.Json['age'])"
}
```

#### $.assert.isBoolean

Checks if a value is a boolean.

```js
$.assert.isBoolean(value);
```

**Example:**

```json
{
  "Title": "Check if isActive is a boolean",
  "Case": "$.assert.isBoolean(Response.Json['isActive'])"
}
```

#### $.assert.isGreaterThan

Checks if a value is greater than another.

```js
$.assert.isGreaterThan(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if user age is greater than 18",
  "Case": "$.assert.isGreaterThan(Response.Json['age'], 18)"
}
```

#### $.assert.isLessThan

Checks if a value is less than another.

```js
$.assert.isLessThan(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if user age is less than 65",
  "Case": "$.assert.isLessThan(Response.Json['age'], 65)"
}
```

#### $.assert.isGreaterThanOrEqual

Checks if a value is greater than or equal to another.

```js
$.assert.isGreaterThanOrEqual(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if user age is greater than or equal to 18",
  "Case": "$.assert.isGreaterThanOrEqual(Response.Json['age'], 18)"
}
```

#### $.assert.isLessThanOrEqual

Checks if a value is less than or equal to another.

```js
$.assert.isLessThanOrEqual(actualValue, expectedValue);
```

**Example:**

```json
{
  "Title": "Check if user age is less than or equal to 65",
  "Case": "$.assert.isLessThanOrEqual(Response.Json['age'], 65)"
}
```

#### $.assert.isBetween

Checks if a value is between two other values.

```js
$.assert.isBetween(actualValue, lowerBound, upperBound);
```

**Example:**

```json
{
  "Title": "Check if user age is between 18 and 65",
  "Case": "$.assert.isBetween(Response.Json['age'], 18, 65)"
}
```

#### $.assert.isNotBetween

Checks if a value is not between two other values.

```js
$.assert.isNotBetween(actualValue, lowerBound, upperBound);
```

**Example:**

```json
{
  "Title": "Check if user age is not between 18 and 65",
  "Case": "$.assert.isNotBetween(Response.Json['age'], 18, 65)"
}
```

#### $.assert.contains

Checks if a collection contains a specific value.

```js
$.assert.contains(collection, item);
```

**Example:**

```json
{
  "Title": "Check if items array contains 'item1'",
  "Case": "$.assert.contains(Response.Json['items'], 'item1')"
}
```

#### $.assert.notContains

Checks if a collection does not contain a specific value.

```js
$.assert.notContains(collection, item);
```

**Example:**

```json
{
  "Title": "Check if items array does not contain 'item2'",
  "Case": "$.assert.notContains(Response.Json['items'], 'item2')"
}
```

#### $.assert.matchesRegex

Checks if a string matches a regular expression.

```js
$.assert.matchesRegex(value, pattern);
```

**Example:**

```json
{
  "Title": "Check if email matches pattern",
  "Case": "$.assert.matchesRegex(Response.Json['email'], '^[\\w.-]+@[\\w.-]+\\.\\w+$')"
}
```

#### $.assert.notMatchesRegex

Checks if a string does not match a regular expression.

```js
$.assert.notMatchesRegex(value, pattern);
```

**Example:**

```json
{
  "Title": "Check if username does not match pattern",
  "Case": "$.assert.notMatchesRegex(Response.Json['username'], '^admin')"
}
```

```

```
