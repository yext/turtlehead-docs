---
title: SpellCheck
order: 3
component: true
appliesTo: Both
apiProperties:
  - property: suggestionHelpText
    type: string
    default: Did you mean
    description: Did you mean text to displays before the query
---

## Background

The `SpellCheck` component helps correct a user's spelling.
![Spell Check](/img/docs/spellcheck.png)
It is usually displayed above the results. It works on both Universal Search and Vertical Search and
is generally added directly above the `VerticalResults` or `DirectAnswer`.

## Configuration

The component is named `SpellCheck`. There are no additional required properties.
```html
<div class="spell-check-container></div>
```
```js
ANSWERS.addComponent("SpellCheck", {
  container: ".spell-check-container",
  // suggestionHelpText: "Are you sure you didn't mean: ",
});
```

## Example
{{% codesandbox cranky-taussig-mrgij %}}

