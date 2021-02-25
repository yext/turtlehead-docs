---
title: Universal Results
order: 5
component: true
appliesTo: Universal
apiProperties:
  - property: appliedFilters
    type: object
    description: Settings for the applied filters bar in the results header. These settings can be overriden in the "config" option below on a per-vertical basis.
    properties:
      - property: show
        type: boolean
        description: If true, show any applied filters that were applied to the universal search.
        default: true
      - property: showFieldNames
        type: boolean
        default: false
        description: If appliedFilters.show is true, whether to display the field name of an applied filter, e.g. "Location - Virginia" vs just "Virginia".
      - property: hiddenFields
        type: array
        default: "['builtin.entityType']"
        description: If appliedFilters.show is true, this is list of filters that should not be displayed.
      - property: resultsCountSeparator
        type: string
        default: '|'
        description: The character that separates the count of results (e.g. “1-6”) from the applied filter bar.
      - property: showChangeFilters
        type: boolean
        default: false
        description: Whether to display the change filters link that links out to the vertical search.
      - property: changeFiltersText
        type: string
        default: 'change filters'
        description: If showChangeFilters is true, the text for the change filters link.
      - property: delimiter
        type: string
        default: '|'
        description: The character that separates each field (and its associated filters) within the applied filter bar.
      - property: labelText
        type: string
        default: 'Filters applied to this search:'
        description: The aria-label given to the applied filters bar.
  - property: config
    type: object
    description: configuration for each vertical's results. Each property is nested under the verticalKey.
    properties:
    - property: card
      type: object
      default: Standard
      description: The card used to display each individual result, see Result Cards.
    - property: template
      type: string
      description: A custom Handlebars template for this section
    - property: title
      type: string
      description: The title of the vertical, displayed in the section heading.
      default: The vertical key
    - property: icon
      type: string
      description: Icon to display to the left of the title. Must be one of the SDK's built-in icons
      default: 'star'
    - property: url
      type: string
      description: The url for both the viewMore link and the change-filters link. The `VERTICAL_KEY` used in the default is the key in the `config` object.
      default: VERTICAL_KEY.html
    - property: viewMore
      type: boolean
      description: Whether to display a view more link at the bottom of the universal results section for this vertical. `Url`, or the `verticalPages`.`url` initialization option must be populated. See more on `verticalPages` [here](/initialization-options/vertical-pages).
      default: true
    - property: viewMoreLabel
      type: string
      description: The text for the view more link, if viewMore is true. 
      default: 'View More'
    - property: appliedFilters
      type: object
      description: Same as appliedFilters settings above. Settings specified here will override any top level settings. 
      properties:
      - property: show
        type: boolean
        description: If true, show any applied filters that were applied to the universal search.
        default: true
      - property: showFieldNames
        type: boolean
        default: false
        description: If appliedFilters.show is true, whether to display the field name of an applied filter, e.g. "Location - Virginia" vs just "Virginia".
      - property: hiddenFields
        type: array
        default: "['builtin.entityType']"
        description: If appliedFilters.show is true, this is list of filters that should not be displayed.
      - property: resultsCountSeparator
        type: string
        default: '|'
        description: The character that separates the count of results (e.g. “1-6”) from the applied filter bar.
      - property: showChangeFilters
        type: boolean
        default: false
        description: Whether to display the change filters link that links out to the vertical search.
      - property: changeFiltersText
        type: string
        default: 'change filters'
        description: If showChangeFilters is true, the text for the change filters link.
      - property: delimiter
        type: string
        default: '|'
        description: The character that separates each field (and its associated filters) within the applied filter bar.
      - property: labelText
        type: string
        default: 'Filters applied to this search:'
        description: The aria-label given to the applied filters bar.
    - property: useAccordion
      type: boolean
      deprecated: true
      description: Use the accordion card instead. Whether to use the AccordionResults component instead of VerticalResults for this vertical. 
      default: false
    - property: includeMap
      type: boolean
      description: Whether to include a map in this vertical results section.
      default: false
    - property: mapConfig
      type: object
      description: Configuration for the map. If includeMap is true, this is required. Additional configuration for this object described in the Map Component documentation. 
      properties:
      - property: mapProvider
        type: string
        required: true
        description: Either 'mapbox' or 'google', not case sensitive
      - property: apiKey
        type: string
        description: API key for the map provider
        required: true
    - property: viewAllText
      type: string
      deprecated: true
      description: Use viewMoreLabel instead. viewAllText is a synonym for viewMoreLabel, where viewMoreLabel takes precedence over viewAllText.
---

## Background

The `UniversalResults` component renders a set of verticals. Each vertical is rendered as a section. Each section includes a list of results, where each result is rendered as a card. Visually, there are lots of similarities between each vertical section and the Vertical Results component. That's because, under the hood, Universal Results uses [Vertical Results component](/components/vertical-results) to render the sections and their results. 

> To learn more about how to design a full Universal Results pages, check out the
> [Universal Search Results](/guides/universal-search-results-page).

## The Configuration Object

The bulk of the Universal Results work is specifying the configuration for each vertical, under the larger `config` object. All verticals that are specified in the backend Answers configuration will be displayed using the default `standard` card (even if the `config` is not specified, or if a vertical is not included in said `config`). 

## Configuration

Here is an example configuration for `UniversalResults`. 

```js
ANSWERS.addComponent('UniversalResults', {
  container: '.universal-results-container',
  appliedFilters: {
    show: true,
    showFieldNames: false,
    hiddenFields: ['builtin.entityType'],
    resultsCountSeparator: '|',
    showChangeFilters: false,
    changeFiltersText: 'change filters',
    delimiter: '|',
    labelText: 'Filters applied to this search:',
  },
  config: {
    people: { // The verticalKey
      title: 'People',
      icon: 'star',
      url: 'people.html',
      viewMore: true,
      viewMoreLabel: 'View More!',
      showResultCount: true,
      includeMap: true,
      mapConfig: {
        mapProvider: 'google',
        apiKey: '<<< enter your api key here >>>',
      },
      
    }
  },
})
```


## Example

Here is a fully working example:
{{% codesandbox blissful-franklin-3t9o0 %}}
