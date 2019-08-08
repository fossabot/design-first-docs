---
title: Tree
type: guide
order: 300
vue_version: 0.0.1
---

An example design-first directory tree is given, below. Each individual subdirectory and its purpose are discussed in further sections.

```
my-first-api
│   .gitignore
│   package.json
│   .env.example
│   design.example.json
│   ts.config.json
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
    └internal
    │   <NOT SHOWN - DO NOT EDIT>
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
    └models
        │   index.ts
```
