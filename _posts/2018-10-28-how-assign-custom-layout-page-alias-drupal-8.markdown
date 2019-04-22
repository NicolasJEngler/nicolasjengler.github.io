---
layout: post
title:  "How to assign a custom layout to a page according to its URL alias in Drupal 8"
date:   2018-10-28 12:00:00 -0300
categories: 'development'
---

### Originally on [DEV Community](https://dev.to/nicolasjengler/how-to-assign-a-custom-layout-to-a-page-according-to-its-url-alias-in-drupal-8-en6)

For the past couple of weeks I've been working as a contractor for an NYC-based NGO that's using Drupal 8 to power their site. My first task was to move their styleguide from a design done in Figma to actual components built with HTML and CSS inside Drupal, but there was a trick: the styleguide had to be set up as some sort of general type content and all I was able to customize was the URL alias.

I went on and duplicated the `page.html.twig` in our subtheme in order to customize that and work there, later renamed the file to `page--styleguide.html.twig`. After that I created all related content and used a URL alias that was `styleguide` happily knowing that (as stated by Drupal's documentation) it would pick up the custom layout automatically without the need of indicating anything.

_**Narrator:** the custom layout wasn't picked up by Drupal_

I wasn't able to find why the custom layout was not being used, so I decided to do some googling and found a useful function to suggest Drupal which layout to use. After some tweaking I ended up with the following snippet (placed inside my `.theme` file)

```php
// Set up hook
function themename_theme_suggestions_page_alter(array &$suggestions, array $variables) {

  // Check if the content being loaded is a node
  if ($node = \Drupal::routeMatch()->getParameter('node')) {
    
    // Get the node's ID
    $nid = $node->id();

    // Get the URL alias based on the ID of the node
    $alias = \Drupal::service('path.alias_manager')->getAliasByPath('/node/'.$nid);

    // Check if the URL alias matches the styleguide's
    if ($alias == '/styleguide') {
      
      // Suggest Drupal to use whatever custom layout we have defined inside /templates
      $suggestions[] = 'page__styleguide';
    }
  }
}
```

I'm by no means a Drupal expert, quite the contrary, so I'm not entirely sure if this solution is properly executed or performant for that matter. If you know a better way to solve this, feel free to leave a comment!