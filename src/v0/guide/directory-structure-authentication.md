---
title: Authentication
type: guide
order: 301
vue_version: 0.0.1
---

Authentication is how the server knows who is making the request. A common method for doing so is to issue a persistent session after a user has provided valid credentials, however there are many available strategies.

The authentication subdirectory is an editable subdirectory where you can handle any authentication logic. Each service, and further, each service's actions have their own subdirectories.

```
my-first-api
│   ...
│
│
│
│
│
└src
    │   server.ts
    │
    └authentication
    │   │
    │   └{service}
    │   │   │
    │   │   └{action}
    │   │   │   │   index.ts
    │   │   │
    │   │   └{...}
    │   │
    │   └{...}
    │
    └...
```

An example action's `index.ts` file is:

```typescript
import { Request, Response } from 'express';
import appContext from '../../../context/app';
import requestContext from '../../../context/request/todos/show';
import { HttpReturn } from '../../../internal/utils';
import { ShowTodoPayload } from '../../../models';

export default async (
  appCtx: appContext,
  requestCtx: requestContext,
  payload: ShowTodoPayload,
  req: Request,
  res: Response,
): Promise<HttpReturn | void> => {
  // check session
  if (!req.session.userID)
    return new HttpReturn(401, 'unauthorized');

  requestCtx.isAdmin = session.isAdmin;
  requestCtx.userID = session.userID;
}
```

Actions that do not have a payload defined in the `./design.json` file will not include a payload parameter in the function definition.

Returning an `HttpReturn` from the function will break the chain of request middleware. The next function in the chain is the authorization handler.
