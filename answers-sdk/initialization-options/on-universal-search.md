---
title: onUniversalSearch
order: 3
component: false
---

## Background
The `onUniversalSearch` attribute accepts a function. It allows you to send an analytics event each time a search is run on a Universal page. This function should take in one parameter, searchParams, which contains information about the search, and return the desired analytics event.

Like all Answers Javascript API Library analytics, this will only work if there is a businessId in the `ANSWERS.init`.

## Usage

The search information exposed in searchParams is shown below.
```js
function (searchParams) => {
    /**
     * The string being searched for.
     * @type {string}
     */
    const queryString = searchParams.queryString;

    /**
     * The number of verticals returned.
     * @type {number}
     */
    const sectionsCount = searchParams.sectionsCount;

    /**
     * A map containing entries of the form:
     * { totalResultsCount: 150, displayedResultsCount: 10}
     * for each returned vertical. The totalResultsCount indicates how many results
     * are present in the vertical. The displayResultsCount indicates how many of
     * those results are actually displayed.
     * @type {Object<string,Object>}
     */
    const resultsCountByVertical = searchParams.resultsCountByVertical;

    let analyticsEvent = new ANSWERS.AnalyticsEvent('ANALYTICS_EVENT_TYPE');
    analyticsEvent.addOptions({
      type: 'ANALYTICS_EVENT_TYPE',
      label: 'Sample analytics event',
      searcher: 'UNIVERSAL',
      query: queryString
      sectionsCount: sectionsCount,
    });
    return analyticsEvent;
  },
}),
```