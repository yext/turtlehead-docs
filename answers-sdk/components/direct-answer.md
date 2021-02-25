---
title: DirectAnswer
order: 4
component: true
appliesTo: Universal
apiProperties:
  - property: formEl
    type: string
    default: '.js-directAnswer-feedback-form'
    description: The selector for the form used for submitting the feedback
  - property: thumbsUpSelector
    type: string
    default: '.js-directAnswer-thumbUp'
    description: The selector to bind ui interaction to for the thumbs up button
  - property: thumbsDownSelector
    type: string
    default: '.js-directAnswer-thumbDown'
    description: The selector to bind ui interaction to for the thumbs down button
  - property: positiveFeedbackSrText
    type: string
    default: 'This answered my question'
    description: The screen reader text for the thumbs up button.
  - property: negativeFeedbackSrText
    type: string
    default: 'This did not answer my question'
    description: The screen reader text for the thumbs down button.
  - property: viewDetailsText
    type: string
    default: 'View Details'
    description: The display text for the View Details click to action link, which is the website URL of the entity.
  - property: footerTextOnSubmission
    type: string
    default: 'Thank you for your feedback!'
    description: The footer text to display on submission of feedback
  - property: defaultCard
    type: string
    description: Optionally specify a custom direct answer card to use, which is the default when there are no matching card overrides. 
---

## Background

The `DirectAnswer` component shows a Direct Answer to a query. It is usually shown above the `UniversalResults` component. DirectAnswers are only returned on Universal Search. 

## Default Styling

Here is the default styling for a Direct Answer:
![Direct Answer](/img/docs/direct-answer/direct-answer-before.png)

## Base Configuration

Here's a basic example of adding a direct answer.
```html
<div class="direct-answer-container"></div>
```
```js
ANSWERS.addComponent("DirectAnswer", {
  container: ".direct-answer-container",
});
```

## Formatting Data in a Direct Answer
You can format data in a direct answer using the `transformData` hook outlined [here](/advanced-concepts/custom-data-transforms#example).

## Example
In this example, we've overridden the `viewDetailsText` and the `footerTextOnSubmission`. We're also using a [Custom Data Transform](/advanced-concepts/custom-data-transforms) to override the formatting for a phone number.

{{% codesandbox hopeful-meitner-wzfbf %}}
