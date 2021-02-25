---
title: Creating Custom Components
order: 5
component: false
---

You can create custom Answers components with the same power of the builtin components.

## Creating a Custom Component

You'll create a subtype of ANSWERS.Component and register it.
### For ES6:

```js

class MyCustomComponent extends ANSWERS.Component {
  constructor (config, systemConfig) {
    super(config, systemConfig);
    this.myProperty = config.myProperty;
  }

  static defaultTemplateName () {
    return 'default';
  }

  /**
  * Whether there can be two components with the same name. Recommend setting this 
  * to false.
  * @returns {boolean}
  */
  static areDuplicateNamesAllowed () {
    return false;
  }

  static get type () {
    return 'MyCustomComponent';
  }
}

ANSWERS.registerComponentType(MyCustomComponent); // Register the component with the library
```

### For ES5:
 ```js
function MyCustomComponent (config) {
  ANSWERS.Component.call(this, config);

  this.myProperty = config.myProperty;
}

MyCustomComponent.prototype = Object.create(ANSWERS.Component.prototype);
MyCustomComponent.prototype.constructor = MyCustomComponent;
MyCustomComponent.defaultTemplateName = function () { return 'default' };
MyCustomComponent.areDuplicateNamesAllowed = function () { return false };
Object.defineProperty(MyCustomComponent, 'type', { get: function () { return 'MyCustomComponent' } });

ANSWERS.registerComponentType(MyCustomComponent); // Register the component with the library
```
## Adding the Custom Component
Now you can use your custom component like any built-in component:

```js
ANSWERS.addComponent('MyCustomComponent', {
  container: '.my-component-container',
  template: `<div>{{_config.myProperty}}</div>`,
  myProperty: 'my property'
});
```

## Listening to State
If your component needs to listen to state, give the component a `moduleId` with the relevant storage key. These keys can be found [here](https://github.com/yext/answers/blob/master/src/core/storage/storagekeys.js).

For example, if a custom component should be listening to updates to vertical results, add `this.moduleId = 'vertical-results'` to its constructor:

```js
class MyCustomComponent extends ANSWERS.Component {
  constructor (config, systemConfig) {
    super(config, systemConfig);
    this.myProperty = config.myProperty;
    this.moduleId = 'vertical-results';
  }

  //etc
}
```
When the `vertical-results` storage key updates, the component's `setState` method will automatically be called. 

## Example
Putting it all together, let's say you'd like to create a new template that displays the results count, and updates every time the results update. (We'll use ES6 for this, but the same can easily be extended to ES5.)

![Custom Component for Results Count](/img/docs/custom-component-result-count.png)


### 1. Define the Component's Template
We'll begin by defining the component's template by using `registerTemplate`. The name of the template will be `CustomResultsCountTemplate`. To start, this template won't display anything -- just a hardcoded string (which we'll replace later on). We'll place this within our `onReady`.

```js
ANSWERS.registerTemplate('CustomResultsCountTemplate', `<div class="myCustomResultsCount">Here's where results count will go!</div>` );
```

### 2. Create & Register the Component

Next, we'll create our custom component. We'll name it `CustomResultsCount`. 
We'll start with the basics: the constructor, the default template (which we defined above), then `areDuplicateNamesAllowed` and `type` methods. 

```js
class CustomResultsCount extends ANSWERS.Component {
  constructor (config) {
    super(config);
  }

  static defaultTemplateName () {
    return 'CustomResultsCountTemplate';
  }

  static areDuplicateNamesAllowed () {
    return false;
    //can there be two components of the same name. Recommended false. 
  }

  static get type () {
    return 'CustomResultsCount';
  }
}

ANSWERS.registerComponentType(CustomResultsCount); // Register the component with the library
```

### 3. Add the Component

Finally, we'll add our component to the page, and provide it with a container, `.custom-results-count`.

```html
<div class=custom-results-count></div>
```
```js
this.addComponent('CustomResultsCount', {
  container: '.custom-results-count'
});
```

At this point, here's what our full page looks like:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <!-- JS Links -->
    <script src="https://assets.sitescdn.net/answers/v1/answerstemplates.compiled.min.js"></script>
    <script src="https://assets.sitescdn.net/answers/v1/answers.js"></script>
    <link
      rel="stylesheet"
      type="text/css"
      href="https://assets.sitescdn.net/answers/v1/answers.css"
    />

    <style>
      .answers-container {
        margin: auto;
        max-width: 698px;
      }
    </style>
  </head>

  <body>
    <div class="answers-container">
      <div class="search-bar-container"></div>
      <div class="custom-results-count"></div>
      <div class="results-container"></div>
    </div>

    <script>
      ANSWERS.init({
        apiKey: "3517add824e992916861b76e456724d9",
        experienceKey: "answers-js-docs",
        businessId: "3215760",
        experienceVersion: "PRODUCTION",
        verticalKey: "locations",
        search: {
          verticalKey: "locations",
          defaultInitialSearch:
            "locations that take mastercard and offer free wifi",
          limit: 20
        },
        onReady: function () {
          ANSWERS.registerTemplate(
            "CustomResultsCountTemplate",
            `<div class="myCustomResultsCount">Here's where results count will go!</div>`
          );

          class CustomResultsCount extends ANSWERS.Component {
            constructor(config, systemConfig) {
              super(config, systemConfig);
            }

            static defaultTemplateName() {
              return "CustomResultsCountTemplate";
            }

            static areDuplicateNamesAllowed() {
              return false;
            }

            static get type() {
              return "CustomResultsCount";
            }
          }

          ANSWERS.registerComponentType(CustomResultsCount); // Register the component with the library

          this.addComponent("CustomResultsCount", {
            container: ".custom-results-count"
          });

          //other components below
          this.addComponent("SearchBar", {
            container: ".search-bar-container",
            verticalKey: "locations"
          });

          this.addComponent("VerticalResults", {
            container: ".results-container",
            hideResultsHeader: true
          });
        }
      });
    </script>
  </body>
