---
title: Vertical Results
order: 6
component: true
appliesTo: Vertical
apiProperties:
  - property: container
    type: string
    description: Selector for the component container
    required: true
  - property: renderItem
    type: function
    description: string to give custom template to result item
  - property: itemTemplate
    type: string
    description: Handlebars template for a custom card
  - property: maxNumberOfColumns
    default: 1
    type: number
    description: Set a maximum number of columns that will display at the widest breakpoint. Possible values are 1,2,3 or 4.
  - property: showResultCount
    type: boolean
    default: true
    description: Whether to display the total number of results
  - property: card
    type: object
    description: The card used to display each individual result, see the [Cards](/result-cards) section for more details
  - property: noResults
    type: object
    description: Configuration for what to display when no results are found
    properties:
      - property: template
        type: string
        description: Handlebars template for the no results component
      - property: displayAllResults
        type: boolean
        default: false
        description: Shows all results in the vertical when no results are found
---

## Background

The `VerticalResults` component is similar to `UniversalResults` but just renders a
single vertical. It displays a list of results where each result is rendered as a card.

> To learn more about how to design a full Vertical Results pages, check out the
> [Vertical Search Results](/guides/vertical-search-results-page).

## Specifying the Vertical

First, you should make sure to specify the vertical of the page.
To specify the vertical that is used on the page, the `verticalKey` should be added to the `Facets`, `SearchBar`, and initial `search` config.

> The `VerticalResults` component does not accept a `verticalKey`. It is infererred based on the other components on the page.

## Configuration

Here is an example configuration for `VerticalResults`. In this example you can see the card is customized and we are choosing to display
all results if no results are found.

```js
ANSWERS.addComponent("VerticalResults", {
  container: ".vertical-results",
  card: {
    cardType: "Standard",
    dataMappings: {
      title: (item) => item.name,
      subtitle: (item) => item.address.line1,
      details: "",
      image: (item) =>
        !item.c_advisorPhoto ? null : item.c_advisorPhoto.image.url,
      url: "#",
    },

    callsToAction: [
      {
        label: "Book Appointment",
        icon: "calendar",
        url: "#",
        analyticsEventType: "CTA_CLICK",
        target: "_self",
      },
      {
        label: "Call",
        icon: "phone",
        url: (item) => `tel:${item.mainPhone}`,
        analyticsEventType: "TAP_TO_CALL",
      },
    ],
  },
  noResults: {
    displayAllResults: true,
  },
});
```

## Customizing Result Cards

To customize the result card you should adjust the `card` property. Specifically, use the `dataMappings` and `callsToAction` to customize
the look and the feel of the card. You can learn more in the [Result Cards Guide](/result-cards).

## No Results

With version 0.13.1, you can show all results when no results return, and link to other verticals that return results. See more in our [Vertical No Results Guide](/guides/vertical-no-results)
