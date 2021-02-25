---
title: Map
order: 13
component: true
appliesTo: Both
apiProperties:
  - property: mapProvider
    default: 
    type: string
    required: true
    description: Supported map providers include `google` or `mapBox`, not case-sensitive
  - property: apiKey
    default: 
    type: string
    required: true
    description: The API Key used for interacting with the map provider, except for Google Maps if provided `clientId`
  - property: clientId
    default: 
    type: string
    required: false
    description: Can be used for Google Maps in place of the API key
  - property: collapsePins
    default: false
    type: boolean
    required: false
    description: Determines whether or not to collapse pins at the same lat/lng
  - property: zoom 
    default: 14
    type: number
    required: false
    description: the zoom level of the map
  - property: defaultPosition
    default: "{lat: 37.0902, long: -95.7129}"
    type: object
    required: false
    description: The default coordinates to display if showEmptyMap is set to true. Takes an object with two properties, `lat` and `lng`.
  - property: showEmptyMap
    default: false
    type: boolean
    required: false
    description: Determines if an empty map should be shown on the initial page load. Also used in no results if specific no results config for the map is not provided.
  - property: onPinClick
    default: 
    type: function
    required: false
    description: Callback to invoke when a pin is clicked. The clicked item(s) are passed to the callback.
  - property: onPinMouseOver
    default: 
    type: function
    required: false
    description: Callback to invoke when a pin is hovered. The hovered item(s) are passed to the callback.
  - property: onPinMouseOut
    default: 
    type: function
    required: false
    description: Callback to invoke when a pin is no longer hovered after being hovered. The clicked item(s) are passed to the callback
  - property: onLoaded
    default: 
    type: function
    required: false
    description: Callback to invoke once the JavaScript is loaded
  - property: noResults
    default: 
    type: object
    required: false
    description: 
    properties:
    - property: displayAllResults
      default: false
      type: boolean
      required: false
      description: Whether to display map pins for all possible results when no results are found.
    - property: visible
      default: false
      type: boolean
      required: false
      description: Whether to display the map when no results are found, taking priority over showEmptyMap. If unset, a map will be visible if showEmptyMap is true OR if displayAllResults is true and alternative results are returned.
  - property: pin
    default: 
    type: function
    required: false
    description: Custom configuration override to use for the map markers. Should return an object. See [here](/components/map#customizing-the-pin) 
---


## Background

The map component is displayed on both universal and vertical search pages. It's added on a [Vertical Search Page](/docs/answers-sdk/guides/vertical-search-results-page), but included as a subcomponent on a Universal Page, within [Universal Results](/docs/answers-sdk/components/universal-results) on a [Universal Search Page](/docs/answers-sdk/guides/universal-search-results-page).

![Map on Universal Page](/img/docs/map-universal.png)
![Stand-Alone Map on Vertical Page](/img/docs/map-vertical.png)

## Basic Configuration

The basic config for a map requires the `apiKey` and the `mapProvider`. All other configuration is optional. We also recommend adding the no results configuration, assuming you are displaying all results as outlined [here](/guides/vertical-no-results)

```html
<div class="map-container"></div>
```

```js
ANSWERS.addComponent('Map', {
  container: '.map-container',
  mapProvider: 'mapBox',
  apiKey: '12345',
  noResults: {
    displayAllResults: false,
    visible: false
  },
  
});
```
## Hooks
The Map component provides a few different hooks, where you can provide functions to trigger custom behavior.

### Pin Hooks
The relevant item is passed to the call back 
- `onPinClick` fires when a pin is clicked
- `onPinMouseOver` fires when a pin is hovered over.
- `onPinMouseOut` fires when a pin is no longer hovered.

### JS Hooks
- `onLoaded` fires when the JS for the map is loaded.

### Example
In the below example, we fire various alerts when pins are clicked or hovered.

```js
ANSWERS.addComponent('Map', {
  container: '.map-container',
  mapProvider: 'mapBox',
  apiKey: '12345',
  onPinClick: function (entity) { console.log("clicked " + JSON.stringify(entity));}
  onPinMouseOver: function (entity) { console.log("hovering on " + JSON.stringify(entity));}
  onPinMouseOut: function (entity) { console.log("leaving " + JSON.stringify(entity));}
  onLoaded: function () { console.log("Loaded JS")},
});
```
 

## Customizing the Pin
The `Map` component also accepts a `pin` function, that allows you to create an entirely custom pin if you're using Google Maps. Said function should return an object with the following properities:
```js
{
  svg: null, //if you're using an svg
  url: null, //if you're using an image hosted elsewhere
  anchor: null, // e.g. { x: 1, y: 1 }, offsets the pin
  scaledSize: null // e.g. { w: 20, h: 20 }, alters the size
};
```

Here's an example using a custom svg (the pin label will automatically be added to the svg):
```js
ANSWERS.addComponent('Map', {
  container: '.map-container',
  mapProvider: 'mapBox',
  apiKey: '12345',
  pin: function () {
    
    return {
      svg: `<svg
            xmlns="http://www.w3.org/2000/svg"
            width="20px"
            height="20px"
            viewBox="0 -1 22 28"
          >
            <g fill="none" fill-rule="evenodd">
              <path
                fill="#848484"
                fill-rule="nonzero"
                stroke="#848484"
                d="M0 10.767c0 5.563 7.196 12.418 9.947 14.836.6.527 1.509.53 2.112.005C14.815 23.212 22 16.423 22 10.767 22 4.82 17.075 0 11 0S0 4.82 0 10.767"
              />
              <text
                fill="#848484"
                font-family="Arial-BoldMT,Arial"
                font-size="12"
                font-weight="bold"
              >
                <tspan x="50%" y="15" text-anchor="middle">
                </tspan>
              </text>
            </g>
          </svg>`
    };
  }
});
```


## Example
{{% codesandbox awesome-forest-9yzuv %}}

