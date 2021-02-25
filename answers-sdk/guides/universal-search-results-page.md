---
title: Universal Search Results Page
order: 1
---

A Universal Search Results Page combines results from multiple verticals into a set of results.

## Recommended Components

| Component          | Description                                    |
| ------------------ | ---------------------------------------------- |
| `SearchBar`        | Where the user enters their Search             |
| `Navigation`       | Navigate between Universal and Vertical Search |
| `SpellCheck`       | Show UI to correct user's spelling             |
| `DirectAnswer`     | Show a direct answer if one is found           |
| `UniversalResults` | Show the results                               |
| `LocationBias`     | Show where the search was run from             |

## Code

Putting these components together the page looks like this:

```html title=universal-search.html
<body>
  <div class="answers-layout">
    <div class="searchbar-container"></div>
    <div class="navigation-container"></div>
    <div class="spellcheck-container"></div>
    <div class="universal-container"></div>
    <div class="directanswer-container"></div>
    <div class="locationbias-container"></div>
  </div>

  <script>
    ANSWERS.init({
      apiKey: "3517add824e992916861b76e456724d9",
      experienceKey: "answers-js-docs",
      businessId: "3215760",
      experienceVersion: "PRODUCTION",

      onReady: function () {
        // init components
        this.addComponent("SearchBar", {
          container: ".searchbar-container",
          allowEmptySearch: true
        });
        this.addComponent("Navigation", {
          container: ".navigation-container",
        });
        this.addComponent("SpellCheck", {
          container: ".spellcheck-container",
        });
        this.addComponent("DirectAnswer", {
          container: ".directanswer-container",
        });
        this.addComponent("UniversalResults", {
          container: ".universal-container",
          config: {
            locations: {
              icon: "pin",
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
              appliedFilters: {
                show: true,
                showFieldNames: true,
                hiddenFields: ["builtin.entityType"],
                delimiter: "|",
                labelText: "Filters applied to this search:",
                removableLabelText: "Remove this filter"
              }
            },
            faqs: {
              icon: "support",
              title: "FAQs",
              card: {
                cardType: "Accordion",
                dataMappings: {
                  title: (item) => item.name,
                  details: (item) => item.answer,
                  url: "#"
                },
                callsToAction: [
                  {
                    label: "Learn More",
                    icon: "",
                    url: "#",
                    analyticsEventType: "CTA_CLICK",
                    target: "_self"
                  }
                ]
              }
            },
            jobs: {
              icon: "briefcase",
              card: {
                cardType: "Standard",
                dataMappings: {
                  title: (item) => item.name,
                  subtitle: (item) => `Date Posted: ${item.datePosted}`,
                  details: (item) => item.description,
                  url: "#"
                },
                callsToAction: [
                  {
                    label: "Learn More",
                    icon: "info",
                    url: "#",
                    analyticsEventType: "CTA_CLICK",
                    target: "_self"
                  }
                ]
              }
            }
          }
        });
        this.addComponent("LocationBias", {
          container: ".locationbias-container",
        });
      }
    });
  </script>
</body>
```

## Example

Here is a fully working example:
{{% codesandbox universal-results-page-p3jzv %}}
