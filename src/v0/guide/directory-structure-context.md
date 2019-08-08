---
title: Context
type: guide
order: 303
vue_version: 0.0.1
---

Contexts are used in http servers to pass data and methods along with requests are responses. For example, an authorization function may need to access information that results from the authentication function (such as a userID, for example). This information can be passed to the authorization function, along with the request and response, in a context.

There are two types of contexts used in a design-first api: application-wide and request restricted.

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
    └context
    │   │
    │   └app
    │   │   │   index.ts
    │   │
    │   └routes
    │       │   index.ts
    │       │
    │       └{service}
    │       │   │
    │       │   └{action}
    │       │   │   │   index.ts
    │       │   │
    │       │   └{...}
    │       │
    │       └{...}
    │
    └...
```

## Application Context

Application context is set once when the app starts and is avaialble to all authentication, authorization and handler functions in a design-first api.

A common use for application context is to attach database connections, for example.

Application context is declared in `./src/context/app/index.ts` and is initialized in `./src/server.ts`. An example follows.

**./src/db/index.ts**
```typescript
import { resolve } from 'path';
import { Pool } from 'pg';
import { migrate } from 'postgres-migrations';

export default class {
  constructor(private user: string, private password: string, private host: string, private database: string, private port: number) {
    this.pool = new Pool({
      user,
      host,
      database,
      password,
      port,
    })
  }

  public async doesUserOwnFoo (todoID: string, userID: string): Promise<boolean> {
    ...
  }

  public async migrate () {
    await migrate({
      user: this.user,
      password: this.password,
      host: this.host,
      database: this.database,
      port: this.port,
    }, resolve(__dirname, './migrations/'), undefined);
  }

  private pool: Pool
}
```

**./src/context/app/index.ts**
```typescript
import DB from '../../db';

export default class appContext {
  // note: add your app-wide context, here
  db: DB;
}
```
**./src/server.ts**
```typescript
import DB from './db';

// Create the db
const db: DB = new DB(
  process.env.POSTGRES_USER,
  process.env.POSTGRES_PASSWORD,
  process.env.POSTGRES_HOST,
  process.env.POSTGRES_DATABASE,
  parseInt(process.env.POSTGRES_PORT),
);

let appCtx: appContext = new appContext();
appCtx.db = db;
app.set('context', appCtx);

/**
 * Primary app routes.
 */
app.use('/', routes);

/**
 *  * Start Express server.
 */
const server = async () => {
  // Migrate the db.
  try {
    console.log('migrating db');
    await db.migrate();
  } catch (e) {
    console.error('err migrating the database\n', e);
    process.exit(1);
  }

  app.listen(app.get('port'), app.get('host'), () => {
    console.log(
      '  App is running at http://%s:%d in %s mode',
      app.get('host'),
      app.get('port'),
      app.get('env')
    );
    console.warn('  Press CTRL-C to stop\n');
  });
}

export default server();
```

And now you can use the methods exposed on the db class in your app. For example:

**./src/authorization/foos/show/index.ts**
```typescript
import { Request, Response } from 'express';
import appContext from '../../../context/app';
import requestContext from '../../../context/request/foos/show';
import { HttpReturn } from '../../../internal/utils';
import { ShowTodoPayload } from '../../../models';

export default async (
  appCtx: appContext,
  requestCtx: requestContext,
  payload: ShowTodoPayload,
  req: Request,
  res: Response,
): Promise<HttpReturn | void> => {
  if (requestContext.isAdmin)
    return

  const authorized = await appCtx.db.doesUserOwnFoo(payload.fooID, requestContext.userID);
  if (!authorized)
    return new HttpReturn(401, 'unauthorized');
}
```
Notice that this authorization function takes advantage of the request context. We'll discuss request context in the next section.

## Request Context

Request context is scoped to each request and is created and destroyed each time a new request is received by the server. It can be used to pass information from the authentication to the authorization to the handler functions.

An example was shown in the previous section where the authorization function needed to know if the logged in user was an administrator and if not, what is the logged in user's id. An example of how this could be done follows.

Request contexts should not have constructor functions.

**./src/context/request/foos/show/index.ts**
```typescript
import defaultRequestContext from '../../../request';

export default class extends defaultRequestContext {
  public userID: string;
  public isAdmin: boolean;
}
```
**./src/authentication/foos/show/index.ts**
```typescript
import { Request, Response } from 'express';
import appContext from '../../../context/app';
import requestContext from '../../../context/request/foos/show';
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

  requestCtx.isAdmin = req.session.isAdmin;
  requestCtx.userID = req.session.userID;
}
```

Note that the request context extends the `defaultRequestContext` which can be defined in `./src/context/request/index.ts`. This helps if you want some data to be available for every request without having to edit each individual requestContext.
