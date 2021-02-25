---
title: onVerticalSearch
order: 4
component: false
---

## Background
The `onVerticalSearch` Configuration accepts a function. It allows you to send an analytics event each time a search is run on a Vertical page. This function should take in one parameter, searchParams, which contains information about the search, and return the desired analytics event.

Like all Answers Javascript API Library analytics, this will only work if there is a `businessId` in the `ANSWERS.init`.

> `queryString` will be undefined for searches initiated by pagination / applying facets/filters/sorts even if there is a query being used.

## Usage
The search information exposed in searchParams is shown below.

```js
function (searchParams) => {
    /**
     * Vertical key used for the search.
     * @type {string}
     */
    const verticalKey = searchParams.verticalKey;

    /**
     * The string being searched for.
     * @type {string}
     */
    const queryString = searchParams.queryString;

    /**
     * The total number of results found.
     * @type {number}
     */
    const resultsCount = searchParams.resultsCount;

    /**
     * Either 'normal' or 'no-results'.
     * @type {string}
     */
    const resultsContext = searchParams.resultsContext;

    let analyticsEvent = new ANSWERS.AnalyticsEvent('ANALYTICS_EVENT_TYPE');
    analyticsEvent.addOptions({
      label: 'Sample analytics event',
      searcher: 'VERTICAL',
      query: queryString,
      resultsCount: resultsCount,
      resultsContext: resultsContext,
    });
    return analyticsEvent;
  },
}),
```