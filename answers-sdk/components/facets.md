---
title: Facets
order: 10
component: true
appliesTo: Vertical
apiProperties:
  - property: verticalKey
    type: string
    description: the vertical for which these facets apply. Required if not included in the [top level search configuration](/core-concepts/initialization).
  - property: title
    type: string
    default: "Filters"
    description: Title to display above the facets
  - property: showCount
    type: boolean
    default: false
    description: Whether to show the number of results for each facet option
  - property: searchOnChange
    type: boolean
    default: false
    description: Whether to execute a new search whenever a facet selection changes
  - property: resetFacet
    type: boolean
    default: false
    description: Show a reset button per facet group
  - property: resetFacetLabel
    type: boolean
    default: 'reset'
    description: If `resetFacet` is true, the label to use for the reset button per facet group
  - property: resetFacets
    type: boolean
    default: false
    description: Show a reset-all button for the facets control. Defaults to true if searchOnChange is false.
  - property: resetFacetsLabel
    type: string
    default: 'reset-all'
    description: If `resetFacets` is true, the label to use to reset all.
  - property: showMore
    type: boolean
    default: true
    description: Whether to allow collapsing of excess facet options after a limit
  - property: showMoreLimit
    type: boolean
    default: true
    description: The max number of filter options to show before collapsing extras
  - property: showMoreLabel
    type: string
    default: show more
    description: If `showMore` is true, the label for the button to display more facets
  - property: showLessLabel
    type: string
    default: show more
    description: If `showMore` is true, the label for the button to display less facets
  - property: expand
    type: boolean
    default: true
    description: Adds an expand/collapse per facet. 
  - property: showNumberApplied
    type: boolean
    default: true
    description: When facet is collapsed, show the number of facet options applied.
  - property: searchable
    type: boolean
    default: false
    description: If true, make this facet searchable by displaying the filter option search input
  - property: placeholderText
    type: string
    default: 'Search here...'
    description: If `searchable` is true, the placeholder text used for the filter option search input
  - property: searchLabelText
    type: string
    default: 'Search for a filter option'
    description: The form label text for the search input
  - property: fields
    type: object
    description: Field-specific overrides
    properties:
      - property: searchable
        type: boolean
        default: false
        description: If true, make this facet searchable by displaying the filter option search input
      - property: placeholderText
        type: string
        description: If `searchable` is true, the placeholder text used for the filter option search input 
      - property: 'control'
        type: string
        default: singleopiton
        description: Control type, either singleoption or multiopton
  - property: applyLabel
    type: string
    default: 'apply'
    description: If `searchOnChange` is false, the label to show on the apply button  
---

## Background
The `Facets` component allows users to filter their search results using the
facets returned by the Answers API. Facets differ from static
filter components in that they are returned by the server, instead of being
defined by the client. This means that the server will decide which facets
are relevant to a particular query and what their options should be, as well
as how many of the results belong to each facet option.

For this reason, the facet component involves less configuration than other
filter components. Like other filter components, facets can only be used on a
`VerticalSearch` page. They can be added to an experience like this:
```html
<div class="facets"></div>
```
```js
ANSWERS.addComponent('Facets', {
  container: '.facets',
  verticalKey: 'VERTICAL_KEY',
});
```

## Basic Configuration
There are a few common configuration options you may want to tweak for the
facets component. Here is a standard example:

```js
ANSWERS.addComponent('Facets', {
  container: '.facets',
  verticalKey: 'VERTICAL_KEY',
  searchOnChange: true,
  showCount: false,
  showMoreLimit: 2,
  showMoreLabel: 'More',
  showLessLabel: 'Less'
});
```
In this example...
- `showCount: false` hides the number of results for each facet option
- `searchOnChange: true` causes the search to automatically refresh as soon as
the user selects a facet
- `showMoreLimit: 10` adds a "Show More" button to reveal more options when
there are more than 10 facet options
- `showMoreLabel and showLessLabel` control the labels of the resulting buttons

The resulting component looks something like this:

![Facets](/img/docs/facets-basic.png)

## Searchable Facets
Some facet fields will have dozens or hundreds of options, so it doesn't make
sense to display them all to the user at once. A better way is to make the facet
**searchable**, which allows the user to search for the particular option
they're looking for. Facets can be made searchable like so:

```js
ANSWERS.addComponent('Facets', {
  container: '.facets',
  verticalKey: 'VERTICAL_KEY',
  searchable: true
});
```

The result looks like this:
![Searchable Facets](/img/docs/facets-searchable.png)


## Field-Specific Configuration
Often it doesn't make sense to apply the same configuration to all facets in
an experience. For example, some facet fields may have hundreds of options and
should be made `searchable`, but others might not.

To apply configuration for particular facet fields, you can utilize the
`fields` object. In the `fields` object, you can reset any component-wide parameter -
such as `searchable` or `showMoreLimit` - for a specific component, like so.
```js
ANSWERS.addComponent('Facets', {
  container: '.facets',
  verticalKey: 'VERTICAL_KEY',
  showMore: false,
  searchOnChange: true,
  fields : {
    'paymentOptions': {
      searchable: true,
      showMoreLimit: 5
    }
  }
});
```

In this example, the `paymentOptions` is searchable and has `showMoreLimit` of 5, which makes sense because
these fields may have many several possible options.

Any other fields will not be searchable and will not have a `showMore`
button, as there may be only a handful of options. The end result looks something
like this:

![Facets with field Overrides](/img/docs/facets-field-overrides.png)


{{% codesandbox wandering-rain-4y8hp %}}
