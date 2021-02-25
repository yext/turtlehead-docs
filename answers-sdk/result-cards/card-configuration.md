---
title: Built-In Card Configuration
order: 1
---

Cards have three properties: `cardType`, `dataMappings` and `callsToAction`.

```js
ANSWERS.addComponent('VerticalResults', {
  container: '.vertical-results-container',
  card: {
    cardType: 'Standard',
    dataMappings: () => {},
    callsToAction: () => []
  }
  //other properties here
}
```

## `cardType`
`cardType` accepts a string and defines which card type you'd like to use. 
There are three options: [Standard](/result-cards/built-in-cards#standard-card), the [Accordion](/result-cards/built-in-cards#accordion-card) and the [Legacy](/result-cards/built-in-cards#legacy-card).


## `dataMappings`
The `dataMappings` option define how a card's attributes, such as title and details, will be rendered. More information can be found in the [Data Mapping](/result-cards/data-mappings) section. 

## `callsToAction`

Calls to Action (CTAs) are the key actions of a result card. See [Calls To Action](/result-cards/calls-to-action) for more information.  