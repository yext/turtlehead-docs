---
title: Card Calls To Action
order: 3
---

Calls to Action (CTAs) are the key actions of a result card. In the example below each card has two CTAs, Call and Directions.

![Card CTAs (call, directions)](/img/docs/result-cards.png)

CTAs provide a visual button in the card and also record analytics events to make it easier to understand
user bevhavior.

> **Note:** A CTA without both a label and link will not be rendered.

## Adding CTAs to Cards

`callsToActions` are added to the card object like so:

```js
ANSWERS.addComponent("VerticalResults", {
  /* ...other vertical results config... */
  card: {
    /* ...other card config...*/
    callsToAction: (item) => [
      {
        label: (item) => item.name,
        url: "https://yext.com",
      },
    ],
  },
  /* ...other vertical results config... */
});
```

## Static List of CTAs

The easiest way to specify CTAs is as an array of objects. Every CTA must have a `label`, `url`, `analytics` and `eventOptions`. (See [below](#eventoptions) for more information on `eventOptions`.) 

```js
const callsToAction = [
  {
    label: "cta label",
    icon: "star",
    url: "https://yext.com",
    analytics: "CTA_CLICK",
    target: "_blank",
    eventOptions: (result) => {
      return {
        verticalKey: "people",
        entityId: result.id,
        searcher: "VERTICAL",
      };
    },
  },
];
```

If you want to use content from the item (e.g. the URL) to populate the CTA, you can return a function to any of the properties.
In this example we are setting the label and URL to properties from the item.

```js
const callsToAction = (item) => [
  {
    label: (item) => item.name,
    url: (item) => item.url,
    analytics: "CTA_CLICK",
    target: "_self",
    icon: "briefcase",
    eventOptions: (item) =>
      `{ 
				"verticalKey": "credit-cards", 
				"entityId": "${item.id}", 
				"searcher":"UNIVERSAL", 
				"ctaLabel": "cards"
			}`,
  },
];
```

## Dynamic List of CTAs

If you want to vary the types of CTAs by card, you can return a function to this property instead of a static array. This function
should return an array, but it could vary its length or contents based on the item.

```js
const callsToAction = (item) => [
  {
    label: item.name,
    url: "https://yext.com",
    analytics: "CTA_CLICK",
    target: "_blank",
    icon: "briefcase",
    eventOptions: `{ 
			"verticalKey": "credit-cards", 
			"entityId": "${item.id}", 
			"searcher":"UNIVERSAL", 
			"ctaLabel": "cards"
		}`,
  },
  {
    label: "call now",
    url: "https://maps.google.com",
    analytics: "CTA_CLICK",
    target: "_blank",
    icon: "phone",
    eventOptions: `{
			"verticalKey": "credit-cards", 
			"entityId": "${item.id}", 
			"searcher":"UNIVERSAL",
			"ctaLabel": "cards"}
		`,
  },
];
```

## `eventOptions`
In order for a click to a CTA to register in Answers Analytics, you'll need to set the `eventOptions` attribute on the CTA. This attribute should return either a valid json string or an object. 

### Format
#### Event Options as a valid JSON string
```js
eventOptions: `{
  "verticalKey": "credit-cards", 
  "entityId": "${item.id}", 
  "searcher":"UNIVERSAL",
  "ctaLabel": "buy" 
}`
```

#### Event Options as an Object
```js
  eventOptions: result => {
    return {
      verticalKey: 'people',
      entityId: result.id,
      searcher: 'VERTICAL'
    };
  }
```

### Properites:

- `verticalKey`: The vertical key for the CTA. If unspecified, it will default to the vertical of the card on which this CTA is placed (either from VerticalResults or UniversalResults). 
- `entityId`: The entity ID for the card on which this CTA is placed.  If unspecified, it will default to the `entityId` field in Knowledge Graph
- `searcher`: `VERTICAL` or `UNIVERSAL`. If the CTA is on a card inside a vertical search, it will default to the `VERTICAL`. If it's on a card inside a universal search, it will default to `UNIVERSAL`.
- `ctaLabel`: An optional additional label sent with the analytics request. For example, if you had two CTAs on a card, one that was "Buy Now" and the other that was "Get Directions", you might add the `ctaLabel`s "Buy" and "Directions", respectively.
