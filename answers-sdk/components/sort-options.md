---
title: Sort Options
order: 11
component: true
appliesTo: Vertical
apiProperties:
  - property: verticalKey
    type: string
    required: true
    description: The vertical key for this vertical.
  - property: options
    type: array
    required: true 
    description: Array of sorting objects. Each object contains type, label, field and direction.
    properties:
    - property: type
      type: string
      required: true 
      description: The type of sorting option. Can be either `FIELD` (on a field),`ENTITY_DISTANCE` (distance), or `RELEVANCE` (relevance determined by answers algorithm and any sorting logic in the serverside configuration, same as the default sort order).
    - property: label
      type: string
      required: true
      description: The label for this sort option
    - property: field
      type: string
      description: Required if `type` is `FIELD`. The API name of the field on which to sort.
    - property: direction
      type: string
      description: Required if `type` is `FIELD`. Can be either ASC (ascending) or DESC (descending). The direction in which to sort.
  - property: defaultSortLabel
    default: Best Match
    type: string
    description: The label used for the “default” sort (determined by answers algorithm and any sorting logic in the serverside configuration), defaults to 'Best Match'
  - property: optionSelector
    type: array
    required: false
    default: '.yxt-SortOptions-optionSelector'
    description: The selector used for options in the sort options template.
  - property: searchOnChange
    type: boolean
    required: false
    default: false
    description: Whether or not changing an option triggers a search immediatley. If false the component also renders an apply button that applies the sort. If true, no apply button is shown.
  - property: showReset
    type: boolean
    required: false
    default: false
    description: Whether or not to show a "reset" in the component. If true, clicking reset will set the component back to the default label. 
  - property: resetLabel
    type: string
    required: false
    default: reset
    description: The label to use for the reset button.
  - property: showMore
    type: boolean
    required: false
    default: true
    description: Allow collapsing excess filter options after a limit, defaults to true. Note, screen readers will not read options hidden by this flag, without clicking show more first.
  - property: showMoreLimit
    type: string
    required: false
    default: 5
    description: The max number of filter options to show before collapsing extras.
  - property: showMoreLabel
    type: string
    required: false
    default: 'Show more'
    description: The label to show for displaying more options
  - property: showLessLabel
    type: string
    required: false 
    default: 'Show less'
    description: The label to show for displaying fewer options.
  - property: onChange
    type: function
    required: false
    default: function() => {}
    description: The callback function to call when changed. Runs before a search is triggered if `searchOnChange` is true.
  - property: label
    type: string
    required: false
    default: 'Sorting'
    description: The label to be used in the legend above the sort options
  - property: applyLabel
    type: string
    required: false
    default: 'Apply'
    description:  The label to be used on the apply button. Only appears if `searchOnChange` is false.
---

## Background
The `SortOptions` component shows optional sorting that a user can apply to a set of results. This component is only used on vertical search. `SortOptions` can be configured to include sorting by a field, distance (automatically calculated from the entity's address), and relevance. The default sort is the order specified by the serverside configuration. Notably, any sorting applied client-side via this component does not replace what’s in the serverside configuration, it’s simply layered on top. Learn more about how to fully configure sorting in this [Hitchhiker module](https://hitchhikers.yext.com/tracks/answers-advanced/ans320-sorting/).

![SortOptions](/img/docs/sorting.png)


## Basic Configuration

There are only a few required configuration options for `SortOptions`; the `verticalKey` and an `options` array. Within the `options` array, you must specify the `type` of sorting option (either `FIELD`, `ENTITY_DISTANCE` or `RELEVANCE`) and the `label` for that sorting option. 

```html
<div class="sort-options-container"></div>
```

```js
ANSWERS.addComponent('SortOptions', {
  container: '.sort-options-container',
  verticalKey: 'locations',
  options: [
    {
      type: 'FIELD',
      field: 'c_popularity',
      direction: 'ASC',
      label: 'Popularity',
    },
    {
      type: "ENTITY_DISTANCE",
      label: 'Distance'
    },
    {
      type: 'RELEVANCE',
      label: 'Relevance'
    }
  ],
});
```

## Advanced Configuration
Beyond the basic configuration options, you can also control various settings similar to the [FilterOptions](/components/filter-options) and [Facets](/components/facets). These include `showMore`, `showMoreLimit`, `showMoreLabel`, `showLessLabel`, `searchOnChange` and `applyLabel`. 

Finally, you can control the `defaultSortLabel`, which is the label for the option that is selected on load and corresponds to the default sort. You can show a reset button via `showReset`, and the label of that button via `resetLabel`.

In the below example, we've updated the `defaultSortLabel` and the `label` for sorting. We've also added a reset button with the label "reset the sort".

{{% codesandbox cocky-leaf-6rc8r %}}


## What Does Ascending and Descending Mean Per Field Type?

| Field                | ASC          | DESC         | 
| -------------------- | -------------------------------------------- | ------------------------------------------- |
| `Date`               | Oldest to most recent, IE 1/1/1990, 1/1/2020 | Most recent to oldest, IE 1/1/2020, 1/1/1990|
| `Number`             | Smalles to largest                           | Largest to smallest                         | 
| `Boolean`            | False, True  | True, False          | 
| `String`              |  Special characters, then A-Z  |  Z-A, then special characters         |
