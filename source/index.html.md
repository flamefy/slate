---
title: FlameFy API Documentation Version 1.0 (c) 2016

language_tabs:
  - javascript

includes:
  - api_reference

search: true
---

# Introduction

Welcome to FlameFy API. 
This documentation introduces the different ways to send data to FlameFy and manage audiences.

# Getting Started

> Don't forget to change the API Key, if you don't have one, please contact us.

```javascript
<script>
  (function(d,s,v,c,e,y){
    e = d.createElement(s), y = d.getElementsByTagName(s)[0];
    e.async = 1; e.src = v;
    if (c) { e.addEventListener('load', function (e) { c(null, e); }, false); }
    y.parentNode.insertBefore(e,y)
  })(document,'script','https://s3-eu-west-1.amazonaws.com/flm-api/flm-js-1-0-0.js', function () {
    initFlmJs('CHANGE_ME', 'http://flamefy.dev:3000/api')
    // First params is the API key for the application
    // last params is optional, default to http://api.flamefy.com/api
  });
</script>
```

Before start anything, you have to embed our JS library and initialize it with your API Key.

<aside class="notice">
You must replace <code>CHANGE_ME</code> with your FlameFy API key.
</aside>

# Disable Cookies

```javascript
//...
initFlmJs('YOUR_API_KEY_HERE')
FlmJs.allowCookies = false;
//...

// OR
document.addEventListener("flmjs:loaded", function() {
  FlmJs.allowCookies = false;
});
```

For some reasons you will have to disable cookies, here is how you can do it.

<aside class="notice">
You will have to keep track of the createad audience and pass the audience identifier to every viewer/tag/event API call
</aside>

# Audience

## Register or Retrieve

```javascript
FlmAudience.register({
  first_name: 'Jean',
  last_name: 'Dupont',
  email: 'foo.bar@test.com',
  twitter_name: 'test',
  phone: '+33000000000',
  zip_code: '75100',
  address: '1 Rue du Parc, Paris',
  company: 'World Company',
  title: 'mr',
  job_title: 'Owner',
  country_code: 'FR',
  external_id: 'XXXXXX',
  extra: { field1: '1', field2: '2' }
}, 'merge').then(function(audience) {
  window.currentAudience = audience
});
```
<aside class="notice">
This action will set a cookie name <code>flm_api_token</code> to keep trace of the audience
</aside>

## Register anonymous

> Optionally, if the apiToken cookie is set, you can pass it to the method

```javascript
FlmAudience.register({}, 'merge').then(function(audience) {
window.currentAudience = audience
})

// Or with apiToken cookie

FlmAudience.register({ flm_api_token: FlmCookie.apiToken }, 'merge').then(function(audience) {
  window.currentAudience = audience
})
```

## Attach to a scenario (create a viewer)

```javascript
FlmViewer.create(storyId, { flm_api_token: currentAudience.apiToken })
// OR
FlmViewer.create(storyId, { external_id: currentAudience.attributes.external_id })
// OR
FlmViewer.create(storyId, { audience_id: currentAudience.id })
// OR
FlmViewer.create(storyId)
```

<aside class="notice">
Call with one parameter will use the stored cookie to identify the audience
</aside>

## Create for a scenario

```javascript
FlmAudience.registerViewer(storyId, {
  first_name: 'Jean',
  last_name: 'Dupont',
  email: 'foo.bar@test.com',
  twitter_name: 'test',
  phone: '+33000000000',
  zip_code: '75100',
  address: '1 Rue du Parc, Paris',
  company: 'World Company',
  title: 'mr',
  job_title: 'Owner',
  country_code: 'FR',
  extra: { field1: '1', field2: '2' }
}, 'overwrite').then(function(audience) { window.currentAudience = audience })
```

<aside class="notice">
This action will set a cookie name flm_api_token to keep trace of the audience
</aside>

## Add a tag

```javascript
FlmTag.add('BigData', { flm_api_token: currentAudience.apiToken })
// OR
FlmTag.add('BigData', { external_id: currentAudience.attributes.external_id })
// OR
FlmTag.add('BigData', { audience_id: currentAudience.id })
// OR
FlmTag.add('BigData')
```

<aside class="notice">
Call with one parameter will use the stored cookie to identify the audience
</aside>

## Create an event in a Story

```javascript
FlmEvent.create('anEventCode', storyId, stepPosition, { flm_api_token: currentAudience.apiToken })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition, { external_id: currentAudience.attributes.external_id })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition, { audience_id: currentAudience.id })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition)
```
