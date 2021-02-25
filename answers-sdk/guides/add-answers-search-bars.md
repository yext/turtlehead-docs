---
title: Add Answers Search Bar
order: 4
---

## Background

Generally a website will have a search bar persistent in the header and might also have a search bar more
prominently on the homepage. These search bars will redirect users to a central search results page to show
the results.

![Two Search Bars on Homepage in Pink](/img/docs/search-bar-homepage.png)

This guide will walk you through how to implement these search bars.

## Before You Begin

Before you begin make sure you have the following pieces of information

- `experienceKey` - Experience Key (supplied by a Yext Admin)
- `businessId` - Business ID (supplied by a Yext Admin)
- `apiKey` - Live API Key (supplied by a Yext Admin)
- `locale` - Locale of the experience (supplied by a Yext Admin)
- `redirectUrl` - This is the url of the search results page

There are three steps to adding a Search Bar

1. Initalize the Library
2. Place the Search Bar
3. Style the Search Bar

## 1. Initalize the Library

To add the search bar to a page we will use the Answers JS library. The first thing we need to do is add the
Answers JS library to the site via a CDN and then initialize Answers. This library will need to exist on every page we have a search bar. If the search
bar is in the header this will mean it's on every page on the website.

Add this into the `<head></head>` on every page that has a search bar. Make sure you replace
any of the properties you collected above in the script below. These are all prefixed with
`REPLACE_ME`.

```html
<head>
  <!-- Other stuff in the head here -->
  <script>
    function initAnswers() {
      ANSWERS.init({
        apiKey: "REPLACE_ME_API_KEY",
        experienceKey: "REPLACE_ME_EXPERIENCE_KEY",
        experienceVersion: "PRODUCTION",
        locale: "en", // e.g. en
        businessId: "REPLACE_ME_BUSINESS_ID",
        onReady: function() {
          ANSWERS.addComponent("SearchBar", {
            container: ".search_form",
            name: "search-bar", //Must be unique for every search bar on the same page
            redirectUrl: "REPLACE_ME_SEARCH_RESULT_URL",
            promptHeader: "You can ask:",
            placeholderText: "Search...",
          });
        },
      });
    }
  </script>
  <script
    src="https://assets.sitescdn.net/answers/v0.13.2/answers.min.js"
    onload="ANSWERS.domReady(initAnswers)"
    async
    defer
  ></script>
</head>
```

You can also configure the text labels (`placeholderText`, `searchText`, `promptHeader`) in
the `addComponent` call above to fit your needs.

## 2. Place the Search Bar

Next you will want to place the search bar DOM element where you want the search bar located. This
element should replace your existing search bar implementation. This search bar comes
pre-wired with autocomplete and other features. Add the following `div` where you want the search
bar located.

```html
<div class="search_form"></div>
```

> Note that the `container` property in the init call above `.search_form` matches the class of the div `search_form`.
> These need to match each other.

This new search bar should entirely replace the old one.
For example if your navigation bar looks something like this today:

```html
<nav>
  <!-- OTHER ELEMENTS HERE -->
  <div>
    <input placeholder="Search Here!" />
  </div>
</nav>
```

You should update it to look like this:

```html
<nav>
  <!-- OTHER ELEMENTS HERE -->
  <div class="search_form"></div>
</nav>
```

## 3. Style the Search Bar

There are two ways to style the search bar. You can either start with the default Yext styling
or you can start with no styling. We recommend starting with the Yext styling and then
customizing from there.

To use the Yext Styling, import the Yext CSS from the CDN. You should put this in the `<head>` of
every page that has a search bar. Feel free to put it right next to the script you added before.

```html
<link
  rel="stylesheet"
  type="text/css"
  href="https://assets.sitescdn.net/answers/v0.13.1/answers.css"
/>
```

To adjust styling, here are the most important css classes.

| Class                                   | Details                                     |
| --------------------------------------- | ------------------------------------------- |
| `.yxt-SearchBar-container`              | Controls border and radius of the container |
| `.yxt-SearchBar-input`                  | The input itself                            |
| `.yxt-SearchBar-button`                 | Search Button                               |
| `.yxt-SearchBar-clear`                  | Clear Button                                |
| `.yxt-SearchBar-form`                   | Form                                        |
| `.yxt-AutoComplete-wrapper`             | Wrapper around autocomplete                 |
| `.yxt-AutoComplete-option`              | Autocomplete option                         |
| `.yxt-AutoComplete-option.yxt-selected` | Selected autocomplete (using keyboard)      |

Here are some example search bars styled in different ways. Click **Open Sandbox** to
see the code that renders the search bars.

https://codesandbox.io/s/search-bar-styling-mh542?file=/index.html

## Adding a second Search Bar

If you want to add second search bar to the page (potentially one in the header and one on the homepage) then you must
use a unique `name` and `container` for each one. Here is an example of the JavaScript init:

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

## Typed Animation

One nice UI trick to encourage more users to search is to show a user typing in the input. While the Answers JS
library doesn't support this by default, this can easily be added using [Typed.js](https://github.com/mattboldt/typed.js/).

First we will get the options via the `/autocomplete` endpoint and then we feed those options into the Typed. This should happen after the
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

{{% codesandbox search-bar-styling-mh542 %}}

## Troubleshooting

Here are some common mistakes that could be causing you problems.

- Make sure the `container` property matches the div class. So if the `container` is `.search-bar-container`
  the div should have a class of `search-bar-container`. The `.` just means it's a class.
- If there are multiple search bars on the page make sure they all have a unique `name`.
- Make sure that the JS library is loaded from the CDN and that init is called.

## Search Bar API

Here is the full list of api properties that are available as part of `addComponent`

| Property             | Type      | Default         | Description                                                                         |
| -------------------- | --------- | --------------- | ----------------------------------------------------------------------------------- |
| `container`          | `string`  |                 | Required                                                                            |
| `verticalKey`        | `string`  |                 | Required if search bar is used on a Vertical Search Page                            |
| `title`              | `string`  | `null`          | Title above the search bar                                                          |
| `query`              | `string`  | `null`          | The initial search term in the search bar.                                          |
| `labelText`          | `string`  | `null`          | Label for the search bar. Hidden by default.                                        |
| `submitText`         | `string`  | `null`          | The label of the submit button. This is hidden by default.                          |
| `customIconUrl`      | `string`  | `null`          | An custom icon URL to replace the default search icon.                              |
| `promptHeader`       | `string`  | `null`          | Text label above autocomplete.                                                      |
| `placeholderText`    | `string`  | `null`          | Placeholder text of the search bar                                                  |
| `autoFocus`          | `boolean` | `false`         | When page loads auto-focus on the search bar.                                       |
| `autocompleteOnLoad` | `boolean` | `false`         | If the page is auto-focusing on the search bar, should it show autocomplete.        |
| `searchCooldown`     | `number`  | `300`           | Number of miliseconds                                                               |
| `promptForLocation`  | `boolean` | `true`          | asks the user for their geolocation when "near me" intent is detected               |
| `clearButton`        | `boolean` | `true`          | displays an "x" button to clear the current query when true                         |
| `redirectUrl`        | `string`  | `null`          | redirect search query to url                                                        |
| `formSelector`       | `string`  | `form`          | Form Selector that should be used if a custom template is used.                     |
| `inputEl`            | `string`  | `js-yext-query` | Input Element that is required if a custom template is used where the input varies. |
| `name`               | `string`  |                 | Use the name property to support multiple search bars on a single page              |
