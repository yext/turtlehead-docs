---
title: FilterSearch
order: 7
component: true
appliesTo: Vertical
apiProperties:
  - property: verticalKey
    type: string
    required: true
    description: verticalKey
  - property: placeholderText
    type: string
    description: Placeholder text for the input
  - property: searchParameters
    type: object
    description: Params for the FilterSearch
    properties:
      - property: fields
        type: array
        description: List of fileds to query
        properties:
          - property: fields[].fieldId
            type: string
            description: Field ID of the field in KG
          - property: fields[].entityTypeIds
            type: string
            description: Entity type api name e.g. `healthcareProfessional`, `ce_person`
      - property: searchParameters.sectioned
        type: boolean
        default: false
        description: if true sections search results by search filter
  - property: storeOnChange
    type: boolean
    default: false
    description: If true the selected filter is saved and used for the next search but it does not trigger a new search.
  - property: title
    type: string
    description: A title above the FilterSearch box
  - property: searchText
    type: string
    default: What are you interested in?
    description: The search text used for labeling the input box
  - property: promptHeader
    type: string
    description: Text to show as the first item in autocomplete
  - property: autoFocus
    type: boolean
    default: false
    description: Auto focuses the input box
  - property: redirectUrl
    type: string
    description: Redirect to search query to a new URL
  - property: query
    type: string
    description: Optional, the query displayed on load. Defaults to the query stored in the url (if any). Does not conduct a search.
  - property: filter
    type: object
    description: the filter for component to apply on load, defaults to the filter stored in the url (if any). For more information see the filter section of this [API documentation](https://developer.yext.com/docs/api-reference/#operation/KnowledgeApiServer.listEntities)


---

## Background

The `FilterSearch` component provides a text input box for users to type characters and select a preset matching filter.

The potential filters displayed are based on fields in the Knowledge Graph. A single `FilterSearch` can combine multiple filters into one interface. FilterSearch has typo tolerance built in but does not do any natural language processing and only supports selecting an explicit filter.

When a filter is selected, a vertical search is performed. If multiple FilterSearch components are on the page, the search will include all selected filters across all of the components.

## Styling

`FilterSearch` doesn't come with much styling, so it's recommended to write custom CSS to style the component. 

![FilterSearch](/img/docs/filter-search.png)

The below [demo](#example) includes some example custom CSS.

## Configuration

The only required configuration option beyond `container` and `verticalKey` (if not specified in the [top level search configuration initialization](/core-concepts/initialization)) is `searchParameters`. 

- `searchParameters` specifies which fields should be searchable. It takes in two properties: `sectioned` and `fields`.
- `fields` takes in an array of field options (`fieldId`, `entityTypeId`). `sectioned` determines if the multiple fields should be seperated by sections.

## Using Single vs. Multiple Fields
`Filtersearch` can filter on a single field, or more than one. 

### Single Field

Here is an example of `FilterSearch` using a single field - `name`

```js
ANSWERS.addComponent("FilterSearch", {
  container: ".filter-search",
  verticalKey: "locations",
  searchText: "Find an Employee",
  searchParameters: {
    fields: [
      {
        fieldId: "name",
        entityTypeId: "ce_person",
      },
    ],
  },
});
```

### Multiple Fields

Here is an example of `FilterSearch` combining two fields together - `name` and `builtin.location`.
`builtin.location` will provide a location autocomplete experience. 

```js
ANSWERS.addComponent("FilterSearch", {
  container: ".filter-search",
  verticalKey: "people",
  searchText: "Find an Employee or Location",
  // highlight-start
  searchParameters: {
    sectioned: true,
    fields: [
      {
        fieldId: "builtin.location",
        entityTypeId: "ce_person",
      },
      {
        fieldId: "name",
        entityTypeId: "ce_person",
      },
    ],
  },
  // highlight-end
});
```

## Setting a Default Query

You might want to set a default query on load. To do this, set the `query` and `filter` attributes; the former controls the text that displays in the input, while the latter controls the actual filter applied on load. 

```js
this.addComponent("FilterSearch", {
  container: ".filter-search-container",
  verticalKey: "locations",
  query: "Peaceful Coffee",
  filter: {
    name: {
      $eq: "Peaceful Coffee"
    }
  },
  searchParameters: {
    sectioned: true,
    fields: [
      {
        fieldId: "builtin.location",
        entityTypeId: "location"
      },
      {
        fieldId: "name",
        entityTypeId: "location"
      }
    ]
  }
});
```
## Example

Here's a demo of `FilterSearch`. In this example, we filter on two fields (`name` and `builtin.location`). We also pre-applied a query and a filter for the location name "Peaceful Cafe". Finally, we've added custom CSS to style the component. 

{{% codesandbox sad-cloud-rtv9m %}}
