---
title: Universal vs. Vertical Search
order: 3
---

Before diving into building an experience, it's important to understand the difference between Vertical and
Universal Search.

## Universal Search

Universal Search is a search page that combines results from multiple different backends (vertical)
together. For example, a Universal Search page might combine locations, events, people and FAQ into
one search experince.

A Universal Search page should use the [`UniversalResults`](components/universal-results) components for displaying results. Generally,
universal search is labeled with the `All` tab in the navigation.

## Vertical Search

Vertical Search is a search page that only shows results from a single backend (vertical). For example,
a Vertical Search page might show a list of locations, or a list of events.

A Vertical Search page should use the [`VerticalResults`](components/vertical-results) components for displaying results. Additionally,
the [`SearchBar`](components/search-bar) component must have the `verticalKey` property set.
