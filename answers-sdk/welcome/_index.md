---
title: Welcome to Answers JS
component: false
order: 1
---

Answers JS is a JavaScript library that helps you build search experiences
on top of the Yext Answers product.
# Quick Start
Looking to quickly try out an Answers experience? The following example uses a basic test account; if you'd like to use your own, replace `apiKey`, `experienceKey` and `businessId` with your own values. 

## CSS
Copy-paste the stylesheet `<link>` into your `<head>` before all other stylesheets to load our CSS.
```html
<link
      rel="stylesheet"
      type="text/css"
      href="https://assets.sitescdn.net/answers/v1/answers.css"
    />
```
## JS
Next, add the JS lib. Place the following `<script>` in your `<head>` tag. 
```html
<script src="https://assets.sitescdn.net/answers/v1/answers.js"></script>
```
## HTML
Next, you'll need to place some `<div>`s in in your page's `<body>`. These `<div>`s have class names that match those specified in the component configuration, and is where the library will place the components. 

```html
<div class="answers-layout">
    <div class="search-bar"></div>
    <div class="spell-check"></div>
    <div class="direct-answer"></div>
    <div class="universal-results"></div>
    <div class="location-bias"></div>
</div>
```

## Initialize the Library
Last but not least, initialize the JS lib with an `apiKey`, `experienceKey` and `businessId`, (we've also added an explicit `exprienceVersion` and information on `verticalPages`), and add the components to the page:  

```html

<script>
ANSWERS.init({
  apiKey: "3517add824e992916861b76e456724d9",  //sample test experience
  experienceKey: "answers-js-docs", //sample test experience
  businessId: "3215760",  //sample test experience
  experienceVersion: "PRODUCTION",
  onReady: function () {
      // init components
      this.addComponent("SearchBar", {
      container: ".search-bar"
      });
      this.addComponent("SpellCheck", {
      container: ".spell-check"
      });
      this.addComponent("DirectAnswer", {
      container: ".direct-answer"
      });
      this.addComponent("UniversalResults", {
      container: ".universal-results"
      });
      this.addComponent("LocationBias", {
      container: ".location-bias"
      });
  }
});
</script>
```

## Starter Template
Put it all together, your pages should look like this:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Answers Test Experience</title>
    <!-- Answers CSS-->
    <link
      rel="stylesheet"
      type="text/css"
      href="https://assets.sitescdn.net/answers/v1/answers.css"
    />

    <!-- Answers Layout styling -->
    <style>
      .answers-layout {
        max-width: 50rem;
        margin: auto;
      }
    </style>

    <!-- Answers JS-->
    <script src="https://assets.sitescdn.net/answers/v1/answers.min.js"></script>
  </head>
  <body>
    <div class="answers-layout">
      <div class="search-bar"></div>
      <div class="spell-check"></div>
      <div class="direct-answer"></div>
      <div class="universal-results"></div>
      <div class="location-bias"></div>
    </div>

    <script>
      ANSWERS.init({
        apiKey: "3517add824e992916861b76e456724d9",  //sample test experience
        experienceKey: "answers-js-docs",  //sample test experience
        businessId: "3215760",  //sample test experience
        experienceVersion: "PRODUCTION",
        onReady: function () {
          // init components
          this.addComponent("SearchBar", {
            container: ".search-bar"
          });
          this.addComponent("SpellCheck", {
            container: ".spell-check"
          });
          this.addComponent("DirectAnswer", {
            container: ".direct-answer"
          });
          this.addComponent("UniversalResults", {
            container: ".universal-results"
          });
          this.addComponent("LocationBias", {
            container: ".location-bias"
          });
        }
      });
    </script>
  </body>
</html>
```


Here is a fully working example:
{{% codesandbox blissful-franklin-3t9o0 %}}

# Next

Now you're up and running with a basic [Universal Results Page](/guides/universal-search-results-page)! Head over to [Core Concepts](/core-concepts/) to learn more about how to use the Answers JS library. 