---
title: Handlers
type: guide
order: 304
vue_version: 0.0.1
---

Handlers perform the actual business logic of the endpoint.

The handlers subdirectory is an editable subdirectory where you can handle any business logic. Each service, and further, each service's actions have their own subdirectories.

The handler function is the last function called in the request chain. The developer doesn't need to worry about any authentication or authorization logic at this step because those concerns have already been addressed.

```
my-first-api
│   ...
│
│
│
│
│
└src
    │
    │
    └handlers
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

Handler functions take many different forms but a possible example is shown below. Actions that don't have payloads specified in the `./design.json` file will not have a payload parameter in the function definition. Fruthermore, actions that don't have responses specified will not have a `result` and should just return standard http status text.

```typescript
import appContext from '../../../context/app';
import { HttpReturn } from '../../../internal/utils';
import requestContext from '../../../context/request/foos/show';
import {
  Foo,
  ShowFooPayload,} from '../../../models';

export const Handler = async (appCtx: appContext, requestCtx: requestContext, payload: ShowFoooPayload): Promise<HttpReturn> => {
  let result: Foo;

  try {
    // your code, here...
    result = await appCtx.db.showFoo(payload.fooID);

    return new HttpReturn(200, result);
  } catch (e) {
    console.error('err in show action of foos service', e);

    return new HttpReturn(500, 'internal server error');
  }
}
```