</html>

```
### 4. React to State
Now that we have a functioning custom component, we'll need it to react to the state of the results updating. 

First, we'll update our constructor to listen to the `vertical-results` storage key by setting `this.moduleId = 'vertical-results';` in the constructor. The storage keys can all be found [here](https://github.com/yext/answers/blob/master/src/core/storage/storagekeys.js).

Next, we'll need to add the `setState` method to our `CustomResultsCount` class. For now, we'll log the data passed to `setState`, and pass it to the super's setState method. This will help us define where the results count lives. 

```js
class CustomResultsCount extends ANSWERS.Component {

  constructor(config, systemConfig) {
    super(config, systemConfig);
    this.moduleId = 'vertical-results'; 
  }

  //what's returned here is what is made available to the component's template
  setState(data) {
    console.log(data.toString());
    super.setState({
      ...data
    });
  }
  // defaultTemplateName(), areDuplicateNamesAllowed(), get type() all defined here
}
```

The console prints the following

`VerticalResults {searchState: "search-complete", verticalConfigId: "locations", resultsCount: 3, encodedState: "", appliedQueryFilters: Array(3), …} `

We'll therefore need to access `data.resultsCount`. 

### 5. Add `resultsCount` 

Now that we're listening to state and we know where `resultsCount` is coming from, we'll modify `setState` and our default template to reflect that `resultsCount`. 

```js

ANSWERS.registerTemplate('CustomResultsCountTemplate', `<div class="myCustomResultsCount">{{resultsCount}}</div>` ); //add resultsCount to the template

class CustomResultsCount extends ANSWERS.Component {
  
  constructor(config, systemConfig) {
    super(config, systemConfig);
    this.moduleId = 'vertical-results'; 
  }

  setState(data) {
    const resultsCount = data.resultsCount; //define results count
    super.setState({
      ...data,
      resultsCount //return it
    });
  }
  // defaultTemplateName(), areDuplicateNamesAllowed(), get type() all defined here
}

```

Here's a final working example:

{{% codesandbox custom-component-3c6i8 %}}


