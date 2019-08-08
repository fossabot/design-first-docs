---
title: Middleware
type: guide
order: 306
vue_version: 0.0.1
---

Middleware are the first functions evaluated in the request chain, before payload validation. This means that payloads are not available on middleware.

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
    └middleware
    │   │    app.ts
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

Two types of middleware are available: app-wide middleware that will be run on each request, and action specific middleware.

## Application Middleware

Application middleware are mounted to each endpoint and are run on each request. An example is given, below.

```typescript
import { Request, Response, NextFunction } from 'express';

export default (req: Request, res: Response, next: NextFunction): void => {
  next();
}
```

## Action Middleware

Action specific middleware are only run when that specific action's endpoint are called. An example is given, below:

```typescript
import { Request, Response, NextFunction } from 'express';

export default (req: Request, res: Response, next: NextFunction): void => {
  next();
}
```
