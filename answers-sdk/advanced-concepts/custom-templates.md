---
title: Using a Custom Template for a Component
order: 3
component: false
---

All component templates are written using Handlebars templates.

It's easy to override these templates with your own templates. Keep in mind, that you must provide valid Handlebars syntax here. You can either define templates in-line, or use a `registerTemplate` method.

## Define Templates In-Line

```js
// Use Handlebars syntax to create a template string
let customTemplate = `<div class="my-search">{{title}}</div>`

ANSWERS.addComponent('SearchBar', {
  container: '.search-container',
  template: customTemplate
})
```


## Define Templates with `registerTemplate`
You can also define templates with our `registerTemplate` method. This method takes two arguments, a name and the template string. 

In order to override an existing component's template, the `name` should match the default template. This is returned in the component's `defaultTemplateName` method; you can find it in the JS files for the various components. For example, [here's the SearchBar](https://github.com/yext/answers/blob/master/src/ui/components/search/searchcomponent.js#L278).


```js
ANSWERS.registerTemplate('search/search', `<div class="my-search">{{title}}</div>` );

ANSWERS.addComponent('SearchBar', {
  container: '.search-container'
})
```

## Which Should I Use?

`registerTemplate` is helpful because it can be called at any point (whereas your `addComponent` can be called once), therefore allowing for dynamic templates. However, in most cases, defining the template inline will suffice. `registerTemplate` is most helpful for [creating custom components](/advanced-concepts/custom-components).