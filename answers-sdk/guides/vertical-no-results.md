---
title: No Results
order: 3
---

With version 0.13.1, you can show all results when no results return, and link to other verticals that return results.  


## Basic Template
In version 0.13.1 and up, you can specify `noResults` in your [verticalResults](/components/vertical-results) component: 
```js
ANSWERS.addComponent("VerticalResults", {
  container: ".vertical-results-container",
  card: {
    cardType: "Standard",
    dataMappings: () => {},
    callsToAction: () => [],
  },
  noResults: {
     displayAllResults: true, //controls whether all results appear below. defaults to false if not specified.  
  }
});
```

## Overwriting the Template
It’s very easy to override the no results template. Note: We did this because the logic is a little bit complicated (like iterating through the list of all alternative verticals, and deciding when to use plural vs. singular for “result” and “category”). We wanted you to still have access to those fancy things, even if you just wanted to customize part of the string. 

You have access to the following Handlebars variables in the no results template:
* The url for universal search: universalUrl
* The current vertical: currentVerticalLabel
* The query that was created: query
* Results count for all results: resultsCount 
* The set of all alternative verticals that returned results: verticalSuggestions

You can see these in the following code sandbox:
https://codesandbox.io/embed/blazing-breeze-uj9m7?fontsize=14&hidenavigation=1&module=%2Ftemplates.js&theme=dark

```js
ANSWERS.addComponent("VerticalResults", {
  container: ".vertical-results-container",
  card: {
    cardType: "Standard",
    dataMappings: () => {},
    callsToAction: () => [],
  },
  noResults: {
     displayAllResults: true, //controls whether all results appear below. defaults to false if not specified.  
     template:  `
      <div class="yxt-AlternativeVerticals{{#unless isShowingResults}} yxt-AlternativeVerticals--notShowingResults{{/unless}}">
      <div class="yxt-AlternativeVerticals-noResultsInfo">
        <em class="yxt-AlternativeVerticals-noResultsInfo--emphasized">
          No results found
        </em>
        in {{currentVerticalLabel}}.
        {{#if isShowingResults}}Showing<em class="yxt-AlternativeVerticals-noResultsInfo--emphasized">all {{currentVerticalLabel}}</em>instead.{{/if}}
      </div>
      {{#if verticalSuggestions}}
        <div class="yxt-AlternativeVerticals-suggestionsWrapper">
          <div class="yxt-AlternativeVerticals-details">
            The following search
            {{plural verticalSuggestions.length 'category' 'categories'}}
            yielded results
            {{#if query}}
              for
              <span class="yxt-AlternativeVerticals-details--query">
                "{{query}}"</span>:
            {{/if}}
          </div>
          <ul class="yxt-AlternativeVerticals-suggestionsList">
            {{#each verticalSuggestions}}
              <li class="yxt-AlternativeVerticals-suggestion">
                <a class="yxt-AlternativeVerticals-suggestionLink"
                    href="{{url}}">
                  {{#if iconName}}
                    <div class="yxt-AlternativeVerticals-verticalIconWrapper"
                          data-component="IconComponent"
                          data-opts='{
                            "iconName": "{{iconName}}"
                          }'>
                    </div>
                  {{/if}}
                  <div class="yxt-AlternativeVerticals-suggestionLink--copy">
                    <span class="yxt-AlternativeVerticals-suggestionLink--copyLabel">
                      {{label}}
                    </span>
                    <span class="yxt-AlternativeVerticals-suggestionLink--copyResults">
                      ({{resultsCount}} {{plural resultsCount 'result' 'results'}})
                    </span>
                  </div>
                  <div class="yxt-AlternativeVerticals-arrowIconWrapper"
                        data-component="IconComponent"
                        data-opts='{
                          "iconName": "chevron"
                        }'>
                  </div>
                </a>
              </li>
            {{/each}}
          </ul>
          {{#if universalUrl}}
            <div class="yxt-AlternativeVerticals-universalDetails">
              Alternatively, you can
              <a class="yxt-AlternativeVerticals-universalLink"
                 href={{universalUrl}}>
                view results across all search categories</a>.
            </div>
          {{/if}}
        </div>
      {{/if}}
    </div>
      `
  }
});
```

## FAQs

### 1. Can this apply to universal? 
Not yet but this is on the roadmap. -- the construct is going to be a little different. 
### 2. How does this work for third party backends?
Third party backends do not get the no results functionality. 
### 3. How do I change the color of the background or the styling?
Overwrite any of the scss here: https://github.com/yext/answers/blob/master/src/ui/sass/modules/_AlternativeVerticals.scss 
