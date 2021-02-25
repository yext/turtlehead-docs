---
title: Pagination
order: 14
component: true
appliesTo: Vertical
apiProperties:
  - property: verticalKey
    type: string
    description: The verticalKey for this vertical. Required if not included in the top level `search` configuration.
  - property: maxVisiblePagesDesktop
    type: number
    default: 1
    description: The maximum number of pages visible to users above the mobile breakpoint. 
  - property: maxVisiblePagesMobile
    type: number
    default: 1
    description: The maximum number of pages visible to users below the mobile breakpoint. 
  - property: pinFirstAndLastPage
    type: boolean
    default: false
    description: ensure that the page numbers for first and last page are always shown. Not recommended to use with showFirstAndLastButton. Defaults to false
  - property: showFirstAndLastButton
    type: boolean
    default: true
    description: Optional, display double-arrows allowing users to jump to the first and last page of results. 
  - property: pageLabel
    type: string
    default: "Page"
    description: Label for a page of results
  - property: noResults
    type: object
    description: Configuration of no results
    properties:
      - property: visible
        type: boolean
        default: false
        description: Whether pagination should be visible when displaying no results.
  - property: onPaginate
    type: function
    default: document.documentElement.scrollTop = 0; document.body.scrollTop = 0;
    description: Function invoked when a user clicks to change pages. Default scrolls user to the top of the page. `(newPageNumber, oldPageNumber, totalPages) => {}`
  - property: showFirst - DEPRECATED
    type: boolean
    default: true
    description: Display a double arrow allowing users to jump to the first page of results. DEPRECATED, please use `showFirstAndLastButton` instead.
  - property: showLast - DEPRECATED
    type: boolean
    default: true
    description: Display a double arrow allowing users to jump to the last page of results. DEPRECATED, please use `showFirstAndLastButton` instead.
---

## Background

The `Pagination` component is used on Vertical Search to allow users to navigate to other pages.
The default page size is 20 results, and pagination is used to help users navigate to results
that are not on the first page.

## Default Styling

Here is the default styling for the pagination component:

![Pagination](/img/docs/pagination-one-page.png)


## Basic Configuration

Here is a basic example of adding pagination to a vertical results page:
```html
<div class="pagination"></div>
```

```js
ANSWERS.addComponent("Pagination", {
  container: ".pagination",
});
```

### Configuring Page Size

By default, the page size is set to 20. To override the page size, edit the `limit` property on the `search`
config in the `init`. Here is an example of chaning the page size to 50:

```js
ANSWERS.init({
  // API Key, Experience Key, Business ID etc. go here
  search: {
    verticalKey: "advisors",
    limit: 50,
    defaultInitialSearch: "",
  },
});
```

### Hiding First and Last Page

By default, the pagination component will always show a button to easily navigate to the first or last page.
This is represented by a double arrow in the search results.


To disable this use the `showFirstAndLastButton` property. Here is an example or hiding first and last page controls:
![First and Last Page Controls](/img/docs/pagination-first-last-page-highlight.png)

```js
ANSWERS.addComponent("Pagination", {
  container: ".pagination",
  showFirstAndLastButton: false
});
```
### Show Multiple Pages
By default, one page is shown at a time. To show multiple page numbers (and allow a user to jump to a specific page), configure `maxVisiblePagesMobile` and `maxVisiblePagesDesktop`. The `pageLabel` can also be removed. 

![Multiple Pages](/img/docs/pagination-multiple-pages.png)

```js
ANSWERS.addComponent("Pagination", {
  container: ".pagination",
  maxVisiblePagesMobile: 5,
  maxVisiblePagesDesktop: 1,
  pageLabel: ""
});
```

### Pin First and Last Page Number
A user might want to see how many pages are available when conducting a query, especially for those verticals that lend themselves to high-result queries. It's not recommended to use this configuration option with `showFirstAndLastButton`, since the frontend behavior is redundant. 

![Pin First and Last Page Number](/img/docs/pagination-pinFirstAndLast.png)


To pin the first and last page, set the `pinFirstAndLastPage` boolean to `true`.
```js
ANSWERS.addComponent("Pagination", {
  container: ".pagination",
  maxVisiblePagesMobile: 3,
  maxVisiblePagesDesktop: 10,
  pageLabel: ""
  pinFirstAndLastPage: true,
});
```

### Trigger Behavior On Page Change
The is a function invoked when a user clicks to change pages. It accepts the "destination" page number (`newPageNumber`), the previous page number (`oldPageNumber`), and the total number of pages (`totalPages`). 

```js
ANSWERS.addComponent("Pagination", {
  container: ".pagination",
  maxVisiblePagesMobile: 3,
  maxVisiblePagesDesktop: 10,
  pageLabel: ""
  pinFirstAndLastPage: true,
  onPaginate: function(newPageNumber, oldPageNumber, totalPages) {
    console.log(
      "I'm going from " + oldPageNumber + " to " + newPageNumber +
       " and there are " + totalPages + " pages.") 
       // Logs the following when paginating from page 1 to 6: 
       // "I'm going from 1 to 6 and there are 35 pages." 
  }
});
```
## Example
Here's a fully working example:

{{% codesandbox stupefied-thunder-zdvyt %}}




