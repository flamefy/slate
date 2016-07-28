# API Reference

## FlmAudience

### apiToken

Audience unique API token

Returns **string** Flamefy audience token

### id

Audience unique ID

Returns **integer** Flamefy Audience unique identifier

### register

Registers or updates an existing audience

**Parameters**

-   `data` **Object** Audience attributes
    -   `data.last_name` **string** Last name. Mandatory
    -   `data.email` **string** Email. Mandatory if no twitter_name
    -   `data.twitter_name` **string** Twitter name. Mandatory if no email
    -   `data.phone` **[string]** Phone number in international format (+33000000000)
    -   `data.zip_code` **[string]** Zip code
    -   `data.first_name` **string** First name. Mandatory
    -   `data.title` **[string]** Title. Must be in ['mr', 'miss', 'mme', 'dr']
    -   `data.job_title` **[string]** Job title
    -   `data.country_code` **[string]** ISO-2 country code ('FR' 'US'...)
    -   `data.external_id` **[string]** User identifier in the client system
    -   `data.extra` **[Object]** Custom fields ({ field1: '1', field2: '2' })
    -   `data.company` **[string]** Company name
-   `writeMode` **[string]** Write mode for attributes
                                             Accepted values are 'overwrite' and 'merge' (optional, default `'overwrite'`)

Returns **Promise** Result of the action with created/updated Audience

### registerViewer

Registers an audience and creates a viewer for it in a story

**Parameters**

-   `storyId` **integer** Id of the story
-   `data` **Object** Audience attributes
    -   `data.first_name` **string** First name. Mandatory
    -   `data.last_name` **string** Last name. Mandatory
    -   `data.email` **string** Email. Mandatory if no twitter_name
    -   `data.twitter_name` **string** Twitter name. Mandatory if no email
    -   `data.phone` **[string]** Phone number in international format (+33000000000)
    -   `data.company` **[string]** Company name
    -   `data.title` **[string]** Title. Must be in ['mr', 'miss', 'mme', 'dr']
    -   `data.job_title` **[string]** Job title
    -   `data.country_code` **[string]** ISO-2 country code ('FR' 'US'...)
    -   `data.external_id` **[string]** User identifier in the client system
    -   `data.extra` **[Object]** Custom fields ({ field1: '1', field2: '2' }
    -   `data.zip_code` **[string]** Zip code
-   `writeMode` **[string]** Write mode for attributes. Accepted values are
    'overwrite' and 'merge' (optional, default `'overwrite'`)

Returns **Promise** Result of the action with created/updated Audience

## FlmEvent

### id

Event unique ID

Returns **integer** Flamefy Event unique identifier

### create

Create an event for an Audience in a Scenario

**Parameters**

-   `eventType` **string** Event type
-   `storyId` **integer** Flamefy Story identifier
-   `stepPosition` **integer** Position of the step to attach the event
-   `data` **[Object]** Audience identifier
                                          Must have one of the followinf property: flm_api_token, external_id, audience_id
                                          If blank, it will try to use the flm_api_token cookie value (optional, default `{flm_api_token: FlmCookie.apiToken}`)
    -   `data.flm_api_token` **[string]** Audience flamefy token (possibly stored in cookie)
    -   `data.external_id` **[string]** User identifier in the client system
    -   `data.audience_id` **[integer]** Audience unique identifier

Returns **Promise**

## FlmTag

Flamefy Tag

### add

Assign a tag to an audience

**Parameters**

-   `tag` **string** Tag to assign
-   `data` **[Object]** Audience identifier.
                                          Must have one of the followinf property: flm_api_token, external_id, audience_id
                                          If blank, it will try to use the flm_api_token cookie value (optional, default `{flm_api_token: FlmCookie.apiToken}`)
    -   `data.flm_api_token` **[string]** Audience flamefy token (possibly stored in cookie)
    -   `data.external_id` **[string]** User identifier in the client system
    -   `data.audience_id` **[integer]** Audience unique identifier

**Examples**

```javascript
FlmTag.add('foo', { flm_api_token: currentAudience.apiToken })
FlmTag.add('foo', { external_id: currentAudience.attributes.external_id })
FlmTag.add('foo', { audience_id: currentAudience.id })
```

Returns **Promise**

## FlmViewer

Flamefy Viewer

**Parameters**

-   `storyId`  

### action

Base url for backend API

**Parameters**

-   `storyId` **integer** Flamefy Story identifier

Returns **string**

### create

Create an audience for a story

**Parameters**

-   `storyId` **integer** Flamefy Story identifier
-   `data` **[Object]** Audience identifier.
                                          Must have one of the followinf property: flm_api_token, external_id, audience_id
                                          If blank, it will try to use the flm_api_token cookie value (optional, default `{flm_api_token: FlmCookie.apiToken}`)
    -   `data.flm_api_token` **[string]** Audience flamefy token (possibly stored in cookie)
    -   `data.external_id` **[string]** User identifier in the client system
    -   `data.audience_id` **[integer]** Audience unique identifier

**Examples**

```javascript
FlmViewer.create(42, { flm_api_token: currentAudience.apiToken })
FlmViewer.create(42, { external_id: currentAudience.attributes.external_id })
FlmViewer.create(42, { audience_id: currentAudience.id })
```

Returns **Promise**
