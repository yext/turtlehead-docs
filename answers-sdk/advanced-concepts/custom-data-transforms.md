---
title: Custom Data Transforms
order: 2
---

## Background
Sometimes, you might want to alter the data recieved by the Answers API before it's passed along to the template for rendering. For example, say you've set a direct answer on the phone number field. Before the raw phone number is passed to the template, you'll likely want to format it to add parenthesis and dashes.

If you want to mutate the data that's provided to the render/template before it gets rendered, you can use the `transformData` hook.

## Usage
All properties and values that you return from `transformData` will be accessible from templates.

```js
ANSWERS.addComponent('SearchBar', {
  container: '.search-container',
  transformData: (data) => {
    // Extend/overide the data object
    return Object.assign({}, data, {
      title: data.title.toLowerCase()
    })
  }
})

```

## Example
Here's an example of using a custom data transform for the [DirectAnswer component](/components/direct-answer).

### Code
{{% codesandbox hopeful-meitner-wzfbf %}}

### Explanation

To start, our `DirectAnswer` component returns phone numbers that look like this:

![Direct Answer Before Formatting](/img/docs/direct-answer/direct-answer-before.png)

First, we'll create a function that formats the `DirectAnswer` value for phone number, keying off of the `fieldType`:
```js
function formatDirectAnswer(fieldName, fieldType, value) {

  //adapted from https://stackoverflow.com/a/8358185
  if (fieldType == "phone") {
    return value
      .replace(/\D+/g, "")
      .replace(/(\d{1})(\d{3})(\d{3})(\d{4})/, "$1 ($2) $3-$4");
  }
  return value;
}
```

Next, in the `DirectAnswer` component, we'll call said function within `transformData`:

```js
this.addComponent('DirectAnswer', {
    container: '.direct-answer-container',   
    transformData: (data) => {
        //data has fieldName, fieldType, value
        return {
            ...data,
            answer: {
                ...data.answer,
                value: formatDirectAnswer(data.answer.fieldName, data.answer.fieldType, data.answer.value)
            }
        }
    }
});
```

Since `transformData` is called before the data is passed to the template, it'll format the phone number as stipulated. Now, when phone number is returned, it'll look like this:

![Direct Answer Before Formatting](/img/docs/direct-answer/direct-answer-after.png)

