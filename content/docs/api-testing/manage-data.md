---
title: "Manage Data and Expressions"
---

## Using Data Placeholders and Expressions

When managing data in your API tests, you can use dynamic placeholders and expressions to make your tests more flexible and powerful. This allows you to reference environment variables, custom variables, and even generate fake data on the fly. You can use curly braces `{{ }}` to denote these placeholders.

### Environment and Variable Placeholders

You can reference both environment variables and custom variables in your placeholders:

- `env`: Retrieves the value of an environment variable by its `KEY`.
- `vars`: Lets you define or override custom variables for your requests or tests, for example: `--vars "key1=value1;key2=value2"`. These variables are merged with the current environment and can be accessed using `{{ vars.key1 }}` or `{{ vars.key2 }}` in your placeholders.

For example:

```json
{
  "userId": "{{ env.USER_ID }}",
  "token": "{{ vars.AUTH_TOKEN }}"
}
```

To know more about environment variables, see the [Environment Variables](/docs/variables#environment-variables) section.

### Faker Data

You can generate fake data on the fly using [FakerJS](https://fakerjs.dev/). Use the following syntax:

- `{# $.faker.<method>() #}`: Calls a Faker method to generate data.

Examples:

```json
{
  "name": "{# $.faker.name.fullName() #}",
  "email": "{# $.faker.internet.email() #}"
}
```

### Expression Evaluation

You can use expressions to transform or compute values dynamically with the pipe operator:

You can execute any C# expression on the fly using this syntax:

#### Examples

```json
{
  "timestamp": "{# $.faker.date.past().toISOString() #}",
  "upperCaseName": "{# $.faker.name.firstName().toUpperCase() #}"
}
```

In these examples:

- `$.faker.date.past().toISOString()` formats the current date.
- `$.faker.name.firstName().toUpperCase()` generates a random first name and converts it to uppercase.

This allows you to compose dynamic values and perform inline data manipulation within your API test definitions.

> **Note:**
>
> - `{{ vars }}` and `{# expression #}` serve different purposes:
> - Use `{{ vars }}` to insert values from custom variables you supply. `vars` are not an expression but a direct reference to a variable's value.
> - Use `{# expression #}` to evaluate and insert the result of a runtime expression or code.
