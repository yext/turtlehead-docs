---
title: Location Bias
order: 15
component: true
appliesTo: Both
apiProperties:
  - property: verticalKey
    type: string
    description: If used on Vertical Search Page, the `verticalKey` for the vertical. Required if not included in [top level search configuration](/core-concepts/initialization).
  - property: updateLocationEl
    type: string
    default: .js-locationBias-update-location
    description: Selector for button a user clicks to update their location
  - property: ipAccuracyHelpText
    type: string
    default: based on your internet address
    description: Text displayed when geolocation fails, and IP address is used to determine location.
  - property: deviceAccuracyHelpText
    type: string
    default: based on your device
    description: Text displayed when geolocation succeeds, and HTML5 is used to determine location.
  - property: updateLocationButtonText
    type: string
    default: Update your location
    description: Label of the button to update your location
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
        default: 6000
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

## Background

The location bias shows the user where we think they are located.
The purpose of this component is to inform the user as well as to allow them to opt in for HTML5 geolocation.
It is recommended to display this at the bottom of any search results page.

> Note: If you want to allow a user to select a location check out the `GeoLocationFilter` component [here](/components/geolocationfilter).

## Default Styling

If we are using IP address to locate the user, the component looks something like this:
![Location Bias (IP Address)](/img/docs/location-bias-ip.png)

If the user has opted in to HTML5 Gelocation, then the component looks something like this:
![Location Bias (Device)](/img/docs/location-bias-device.png)

## Basic Configuration

The only required option is `verticalKey` when this is used on Vertical Search Results Page (if not included in [top level search configuration](/core-concepts/initialization)).

```html
<div class="location-bias-container"></div>
```

```js
ANSWERS.addComponent("LocationBias", {
  container: ".location-bias-container",
  // verticalKey: "VERTICAL_KEY"
});
```
## Advanced Configuration

### HTML5 Geolocation
Similar to the [SearchBar component](/components/search-bar), you can adjust the options passed to the [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/getCurrentPosition).

You can reduce the timeout (`geolocationOptions.timeout`) or increase the maximum age for a geolocation request (`geolocationOptions.maximumAge`) and add an optional alert if the request fails (`geolocationTimeoutAlert`). 

Additionally, if the accuracy of geolocation accuracy is particularly important, you can enable "high accuracy", which will provide more accurate geolocation results at the expense of response time and/or device power consumption. 

```js
ANSWERS.addComponent("LocationBias", {
  container: ".location-bias-container",
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
### Overriding Strings
You can also override the default strings used in the component using the following attributes:
- `ipAccuracyHelpText`: Text displayed when geolocation fails and IP address is used to determine location
- `deviceAccuracyHelpText`: Text displayed when geolocation succeeds and HTML5 is used to determine location.
- `updateLocationButtonText`: Button text used to re-request location information.

```js
ANSWERS.addComponent("LocationBias", {
  container: ".location-bias-container",
  ipAccuracyHelpText: 'based on your internet address',
  deviceAccuracyHelpText: 'based on your device',
  updateLocationButtonText: 'Update your location',
});
```

## Example
{{% codesandbox nice-surf-hyc28 %}}

