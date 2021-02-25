---
title: Component Lifecycle and State
component: false
---

Components in the Answers Javascript Library have a certain lifecycle and react to the surrounding state; take, for example, the `UniversalResults` Component.

When the `UniversalResults` component is displayed for the first time, it is created and then mounted to the page. When a new set of results return, it is unmounted, then remounted to reflect the new results. 

The behavior of `UniversalResults` described above is part of its component lifecycle. You can also see the component react to the environment around it (or the "state"), namely new results returning. A component's lifecycle is influenced by said state. 

## Lifecycle methods

### Definitions
Every component has the following three lifecycle methods. With the exception of `onMount`, these callbacks do not take any parameters:

1. `onCreate`: Callback envoked when the component is created (the class for that component is instantiated). This creates a new instance of the component class. It is only fired once on a page. 
2. `onMount`: Callback envoked when a component is mounted to the DOM. Calls the render function and appends what is rendered to the DOM. This is passed a reference to the component (`this`).
3. `onUpdate`: Callback envoked when a component's state is updated. It is triggered by a call to `setState`.

> We do not expose an `unMount()` or a `remove()`.

### Using Lifecycle Methods
The above lifecycle methods are exposed in all components' configuration. These callbacks are additive; they are run following our internal lifecycle methods. For example, the below would print a console statement every time the `UniversalResults` was mounted to the page:

```js
this.addComponent('UniversalResults', {
    container: '.results-container',
    onMount: function(data) {
        console.log(data);
    }
    //... other component config here
});
```

If you'd like to completely replace our lifecycle methods, you can use the following methods, which behave like their counterparts listed above:
- `onCreateOverride`
- `onMountOverride`
- `onUpdateOverride`

We **recommend using these methods sparingly**, since you will impact the core functionality of the component. If you find yourself needing to use the overrides, we recommend looking at [creating custom components](/advanced-concepts/custom-components).


## State
As described above, components also react to their surrounding environment, called the state. Changes in state can trigger certain lifecycle methods, such  as `unMount` and `Mount`.

Each component has a `setState` method. This is not exposed currently to implementers using the SDK's built-in components, but is helpful context (and can be used when [creating custom components](/advanced-concepts/custom-components)).

When called, the `setState` method will re-render a component. Said method can be triggered in a few ways: 

1. If the component is listening for a change to something in global storage. 
Going back to our `UniversalResults` example; when the results update, this component will re-render to reflect the new results. Behind the scenes, the component is listening to changes to the `Universal Results` key in global storage. When said key changes, `setState` is called on the `UniversalResults` component. The component is unmounted and then mounted again with the new results.


2. If the component has custom JS. For example, the [Location Bias](/components/location-bias) component has custom JS for disabling the geolocation prompt if a user has denied it. This calls `setState` to set the current display name and accuracy (which will likely be IP accuracy).
