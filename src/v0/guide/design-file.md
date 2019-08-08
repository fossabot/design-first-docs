---
title: design.json
type: guide
order: 400
vue_version: 0.0.1
---

The `./src/design.json` file is where the api is specified and should be modified before running `design-first gen`. An example file is included after initializing the app.

The design file contains two sections: api and services.

```json
{
  "api": {
    "name": "design-first-example",
    "description": "A well-designed REST api",
    "baseURL": "",
    "version": "0.0.1"
  },
  "services": [
    {
      "name": "foos",
      "path": "/foos",
      "description": "",
      "actions": [
        {
          "name": "show",
          "description": "",
          "method": "GET",
          "path": "/:fooID",
          "payload": "ShowFoo",
          "response": "Foo"
        },
        {
          "name": "list",
          "description": "",
          "method": "GET",
          "path": "",
          "payload": "ListFoos",
          "response": "Foos"
        },
        {
          "name": "update",
          "description": "",
          "method": "PUT",
          "path": "/:fooID",
          "payload": "UpdateFoo",
          "response": "Foo"
        },
        {
          "name": "create",
          "description": "",
          "method": "POST",
          "path": "",
          "payload": "CreateFoo",
          "response": "Foo"
        },
        {
          "name": "delete",
          "description": "",
          "method": "DELETE",
          "path": "/:fooID"
        }
      ]
    },
    {
      "name": "bars",
      "description": "",
      "path": "/bars",
      "actions": [
        {
          "name": "show",
          "description": "",
          "method": "GET",
          "path": "/:barID",
          "payload": "ShowBar",
          "response": "Boo"
        },
        {
          "name": "list",
          "description": "",
          "method": "GET",
          "path": "",
          "payload": "ListBars",
          "response": "Bars"
        },
        {
          "name": "update",
          "description": "",
          "method": "PUT",
          "path": "/:barID",
          "payload": "UpdateBar",
          "response": "Bar"
        },
        {
          "name": "create",
          "description": "",
          "method": "POST",
          "path": "",
          "payload": "CreateBar",
          "response": "Bar"
        },
        {
          "name": "delete",
          "description": "",
          "method": "DELETE",
          "path": "/:barID"
        }
      ]
    }
  ]
}
```

## API

The api section of the design file contains meta information about the api. None of this information is used elsewhere in the app but may at a future date be used to generate swagger files.

```json
{
  "api": {
    "name": "design-first-example",
    "description": "A well-designed REST api",
    "baseURL": "",
    "version": "0.0.1"
  },
  "services": [ ... ]
}
```

## Services

The services section of the design file contains information about what resources the api exposes. Typically, services are nouns (but not always) and are plural. For example: users, todos, etc.

Services have actions that can be performed on them. In REST terminology, create, read, update, and delete are common actions. Furthermore, read is often further broken down into show and list actions. These are just examples and your needs may vary.

Action payload and response object keys are optional as not every endpoint requires them. Actions that do not have a response key should return standard http status text.

```json
{
  "api": { ... },
  "services": [
    {
      "name": "foos",
      "path": "/foos",
      "description": "",
      "actions": [
        {
          "name": "show",
          "description": "",
          "method": "GET",
          "path": "/:fooID",
          "payload": "ShowFoo",
          "response": "Foo"
        },
        {
          "name": "list",
          "description": "",
          "method": "GET",
          "path": "",
          "payload": "ListFoos",
          "response": "Foos"
        },
        {
          "name": "update",
          "description": "",
          "method": "PUT",
          "path": "/:fooID",
          "payload": "UpdateFoo",
          "response": "Foo"
        },
        {
          "name": "create",
          "description": "",
          "method": "POST",
          "path": "",
          "payload": "CreateFoo",
          "response": "Foo"
        },
        {
          "name": "delete",
          "description": "",
          "method": "DELETE",
          "path": "/:fooID"
        }
      ]
    },
    { ... }
  ]
}
```
