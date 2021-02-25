---
title: FilterBox
order: 9
component: true
appliesTo: Vertical
apiProperties:
  - property: filters
    type: object
    required: true
    description: List of filter component configurations
  - property: verticalKey
    type: string
    description: The `verticalKey`, required if not specified in top level `search` configuration. 
  - property: title
    type: string
    default: "Filters"
    description: Title to display above the filter components.
  - property: showCount
    type: boolean
    description: Show the number of results for each filter
    default: true
  - property: searchOnChange
    type: boolean
    default: false
    description: Execute a new search whenever a filter selection changes. If true, the Apply and Reset buttons will not display.
  - property: resetFilter
    type: boolean
    default: false
    description: Whether to show a reset button per filter
  - property: resetFilterLabel
    type: string
    default: 'Reset'
    description: The label to use for the reset button per filter
  - property: resetFilters
    type: boolean
    default: false
    description: Whether to show a reset button for the group of filters. Defaults to true if `searchOnChange` is true.
  - property: resetFiltersLabel
    type: string
    default: 'Reset'
    description: The label to use for the reset button for group of filters.
  - property: showMore
    type: boolean
    default: true
    description: Whether to allow collapsing of excess filter options after a limit
  - property: showMoreLimit
    type: number
    default: 5
    description: The max number of filter options to show before collapsing extras
  - property: showMoreLabel
    type: string
    default: show more
    description: If showMoreLimit is true, the label to show for displaying more filter options.
  - property: showLessLabel
    type: string
    default: show less
    description: If showMoreLimit is true, the label to show for displaying fewer filter options.
  - property: showNumberApplied
    type: boolean
    default: false
    description: Show the number of applied filter when a group is collapsed.
  - property: expand
    type: boolean
    default: true
    description: Allow expanding and collapsing entire groups of filters
  - property: applyLabel
    type: string
    default: Apply
    description: The label to show on the apply button. Will only display if `searchOnChange` is false.
---

## Background

The `FilterBox` component allows users to add multiple static filter components together, including [FilterOptions](/components/filter-options). Configuration on the top-level FilterBox component carries through to the individual filter components. Use `FilterBox` instead of individual filter components if:
- A single apply button should impact all filters
- The filters are grouped together visually
- A set of static filter components should share settings

> **Bug Alert**: There's currently an issue with filterbox's searchOnChange functionality. For now, setting this to false will simply hide the apply button, but a new search is needed to the filters to be applied. 

Here's a basic example:

```js
this.addComponent("FilterBox", {
    container: ".filter-box-container",
    searchOnChange: true,
    filters: [
      {
        type: "FilterOptions",
        control: "singleoption",
        optionType: "STATIC_FILTER",
        label: "Services",
        options: [
            {
            label: "Pick Up",
            field: "pickupAndDeliveryServices",
            value: "In-Store Pickup"
            },
            {
            label: "Delivery",
            field: "pickupAndDeliveryServices",
            value: "Delivery"
            }
        ]
        },
        {
        control: "singleoption",
        type: "FilterOptions",
        optionType: "RADIUS_FILTER",
        showExpand: false,
        label: "Within",
        options: [
            {
            value: 8046.72,
            label: "5 miles"
            },
            {
            value: 16093.4,
            label: "10 miles"
            },
            {
            value: 40233.6,
            label: "25 miles"
            },
            {
            value: 80467.2,
            label: "50 miles"
            }
        ]
        } 
    ]
    });
```


## Advanced Configuration & Example
After you've specified the individual filter components, you can also take advantage of several of the configuration options that will apply to all included filter components. Here, we've added a reset button per filter, and a reset button for the group of filters. We've also updated the `showMoreLimit`, which applies to both `FilterOptions` components. Finally, we've set `expand` to false for both `FilterOptions` components, but have overridden it for the static `FilterOptions`. 

{{% codesandbox quizzical-murdock-yo3h0 %}}

