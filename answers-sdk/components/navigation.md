---
title: Navigation
order: 2
component: true
appliesTo: Both
apiProperties:
  - property: ariaLabel
    default: Search Page Navigation
    type: string
    description: Optional, the aria-label to set on the navigation.
  - property: mobileOverflowBehavior
    type: string
    default: COLLAPSE
    description: Optional, controls if navigation collapses to a more tab, or uses inner scroll on mobile. Options are `COLLAPSE` or `INNERSCROLL`. 
  - property: overflowLabel
    type: string
    default: More
    description: Optional, the label to display on the dropdown menu button when it overflows.
  - property: overflowIcon
    type: string
    default: three stacked dots
    description: Optional, name of the icon to show on the dropdown button instead when it overflows.
---


## Background
The navigation component is made of up of tabs that link to universal and vertical search pages. You'll typically see it appear just below the search bar. 

When conducting a search from universal, the tabs will reorder based on the order of the search results. By default, tabs that do not fit in the container will be placed inside a dropdown menu. This can be configured for mobile (see `mobileOverflowBehavior` in API Properties below). 

The pages that appear in the navigation are configured in [the ANSWERS.init's verticalPages configuration](/initialization-options/vertical-pages). 
![Full Width (Mobile + Desktop)](/img/docs/nav-full-width.png)
![Collapsed (Mobile + Desktop)](/img/docs/nav-collapsed.png)
![Overflow (Optionally on Mobile)](/img/docs/nav-inner-scroll.png)

> We don’t recommend dynamically changing the width of the overflow button, as this can impact it’s behavior unintentionally (there is logic that checks for changes to Navigation width to update the More dropdown content). If you need to include a border on the button, we recommend using an `outline` instead.

## Configuration

```html
<div class="navigation-container"></div>
```

```js
ANSWERS.addComponent('Navigation', {
  container: '.navigation-container',
  // mobileOverflowBehavior: 'COLLAPSE',
  // ariaLabel: 'Search Page Navigation',
  // overflowLabel: 'More',
  // overflowIcon: null
})
```

## Example
{{% codesandbox mystifying-mountain-6p8b3 %}}

