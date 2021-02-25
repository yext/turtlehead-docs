---
title: Vertical Search Results Page
order: 2
---

A Vertical Search Results Page displays results for a single vertical.

## Recommended Components

| Component         | Description                                                             |
| ----------------- | ----------------------------------------------------------------------- |
| `SearchBar`       | Where the user enters their Search                                      |
| `Navigation`      | Navigate between Universal and Vertical Search using tabs               |
| `SpellCheck`      | Show UI to correct user's spelling                                      |
| `Facets`          | Show a list of facets to help the user filter                           |
| `Pagination`      | Show pagination controls the help the user navigate to subsequent pages |
| `VerticalResults` | Show the results for the vertical                                       |
| `LocationBias`    | Show where the search was run from                                      |

## Example Code

Putting these components together the page looks like this:

```html title=vertical-search.html
<body>
  <div class="answers-layout">
    <div class="searchbar-layout">
      <div class="searchbar-container"></div>
      <div class="navigation-container"></div>
    </div>
    <div class="filters-and-results-layout">
      <div class="facets-container"></div>
      <div class="results-layout">
        <div class="spellcheck-container"></div>
        <div class="vertical-container"></div>
        <div class="locationbias-container"></div>
      </div>
    </div>
  </div>

  <script>
    ANSWERS.init({
      apiKey: "3517add824e992916861b76e456724d9",
      experienceKey: "answers-js-docs",
      businessId: "3215760",
      experienceVersion: "PRODUCTION",
      verticalKey: "locations",
      search: {
        verticalKey: "locations",
        defaultInitialSearch:
          "locations that take mastercard and offer free wifi",
        limit: 20
      },
      verticalPages: [
        {
          label: "All",
          url: "/index.html",
          isFirst: true
        },
        {
          verticalKey: "locations",
          label: "Locations",
          url: "/locations.html",
          isActive: true
        },
        {
          verticalKey: "jobs",
          label: "Jobs",
          url: "/jobs.html"
        },
        {
          verticalKey: "faqs",
          label: "FAQs",
          url: "/faqs.html"
        }
      ],
      onReady: function () {
        // init components
        this.addComponent("SearchBar", {
          container: ".searchbar-container",
          allowEmptySearch: true,
          verticalKey: "locations"
        });
        this.addComponent("Navigation", {
          container: ".navigation-container"
        });
        this.addComponent("SpellCheck", {
          container: ".spellcheck-container"
        });
        this.addComponent("Facets", {
          container: ".facets-container",
          verticalKey: "locations",
          searchOnChange: true
        });
        this.addComponent("VerticalResults", {
          container: ".vertical-container",
          verticalKey: "locations",
          card: {
            cardType: "Standard",
            dataMappings: {
              title: (item) => item.name,
              subtitle: (item) => item.address.line1,
              details: (item) => item.description,
              image: (item) =>
                item.photoGallery ? item.photoGallery[0].image.url : null,
              url: "#"
            },

            callsToAction: [
              {
                label: "Get Directions",
                icon: "directions",
                url: "#",
                analyticsEventType: "GET_DIRECTIONS",
                target: "_self"
              },
              {
                label: "Call",
                icon: "phone",
                url: (item) => `tel:${item.mainPhone}`,
                analyticsEventType: "TAP_TO_CALL"
              }
            ]
          },
          noResults: {
            displayAllResults: true
          },
          appliedFilters: {
            show: true,
            removable: true
          }
        });
        this.addComponent("LocationBias", {
          container: ".locationbias-container"
        });
      }
    });
  </script>
</body>

```

## UX Recommendations

Vertical Result pages can come in all shapes and sizes. Here are some recommendations
to consider when designing a Vertical Search Results page.

### Show all results on load

Generally, you want to display ALL results for a vertical as soon as the page loads. To
to this:

- Set `allowEmptySearch: true` on the `SearchBar` component
- Set `defaultInitialSearch: ""` on the `search` config

### Display ALL Results on No Results

You never want your user to hit a dead end. To help solve this, you can easily
display ALL results if the user would see no results. To do this set `displayAllResults`
in the `noResults` configuration option in the `VerticalSearch` component.

For example:

```js
noResults: {
	displayAllResults: true,
}
```

To learn more about no results check out the [Vertical No Results Guide](/guides/vertical-no-results).

## Example

Here is a fully working example:
{{% codesandbox vertical-results-page-6ol7c %}}

