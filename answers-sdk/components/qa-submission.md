---
title: QA Submission
order: 12
component: true
appliesTo: Both
apiProperties:
  - property: entityId
    type: string
    required: true
    description: the Entity ID of the organization entity in the Knowledge Graph.
  - property: privacyPolicyUrl
    type: string
    required: true
    default:  
    description: URL for the privacy policy link. 
  - property: formSelector
    default: Native form node within container
    type: string
    description: Optional, the form selector to use in the component.
  - property: sectionTitle
    type: string
    default: Ask a Question
    description: Optional, title displayed in the heading for the form.
  - property: teaser
    type: string
    default: Can't find what you’re looking for? Ask a question below.
    description: Teaser displayed for the form, next to the title.
  - property: expanded
    type: boolean
    default: true
    description: whether or not the form is expanded by default when a user arrives on the page.
  - property: description
    type: string
    default: Enter your question and contact information, and we'll get back to you with a response shortly.
    description: Description that displays above the name and email, once the form is expanded
  - property: nameLabel
    type: string
    default: Name
    description: Optional, label for name input
  - property: emailLabel
    type: string
    default: Email
    description: Optional, label for email input.
  - property: questionLabel
    type: string
    default: Question
    description: Optional, label for question input.
  - property: privacyPolicyText
    type: string
    default: By submitting my email address, I consent to being contacted via email at the address provided.
    description: Text before the privacy policy link.
  - property: privacyPolicyUrlLabel
    type: string
    default: Learn more here.
    description: Display text for the privacy policy url.
  - property: privacyPolicyErrorText
    type: string
    default: You must agree to the privacy policy to submit feedback.
    description: Error message displayed when the privacy policy is not selected.
  - property: emailFormatErrorText
    type: string
    default: Please enter a valid email address.
    description: Error message displayed when an invalid email is not submitted
  - property: requiredInputPlaceholder
    type: string
    default: (required)
    description: Placeholder displayed in all required fields
  - property: buttonLabel
    type: string
    default: Submit
    description: Label displayed on the button to submit a question.
  - property: questionSubmissionConfirmationText
    type: string
    default: Thank you for your question!
    description: Confirmation displayed once a question is submitted.
  - property: networkErrorText
    type: string
    default: We're sorry, an error occurred.
    description: Error message displayed when there is an issue with the QA Submission request.
---
## Background
To fully set up this module, we recommend going through our [Hitchhiker Training](https://hitchhikers.yext.com/tracks/answers-advanced/ans322-q-and-a-component/), which covers how to add create an organization entity, enable the Q&A feature and answer questions.

QASubmission is a form that appears at the bottom of the [Universal Search Page](/guides/universal-search-results-page) or vertical search page. It allows a user to submit a question, along with their name and email. If a user is unable to find what s/he is looking for, they can submit a question.

When a user submits a question, it creates an instance of first party Q&A within Yext. This is available in the listings tab, under Q&A. You can then answer questions within Yext. The answer is sent to the end user’s email (inputted when s/he submitted a question). 

![QASubmission](/img/docs/qa-submission.png)

## Basic Configuration
There are only two required attributes to add QASubmission -- a `privacyPolicyUrl` and an `entityId` for the corresponding organization `entityId`. 

```html
<div class="question-submission-container"></div>
```

```js
ANSWERS.addComponent('QASubmission', {
  container: ".question-submission-container",
  entityId: 'org-1',
  privacyPolicyUrl: 'https://mybiz.com/policy',
})

```

## Example
In this more advanced example, we've updated the `teaser`, the `description` and the `buttonLabel`.

{{% codesandbox condescending-kowalevski-j2plf %}}
