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
You need to be a registered customer and get your 'api key' before doing any calls to this API.

# Getting Started

> Don't forget to change the API Key, if you don't have one, please contact us.

```javascript
<script>
  (function(d,s,v,c,e,y){
    e = d.createElement(s), y = d.getElementsByTagName(s)[0];
    e.async = 1; e.src = v;
    if (c) { e.addEventListener('load', function (e) { c(null, e); }, false); }
    y.parentNode.insertBefore(e,y)
  })(document,'script','https://cdn.flamefy.com/js/flm-api/flm-js-1-0-0.js', function () {
    initFlmJs('CHANGE_ME')
    // First params is the API key for the application
  });
</script>
```

Before starting anything, you have to embed our JS library and initialize it with your API Key.<br>
An api key is an access token provided by our technical team. 
This string generation process is based on your calling URL domain and subdomains to guarantee safety and security access to our servers.</p>

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

To track and then manage audiences, FlameFy uses different mechanisms including, for web usages, regular cookies.
You can disable the tracking with cookies if you have some privacy rules to follow.

<aside class="notice">
You will have to keep track of the created audience and pass the audience identifier to every viewer/tag/event API call.
</aside>

# Audience Management

## Concepts

<p>FlameFy is an Audience management system, meaning it collects actions done by end users across the web (via a browser), a mobile application or any other interaction with a software or an internet platform. <br>
<p>So, an <strong>Audience</strong> is an object describing an end-user according to two main information types: it's identity (email, name, etc.) and it's behavior (clicks, log in, likes, etc.).  All events building up a behavior are named <strong>Views</strong> and are contextualized (timestamp, IP address, etc.)</p>
<p> Audience have different status, according to this rules:
<ul>
<li> Anonymous: we have no information about the end user, either because no cookies have been set (privacy rules) or we have no detailed data; </li>
<li> Known: we have in database at least one information (email, external_id, name) that makes it no more anonymous; </li>
<li> Passive: the Audience is anonymous or known but has no Views data, classically this is a new Audience (just created or imported); </li> 
<li> Active: the Audience is anonymous or known and has one or more Views data.</li>
</ul>
</p>

## Register or Retrieve

You can create an Audience and immediately attach an interaction by adding a Viewer, all in a single call.</br>
To register a new audience in your FlameFy database, you can send different information that make it anonymous our know. You will have to add some Viewers to make this Audience active. </br>

This registration is typically called when en end-user register to your VOD platform, your emailing list or any moment you capture an identity via a form.</br>

This action will set a cookie name <code>flm_api_token</code> to keep trace of the audience unless you set the parameter allowCookies to false.

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

Registering an Audience with no information will make it anonymous.

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

A <strong>Viewer</strong> object represents an end-user interaction with a service. 
In FlameFy, we use a <strong>Scenario</strong> object to represent a touch point between an end-user and any system (a platform like Facebook, Twitter, a website, any digitally based service, etc.).

Then to register this interaction, you have to add to your Audience a new Viewer with the scenario_id of the Scenario previously created in the FlameFy back-end. You can precise, using an external_id data, to which Audience the Viewer will be added or let FlameFy find it (using cooky and sessions information).

You will attach viewers every time a known Audience will do something with your service like starting to watch a movie (VOD), browsing a page (media), etc.

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

You can create an Audience and immediately attach an interaction by adding a Viewer, all in a single call.

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

A <strong>Tag</strong> is a simple label that can be set to a lot of different objects in FlameFy, including Viewers, Videos, and more.
 It allows to tag end-users interactions and feed our machine learning algorithms to build up people behaviors and predictions.
 There are no rules about syntax with tags, neither length limit.
 
A classical case is that you add a Tag 'SciFi lover' to and Audience that has a lot of Viewers related to interactions on content about Science-Fiction.</p>

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

# Managing Events
## Logging an event in a Story

During the execution of a Story, different <strong>Events</strong> can be performed and then logged by the system.
Available events differ according to the current step.

For example, if a Step includes a video player, then clicks on 'play', 'pause', etc... will be logged in.
Events can also be custom ones. Any event_code sent to the API which is not known will be treated as a custom new one.

To log an Event, you must provide the the event_code, the running scenario_id and the current stepPosition.

Available Events are:

- For audio, video, picture player: video_play video_pause, video_finished, chromecast_on chromecast_off
   airplay_on
- For generic player : social_share

```javascript
FlmEvent.create('anEventCode', storyId, stepPosition, { flm_api_token: currentAudience.apiToken })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition, { external_id: currentAudience.attributes.external_id })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition, { audience_id: currentAudience.id })
// OR
FlmEvent.create('anEventCode', storyId, stepPosition)
```
