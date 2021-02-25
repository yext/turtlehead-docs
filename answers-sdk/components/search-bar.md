---
title: Search Bar
order: 1
component: true
appliesTo: Both
apiProperties:
  - property: verticalKey
    type: string
    description: The vertical for the search bar. Required if used in vertical search, and not included in the [top level search configuration](/core-concepts/initialization).
  - property: query
    type: string
    description: The initial search term in the search bar. Does not apply a query
  - property: labelText
    type: string
    default:
    description: Label for the search bar. Hidden by default.
  - property: submitText
    type: string
    default:
    description: The label of the submit button. Hidden by default.      
  - property: customIconUrl
    type: string
    default:
    description: A custom icon URL to replace the default search icon.    
  - property: promptHeader
    type: string
    default:
    description: Text label displayed above the autocomplete options. 
  - property: placeholderText
    type: string
    description: Placeholder text of the search bar 
  - property: allowEmptySearch
    type: boolean
    default: false
    description: On vertical search only, allow a user to conduct an empty search. Should be set to true if the defaultInitialSearch in the [top level search configuration](/core-concepts/initialization) is "".         
  - property: autoFocus
    type: boolean
    default: false
    description: Auto-focus the search bar on page load. 
  - property: autocompleteOnLoad
    type: boolean
    default: false
    description: If `autoFocus` is true, show or display the autocomplete prompts.
  - property: searchCooldown
    type: number
    default: 300
    description: The number of miliseconds to wait before conducting new searches. Guards against someone rage-clicking the search bar. 
  - property: promptForLocation
    type: boolean
    default: true
    description: Ask the user for their geolocation when "near me" intent is detected.
  - property: clearButton
    type: boolean
    default: true
    description: Display an "x" button to clear the current query.      
  - property: redirectUrl
    type: string
    default: true
    description: Redirect the search query to url
  - property: formSelector
    type: string
    default: form
    description: Form Selector that is provided to the search bar template
  - property: inputEl
    type: string
    default: js-yext-query
    description: Input Element that is required if a custom template is used where the input varies.
  - property: useForm
    type: boolean
    default: true
    description: When true, a form is used as the query submission context. Note it's WCAG best practice to set this to `true`.
  - property: geolocationOptions
    type: object
    description: Geolocation-related configuration. See [Geolocation Documentation](https://developer.mozilla.org/en-US/docs/Web/API/PositionOptions) for more information.
    properties:
      - property: enableHighAccuracy
        type: boolean
        default: false
        description: Whether to improve accuracy at the cost of response time and/or power consumption, defaults to false.
      - property: timeout
        type: number
        default: 1000
        description: The maximum amount of time (in ms) a geolocation call is allowed to take before defaulting, defaults to 1 second. 
      - property: maximumAge
        type: number
        default: 300000
        description: The maximum amount of time (in ms) to cache a geolocation call, defaults to 5 minutes.
  - property: geolocationTimeoutAlert
    type: object
    description: Configuration for a native alert, displayed when a call to the geolocation API fails. 
    properties:
      - property: enabled
        type: boolean
        default: false
        description: Whether to display a window.alert() on the page
      - property: message
        type: string
        default: "We are unable to determine your location"
        description: If `enabled` is true, the message in the alert.

---

The `SearchBar` component is the main entry point for search querying.
It provides the input box where the user types their query, as well as the autocomplete behavior.
Search Bars can be used in both Vertical and Universal Search.

## Default Styling

Here is the default styling for a search bar:
![Search Bar](/img/docs/search-bar.png)

And here is the default styling for a search bar with autocomplete showing:
![Search Bar Expanded](/img/docs/search-bar-expanded.png)

## Universal Search

Here is an example of adding a search bar to Universal Search:

```html
<div class="search-bar-container"></div>
```

```js
ANSWERS.addComponent("SearchBar", {
  container: ".search-bar-container",
});
```

## Vertical Search

Here is an example of adding a search bar to Vertical Search.
It's the same as Universal Search but requires an additional `verticalKey`.


```html
<div class="search-bar-container"></div>
```

```js
ANSWERS.addComponent("SearchBar", {
  container: ".search-bar-container",
  verticalKey: "VERTICAL_KEY", // required if not specified in search config in initialization.
});
```

## Redirecting Search

Often you want to add a Search Bar to a page but then redirect to a search results page.
This is accomplished via the `redirectUrl` property. If this property is filled out,
the search will redirect when a user hits submit. This URL should be a search results page that includes
a [`UniversalResults`](/components/universal-results) or [`VerticalResults`](/components/vertical-results) component.

## Using HTML5 Geolocation

By default, when a user types a query with "near me" intent (IE "locations near me"), we'll prompt them to grant access to HTML5 geolocation. If you do not want this behavior, you can set the `promptForLocation` to false. If you notice consistently lethargic geolocation requests, you can reduce the timeout (`geolocationOptions.timeout`) or increase the maximum age for a geolocation request (`geolocationOptions.maximumAge`), and add an optional alert if the request fails (`geolocationTimeoutAlert`). 

Finally, if the accuracy of geolocation information is particularly important, you can enable "high accuracy", which will provide more accurate geolocation results at the expense of response time and/or device power consumption. 

More information on `enableHighAccuracy`, `timeout` and `maximumAge` can be found in the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/API/PositionOptions).
```js
ANSWERS.addComponent("SearchBar", {
  container: ".search-bar-container",
  redirectUrl: "https://www.domain.com",
  geolocationOptions: {
    timeout: 500,
    maximumAge: 300000,
    enableHighAccuracy: false
  },
  geolocationTimeoutAlert: {
    enabled: true,
    message: "We are unable to determine your location"
  }

});
```

## Multiple Search Bars

Often you will want to have multiple search bars on a single page. For example one in the header and one in the center
of the home page. To accomplish this make sure each search bar has a unique `container` and `name`.

```js
ANSWERS.addComponent("SearchBar", {
  container: ".search-bar-container",
  name: "search-bar",
  redirectUrl: "https://www.domain.com/search-results",
});

ANSWERS.addComponent("SearchBar", {
  container: ".search-bar-container-2",
  name: "search-bar-2",
  redirectUrl: "https://www.domain.com/search-results",
});
```

## Typing Placeholder

One nice UI trick to encourage more users to search is to show a user typing in the input. While the Answers JS
library doesn't support this by default, this can easily be added using [Typed.js](https://github.com/mattboldt/typed.js/).

First, we will get the options via the `/autocomplete` endpoint and then we feed those options into the Typed. This should happen after the
`SearchBar` component is added to the page. We are using `axios` to handle the API request.

```js
var url = `https://liveapi-cached.yext.com/v2/accounts/me/answers/autocomplete`;
url += `?v=20190101`;
url += `&api_key=${apiKey}`;
url += `&sessionTrackingEnabled=false`;
url += `&experienceKey=${experienceKey}`;
url += `&input=`;
url += `&version=${version}`;
url += `&locale=${locale}`;

axios.get(url).then(function(response) {
  // Get strings from response
  const strings = response.data.response.results.map(function(r) {
    return r.value;
  });

  // Set up Typed
  var options = {
    strings,
    showCursor: true,
    cursorChar: "|",
    typeSpeed: 45,
    backSpeed: 20,
    smartBackspace: true,
    loop: true,
    startDelay: 500,
    backDelay: 2000,
    attr: "placeholder",
  };

  var typed = new Typed(".js-yext-query", options);
});
```

Here is a working example:

{{% codesandbox typed-with-answers-demo-yuzsm %}}


