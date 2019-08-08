---
title: Concerns
type: guide
order: 600
vue_version: 0.0.1
---

Most api's need to perform a few basic functions in order to answer a user's request. These basic functions can most typically be broken down into three categories:
1. Authentication i.e. "who are you?"
2. Authorization i.e. "are you allowed to perform this action?"
3. Business logic i.e. the action the user has requested to do

Many bugs can be introduced when these needs are intermingled, addressed out of order or not addressed at all. For this reason, design-first has separated these functionalities so they can be performed individually. However, sometimes information needs to be shared between each. For this reason, design-first implements route scoped context which can be used to pass information or callbacks.

The order that each route is processed is authentication, then authorization and finally business logic.
