---
title: verticalPages
order: 1
component: false
appliesTo: Both
description: verticalPages takes an array. Each object in the array has the following properties.
apiProperties:
  - property: label
    type: string
    required: true
    description: The label for this page, used in both the `Navigation` and no results
  - property: url
    type: string
    required: true
    description: The link to this page, used in `Navigation`, no results and `UniversalResults` 
  - property: verticalKey
    type: string
    required: false
    description: The verticalKey, **required for vertical search pages**, must be omitted for universal search  
  - property: icon
    type: string
    required: false
    description: The icon associated with this vertical, used in no results
  - property: iconUrl
    type: string
    required: false
    description: the url of the icon associated with this vertical, used in no results. Takes precedence over `icon`.  
  - property: isFirst
    type: boolean
    required: false
    description: If true, will show this page first in `Navigation`
  - property: isActive
    type: boolean
    required: false
    description: If true, will add active styling to this page in `Navigation` 
  - property: hideInNavigation
    type: boolean
    required: false
    description: If true, hide this tab in the `Navigation`     
---

## Configuration
`verticalPages` is included in the initialization, and is used in various components that require information about the vertical search pages, including [Navigation](/components/navigation), [Universal Results](/components/universal-results) and [No Results](/guides/vertical-no-results) in [Vertical Results](/components/vertical-results). It takes an array of objects, each representing a vertical search page. 

```js
ANSWERS.init({
  apiKey: "3517add824e992916861b76e456724d9",
  experienceKey: "answers-js-docs",
  businessId: "3215760",
  experienceVersion: "PRODUCTION",
  verticalPages: [
  {
    label: "Home",
    url: "index.html",
    icon: "star",
    isFirst: true,
    isActive: true,
    hideInNavigation: false
  },
  {
    label: "Locations",
    url: "/locations.html",
    verticalKey: "locations",
    icon: "pin",
    isFirst: false,
    isActive: false,
    hideInNavigation: false
  },
  // Other pages here
],
  onReady: function() {
    // ADD COMPONENTS HERE
  },
});

```
## Where Is It Used?
| Option        | No Results           | `UniversalResults`  | `Navigation`  |
| ------------- |--------------------- | ------------------- |----------- |
| `label`       | Used in alernative verticals. | Not used | Used as tab label |
| `url`         | Used in alernative verticals | Used in "View More" link if not specified| Used as tab URL |
| `icon`       | Used in alernative verticals. | Not used | Not used |
| `isFirst`       | Not used | Not used | Used to determine which tab is pinned to the first slot |
| `isActive`       | Not used | Not used | Used to determine which tab gets active state |
| `hideInNavigation`       | Not used | Not used | Used to determine which tabs are hidden |

## Recommended Configuration
### `isFirst` and `isActive`
The navigation component should ideally always display universal search as the first tab (`isFirst`), and the current page as active (`isActive`). It's therefore recommended that your Universal tab is always set to `isFirst: true` on each page, and the current page is set to `isActive: true`.

### `hideInNavigation`
In the case that you have a vertical that you would like to remove from the navigation, but still maintain a page for, you can toggle the `hideInNavigation` boolean as needed. All other configuration will still be passed to the `Navigation`, no results and `UniversalResults`. 

## Example
{{% codesandbox competent-cartwright-0qcyz %}}
