---
title: search
order: 2
component: false
appliesTo: Vertical
apiProperties:
  - property: verticalKey
    type: string
    required: false
    description: The vertical key to use for searches, passed to certain components. Required if a `defaultInitialSearch` is supplied.
  - property: limit
    type: string
    required: false
    default: 20
    description: The number of results to display per page, defaults to 20. Maximum is 50.
  - property: defaultInitialSearch
    type: string
    required: false
    description: A default search to use on page load when the user hasn't provided a query. If set to "", the `SearchBar`'s `allowEmptySearch` must be set to `true`.
---

## Background
The `search` configuration in the initialization is used to control the default initial search and the number of results per page. Notably, it is only relevant for vertical search pages. 

```js
    search: {
      verticalKey: 'verticalKey',
      limit: '20',
      defaultInitialSearch: 'What is Yext Answers?',
    }
```
## Example
{{% codesandbox zen-mahavira-rboyd %}}
