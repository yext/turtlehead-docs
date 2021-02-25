---
title: Components
order: 2
apiProperties:
  - property: container
    type: string
    required: true
    description: the CSS selector to append the component
  - property: name
    type: string
    description: a unique name, if using multiple components of the same type
  - property: class
    type: string
    description: a custom class to apply to the component
  - property: template
    type: string
    description: override internal Handlebars template
  - property: render
    type: function
    description: override render function. data provided
  - property: transformData
    type: function
    description: A hook for transforming data before it gets sent to render
  - property: onMount
    type: function
    description: invoked when the HTML is mounted to the DOM
  - property: analyticsOptions
    type: object
    description: additional properties to send with every analytics event
---

This is the current version of answers components

## Overview

Components are the core building block of an Answers Search Experience. Each component represents a standalone piece of functionality.
Some of the most common components include:

- **SearchBar** - Displays a NLP Powered Search Bar
- **Navigation** - Displays tabs to navigate between search pages
- **UniversalResults** - Displays results combining multiple verticals
- **VerticalResults** - Displays results for a single vertical

## Adding a Component

Components are added to the page one by one using the `addComponent` API. Here is a basic example adding a search bar.

```js
ANSWERS.init({
  apiKey: "df4b24f4075800e5e9705090c54c6c13",
  experienceKey: "rosetest",
  businessId: "2287528",
  experienceVersion: "PRODUCTION",
  onReady: function() {
    ANSWERS.addComponent("SearchBar", {
      container: ".search-container",
    });
  },
});
```

> You will notice that components are added in the `onReady` property of the
> `init` function. The rest of the examples on this page omit this piece to
> focus solely on components, but components should ALWAYS be added in the `onReady`
> function.

## Component Container

The `container` property is required for every component you wish to add to the page. This property is a JavaScript selector that must match an HTML element. You should choose a unique class name (you cannot add a component to multiple places on the page). In the example above, this HTML should be somewhere on the page:

```html
<div class="search-container"></div>
```

## Full Example

Putting this together a full HTML page could look like this:

```html
<body>
  <div class="search-container"></div>
  <script>
    ANSWERS.init({
      apiKey: "3517add824e992916861b76e456724d9",
      experienceKey: "answers-js-docs",
      businessId: "3215760",
      experienceVersion: "PRODUCTION",
      onReady: function() {
        ANSWERS.addComponent("SearchBar", {
          container: ".search-container",
        });
      },
    });
  </script>
</body>
```

## Overriding a Template

To completely override the UI of a component, you can pass in a custom Handlebars template to the `template` option. See [custom templates](/advanced-concepts/custom-templates) for more information.

## Custom Rendering

If you want to use a different templating language, you can use the `render` option. This option accepts a function that returns HTML. See [custom rendering](/advanced-concepts/custom-rendering) for more information.

## Custom Components

If you want to create completely custom components, see [custom components](/advanced-concepts/custom-components) for more information.
