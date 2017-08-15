# graphql-schema-linter [![Travis CI](https://travis-ci.org/cjoudrey/graphql-schema-linter.svg?branch=master)](https://travis-ci.org/cjoudrey/graphql-schema-linter) [![npm version](https://badge.fury.io/js/graphql-schema-linter.svg)](https://yarnpkg.com/en/package/graphql-schema-linter)

This package provides a command line tool to validate GraphQL schema definitions against a set of rules.

![](https://raw.githubusercontent.com/cjoudrey/graphql-schema-linter/master/screenshot.png)

If you're looking to lint your GraphQL queries, check out this ESLint plugin: [apollographql/eslint-plugin-graphql](https://github.com/apollographql/eslint-plugin-graphql).

## Install

Yarn:

```
yarn global add graphql-schema-linter
```

npm:

```
npm install -g graphql-schema-linter
```

## Usage

```
Usage: graphql-schema-linter [options] [schema.graphql]


Options:

  -o, --only <rules>

    only the rules specified will be used to validate the schema

    example: --only fields-have-descriptions,types-have-descriptions

  -e, --except <rules>

    all rules except the ones specified will be used to validate the schema

    example: --except fields-have-descriptions,types-have-descriptions

  -f, --format <format>

    choose the output format of the report

    possible values: json, text

  -s, --stdin

    schema definition will be read from STDIN instead of specified file

  --version

    output the version number

  -h, --help

    output usage information
```

## Config

In addition to being able to specify rules on the command line, config can also be placed in the following locations:

### In `package.json`

```json
{
  "graphql-schema-linter": {
    "rules": ["enum-values-sorted-alphabetically"]
  }
}
```

### In `.graphql-schema-linterrc`

```json
{
  "rules": ["enum-values-sorted-alphabetically"]
}
```

### In `graphql-schema-linter.config.js`

```js
module.exports = {
  rules: ['enum-values-sorted-alphabetically'],
};
```

## Built-in rules

### `defined-types-are-used`

This rule will validate that all defined types are used at least once in the schema.

### `deprecations-have-a-reason`

This rule will validate that all deprecations have a reason.

### `enum-values-sorted-alphabetically`

This rule will validate that all enum values are sorted alphabetically.

### `fields-have-descriptions`

This rule will validate that all fields have a description.

### `types-are-capitalized`

This rule will validate that interface types and object types have capitalized names.

### `types-have-descriptions`

This will will validate that interface types and object types have descriptions.

## Output formatters

The format of the output can be controlled via the `--format` option.

The following formatters are currently available: `text`, `json`.

### `TextFormatter` (default)

Sample output:

```
5:1 The object type `QueryRoot` is missing a description.
6:3 The field `QueryRoot.a` is missing a description.

2 errors detected
```

Each error is prefixed with the line number and column the error occurred on.

### `JSONFormatter`

Sample output:

```json
{
  "errors": [
    {
      "message": "The object type `QueryRoot` is missing a description.",
      "location": {
        "line": 5,
        "column": 1
      }
    },
    {
      "message": "The field `QueryRoot.a` is missing a description.",
      "location": {
        "line": 6,
        "column": 3
      }
    }
  ]
}
```

## Exit codes

Verifying the exit code of the `graphql-schema-lint` process is a good way of programmatically knowing the
result of the validation.

If the process exits with `0` it means all rules passed.

If the process exits with `1` it means one or many rules failed. Information about these failures can be obtained by
reading the `stdout` and using the appropriate output formatter.

If the process exits with `2` it means an uncaught error happen. This most likely means you found a bug.
