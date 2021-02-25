---
title: Built In Cards
order: 4
---

## Standard Card

The data mappings for a standard card has these attributes:

```js
const dataMappings = (item) => {
  return {
    // Title for the card, defaults to the name of the entity
    title: item.title,
    // Subtitle, defaults to null
    subtitle: `Department: ${item.name} `,
    // Details, defaults to the entity's description
    details: item.description,
    // Image to display, defaults to null
    image: item.headshot ? item.headshot.url : "",
    // Url for the title/subtitle, defaults to the entity's website url
    // Note, a protocol like https://yext.com is required, as opposed to just yext.com
    url: item.link || item.website,
    // Character limit to hide remaining details and display a show more button, defaults to 350
    showMoreLimit: 350,
    // Text for show more button, defaults to 'Show More'
    showMoreText: "show more",
    // Text for show less button, defaults to 'Show Less'
    showLessText: "put it back",
    // The target attribute for the title link, defaults to '_self'. To open in a new window use '_blank'
    target: "_blank",
    // Whether to show the ordinal of this card in the results, i.e. first card is 1 second card is 2,
    // defaults to false
    showOrdinal: false,
    // A tag to display on top of an image, always overlays the image, default no tag
    tagLabel: "On Sale!",
  };
};
```

## Accordion Card

The data mappings for an accordion card has these attributes:

```js
const dataMappings = (item) => {
  return {
    // Title for the card, defaults to the name of the entity
    title: item.title,
    // Subtitle, defaults to null
    subtitle: `Department: ${item.name} `,
    // Details, defaults to the entity's description
    details: item.description,
    // Whether the first Accordion Card shown in vertical/universal results should be open on page load, defaults to false
    expanded: false,
  };
};
```

## Legacy Card

The Legacy Card is very similar to the Standard Card, but with the legacy DOM structure and class names
from before v0.13.0. New users should not use the Legacy Card; instead, use the Standard Card.

The data mappings for a legacy card has these attributes:

```js
const dataMappings = (item) => {
  return {
    // Title for the card, defaults to the name of the entity
    title: item.title,
    // Subtitle, defaults to null
    subtitle: `Department: ${item.name} `,
    // Details, defaults to the entity's description
    details: item.description,
    // Image to display, defaults to null
    image: item.headshot ? item.headshot.url : "",
    // Url for the title/subtitle, defaults to the entity's website url
    url: item.link || item.website,
    // The target attribute for the title link, defaults to '_self'. To open in a new window use '_blank'
    target: "_blank",
    // Whether to show the ordinal of this card in the results, i.e. first card is 1 second card is 2,
    // defaults to false
    showOrdinal: false,
  };
};
```
