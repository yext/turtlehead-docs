---
title: Card Data Mapping
order: 2
---

The `dataMappings` option define how a card's attributes, such as title and details, will be rendered. It accepts either:

1. A function that accepts the result item from the API and returns a dataMappings object.
2. A static dataMappings object. Each attribute of a dataMappings object is also either a function or a static value.

Below is an example of dataMappings as function.

```js
ANSWERS.addComponent('VerticalResults', {
  /* ...other vertical results config... */
  card: {
    /* ...other card config...*/
    dataMappings: item => ({
      title: item.name,
      subtitle: `Department: ${item.name} `,
      details: item.description,
      image: item.headshot ? item.headshot.url : '',
      url: 'https://yext.com',
      showMoreLimit: 500,
      showMoreText: "show more",
      showLessText: "put it back",
      target: '_blank'
    })
  }
  /* ...other vertical results config... */
})
```
And below is an example of `dataMappings` as an object with functions inside it. You can use both static attributes and function attributes together.

```js
ANSWERS.addComponent('VerticalResults', {
  /* ...other vertical results config... */
  card: {
    /* ...other card config...*/
    dataMappings: {
      title: item => item.name,
      subtitle: item => `Department: ${item.name} `,
      details: item => item.description,
      image: item => item.headshot ? item.headshot.url : '',
      url: 'https://yext.com',
      showMoreLimit: 500,
      showMoreText: 'show more',
      showLessText: 'put it back',
      target: '_blank'
    }
  }
  /* ...other vertical results config... */
})
```

Each cardtype has standard attributes that it expects in the returned `dataMappings` object. You can read more about these in the [Built-In Cards Section](/result-cards/built-in-cards). 