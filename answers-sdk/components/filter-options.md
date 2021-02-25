---
title: FilterOptions
order: 8
component: true
appliesTo: Vertical
apiProperties:
  - property: control
    type: string
    default: singleoption
    description: "`singleoption` or `multioption`. `singleoption` will show a radio control and `multioption` will show as checkboxes"
  - property: options
    type: array
    description: The list of options a user can choose from
    properties:
      - property: options[].label
        type: string
        description: Label to show on the filter option
      - property: options[].field
        type: string
        description: API Field name (e.g. `c_openNow`)
      - property: options[].value
        type: any
        description: The value to the filter the field by
  - property: showReset
    type: boolean
    description: Show a reset button
  - property: storeOnChange
    type: boolean
    description: Where the filter value is saved on change and sent with the next search.
  - property: optionSelector
    type: string
    default: '.js-yext-filter-option'
    description: The selector used for options in the template, defaults to '.js-yext-filter-option'
  - property: showReset
    type: boolean
    default: false
    description: Whether to show a reset button
  - property: resetLabel
    type: string
    default: 'Reset'
    description: The label to use for the reset button
  - property: showMore
    type: boolean
    default: true
    description: WWhether to allow collapsing of excess filter options after a limit
  - property: showMoreLimit
    type: number
    default: 5
    description: The max number of filter options to show before collapsing extras
---

## Background

The `FilterOptions` component allows users to filter their search results using
a list of static, pre-defined filters. Static filters differ from
Facets in that they are defined client-side, which means that they will be
rendered pre-search and are *not* sensitive to the results of the query. Static filters are applied once a search is conducted.

![Filter Options](/img/docs/filter-options-static.png)


> If you are using static filters differently and need to trigger a new search as soon as a static filter changes, use `FilterOptions` within the `FilterBox` component and toggle the `searchOnChange` attribute.

Here's a basic example:

```html
<div class="filter-container"></div>
```

```js
ANSWERS.addComponent('FilterOptions', {
  container: '.filter-container',
  control: 'singleoption',
  options: [
    {
      label: 'Pick Up',
      field: 'pickupAndDeliveryServices',
      value: "In-Store Pickup"
    },
    {
      label: 'Delivery',
      field: 'pickupAndDeliveryServices',
      value: "Delivery"
    }
  ]
});
```


## Options
There are two different types of `FilterOptions`: `STATIC_FILTER` and `RADIUS_FILTER`. 
`STATIC_FILTER` is used for standard filtering on booleans and string fields, whereas 
`RADIUS_FILTER` allows you to filter results based on their distance from the user.

### `STATIC_FILTER`

`STATIC_FILTER` options take few attributes
* `field`:  Corresponds to the field API name in the knowledge graph
* `value`: If the field is a boolean, `true` or `false`, otherwise the display name of the field option.
* `label`: The label to display for this option
* `selected` : Whether this option should be selected on load. 

> If an option is marked as `selected: true` and you've configured a `defaultInitial` search, 
> it will be applied to the results on load. See the
> [Vertical Search Results](/guides/vertical-search-results-page) guide for more information.

```js
{
  options: [
    {
      // Optional, the label to show next to the filter option.
      label: 'Pick Up',
      // Required, the api field to filter on, configured on the Yext platform.
      field: 'pickupAndDeliveryServices', 
      // Required, the value for the above field to filter by.
      value: "In-Store Pickup",
      // Optional, whether this option will be selected on page load. Selected options stored in the url
      // take priority over this. Defaults to false.
      selected: false
    },
    {
      label: 'Delivery',
      field: 'pickupAndDeliveryServices',
      value: "Delivery"
    }
  
  ]
});
```
Here's an example:

{{% codesandbox exciting-williamson-ih66q %}}

### `RADIUS_FILTER`

`RADIUS_FILTER` takes a `value` in meters, in addition to a `label` and the `selected` boolean.

```js
{
  options: [
    {
      // Required, the value of the radius to apply (in meters). If this value is 0, the SDK will not add explicit radius filtering to the request. The backend may still perform its own filtering depending on the query given.
      value: 8046.72,
      // Optional, the label to show next to the filter option.
      label: '5 miles',
      // Optional, whether this option will be selected on page load. Selected options stored in the url
      // take priority over this. Defaults to false.
      selected: false
    },
    {
      value: 16093.4,
      label: "10 miles",
      selected: true
    },
    {
      value: 40233.6,
      label: "25 miles"
    },
    {
      value: 80467.2,
      label: "50 miles"
    }
  ],
}
```

Here's an example:
{{% codesandbox practical-http-kx1hy %}}

## Advanced Configuration
After you've specified your options, you can also take advantage of several of the advanced configuration options. Here, we've added a reset button, removed the expand/collapse, and hidden all options beyond two behind a "show more!" button. 

```js
ANSWERS.addComponent('FilterOptions', {
  container: ".filter-options-container",
  control: "singleoption",
  optionType: "STATIC_FILTER",
  showReset: true,
  resetLabel: "reset!", //defaults to "reset"
  showMore: true,
  showMoreLimit: 2,
  showMoreLabel: "show more!",
  options: [ ... ]
});
```
{{% codesandbox loving-montalcini-q37pd %}}
