---
title: Authorization
type: guide
order: 302
vue_version: 0.0.1
---

Authorization is how the server decides if the user making the request is allowed to do so. For example, a user is typically unauthorized from requesting resources that don't belong to them.

The authorization subdirectory is an editable subdirectory where you can handle any authorization logic. Each service, and further, each service's actions have their own subdirectories.

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
    └authorization
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
  if (requestCtx.isAdmin)
    return

  const authorized = await appCtx.db.doesUserOwnFoo(payload.fooID, requestContext.userID);
  if (!authorized)
    return new HttpReturn(401, 'unauthorized');
}
```

Actions that do not have a payload defined in the `./design.json` file will not include a payload parameter in the function definition.

Returning an `HttpReturn` from the function will break the chain of request middleware. The next function in the chain is the handler function.
