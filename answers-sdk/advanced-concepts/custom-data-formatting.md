---
title: Custom Data Formatting
order: 4
component: false
---
> Note there are known bugs with this feature, proceed with caution. 

You can format specific entity fields using fieldFormatters. These formatters are applied before the transformData step.

Each formatter takes in an object with the following properties:

- entityProfileData
- entityFieldValue
- highlightedEntityFieldValue
- verticalId
- isDirectAnswer

Below is an example usage.

```js
ANSWERS.init({
  apiKey: '<API_KEY_HERE>',
  experienceKey: '<EXPERIENCE_KEY_HERE>',
  fieldFormatters: {
    'name': (formatterObject) => formatterObject.entityFieldValue.toUpperCase(),
    'description' : (formatterObject) => formatterObject.highlightedEntityFieldValue
  }
});
```