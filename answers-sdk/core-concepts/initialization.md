---
title: Answers Initialization
order: 1
component: false
apiProperties:
  - property: apiKey
    type: string
    required: true
    description: Your API key
  - property: experienceKey
    type: string
    required: true
    description: Your answers experienceKey  
  - property: onReady
    type: function
    required: true
    default: function() {}
    description: Invoked when the Answers component library is loaded/ready
  - property: businessId
    type: string
    required: false
    description: Yext businessId, **strongly encouraged** to send analytics requests.     
  - property: useTemplates
    type: boolean
    required: false
    default: true
    description: If false, don't fetch pre-made templates. Only use this if you plan to implement custom renders for every component!    
  - property: templateBundle
    type: object
    required: false
    description: If `useTemplates` is `false`, provide the precompiled templates here.
  - property: locale
    type: string
    required: false
    default: en
    description: The locale of the configuration. The locale is passed to the api request, and will affect how queries are interpreted and the results returned.
  - property: experienceVersion
    type: string
    required: false
    default: PRODUCTION
    description: The Answers Experience version to use for api requests. Can be a label or version number. 
  - property: debug
    type: boolean
    required: false
    default: false
    description: Prints full Answers error details when set to `true`.
  - property: sessionTrackingEnabled
    type: boolean
    required: false
    default: true
    description: If true, the search session is tracked using session storage and cookies. If false, there is no tracking. 
  - property: disableCssVariablesPonyfill
    type: boolean
    required: false
    default: false
    description: Opt-out of automatic css variable resolution on init for legacy browsers
  - property: onStateChange
    type: function
    required: false
    default: function() {}
    description: Invoked when the state of any component changes 
  - property: onVerticalSearch
    type: function
    required: false
    default: function() {}
    description: Analytics callback after a vertical search, see [onVerticalSearch Configuration](/initialization-options/on-vertical-search) for additional details 
  - property: onUniversalSearch
    type: function
    required: false
    default: function() {}
    description: Analytics callback after a universal search, see [onUniversalSearch Configuration](/initialization-options/on-universal-search) for additional details 
  - property: search
    type: object
    required: false
    description: Search specific settings, see [Search Configuration](/initialization-options/search). 
  - property: verticalPages
    type: array
    required: false
    description: provide configuration for each vertical that is shared across components, see [Vertical Pages Configuration](/initialization-options/vertical-pages).
  - property: navigation
    type: object
    required: false
    deprecated: true
    description: Provide navigation configuration including tab configurations. Use `verticalPages` instead.

---



Adding Answers JS to a page is a two step process.

1. Initialize the library
2. Add components.

## Basic Initialization

The first step is to initalize the library. This is done using the **`ANSWERS.init`** command.
This function accepts a set of configuration options.

At a minimum `apiKey`, `experienceKey`, `businessId` and `experienceVersion` must be specified.
Here is an example.

```js
ANSWERS.init({
  apiKey: "3517add824e992916861b76e456724d9",
  experienceKey: "answers-js-docs",
  businessId: "3215760",
  experienceVersion: "PRODUCTION",
  onReady: function() {
    // ADD COMPONENTS HERE
  },
});
```

The `onReady` property is used to add components. You can learn more about adding components in the [Components](/components) section.

## Additional Initialization Options

Learn more about additional initialization configuration in the [Initialization Options](/initialization-options) section.
