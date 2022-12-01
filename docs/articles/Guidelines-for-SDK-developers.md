Welcome to the Kontent.ai guidelines for SDK developers! These guidelines cover all the requirements needed to create SDKs for the [Kontent.ai Delivery API](https://kontent.ai/learn/reference/delivery-api/) and should give you a solid starting point for developing SDKs in the framework of your choice.

The guidelines cover the following topics:

* [Expected SDK functionality](#expected-functionality)
* [Ensuring backward compatibility](#ensuring-backward-compatibility)
* [Format of the returned data](#returned-data)
* [Sending HTTP headers](#sending-http-headers)
* [Delivery API limits](#delivery-api-limits)
* [Naming conventions](#naming-conventions)
* [OS project requirements](#os-project-requirements)

## Expected functionality

### Required endpoints

The SDK must support the following Delivery API endpoints:

Method | Endpoint
---------|----------
GET | Retrieve a list of content items <br/> `https://deliver.kontent.ai/{project_id}/items`
GET | Retrieve a content item <br/> `https://deliver.kontent.ai/{project_id}/items/{item_codename}`
GET | Retrieve a list of content types <br/> `https://deliver.kontent.ai/{project_id}/types`
GET | Retrieve a content type <br/> `https://deliver.kontent.ai/{project_id}/types/{type_codename}`
GET | Retrieve a content type element <br/> `https://deliver.kontent.ai/{project_id}/types/{type_codename}/elements/{element_codename}`
GET | Retrieve a list of taxonomy groups <br/> `https://deliver.kontent.ai/{project_id}/taxonomies`
GET | Retrieve a taxonomy group <br/> `https://deliver.kontent.ai/{project_id}/taxonomies/{taxonomy_group_codename}`

### Feature support

The SDK should also support the following features:

Feature | Description
---------|----------
Delivery Preview API | Support for [previewing unpublished content items](https://kontent.ai/learn/tutorials/develop-apps/build-strong-foundation/set-up-preview/) via the Delivery Preview API.
Content filtering | Support for the [filtering parameters](https://kontent.ai/learn/reference/delivery-api/#tag/Filtering-content) when making queries to the Delivery API.
Localization | Support for retrieving localized content, that is using the `language` query parameter when retrieving content items.
Rendering Rich text elements | Support for rendering content (in other words, transforming HTML 5 fragments) of the built-in Rich text elements. This content includes links, images, components, and other content items.
Resolving links | Support for resolving hypertext links to other content items within the built-in Rich text elements.
Retrieving linked items | Support for retrieving content items and components that are used within other content items.

### Dynamic vs. static typing

We recommend that you use statically typed models for the SDK, if supported by a given stack. Otherwise, use dynamically typed models if your stack supports only those.

For information about content elements and their data types, see [Content element data types](#content-element-data-types) below.

## Ensuring backward compatibility

The Delivery API evolves in a way that is backward compatible. This means that, as long as SDKs and client applications follow a few rules, changes in the Delivery API won't break them.

The following changes to the Delivery API are **NOT** considered breaking changes and you should cope with them in your SDK.

* Add a new property to JSON objects.
* Change the order of JSON object properties (with exceptions, see below).
* Add a new type of content element.
* Add a new HTML element to the Rich text elements.
* Add a new attribute to HTML element in the Rich text elements.
* Add a new value to HTML element attribute in the Rich text elements.
* Change the order of HTML element attributes in the Rich text elements.
* Represent some characters as HTML entities.

**Note**: If a breaking change should happen, we will provide a new version of the Delivery API (alongside the current version of the API) together with new SDK guidelines. We will let you know in time before this happens.

You can always rely on the following features:

* Elements in content types are in the same order as in Kontent.ai UI.
* Elements in content item variants are in the same order as in content types.
* Options in Multiple choice elements in content types are in the same order as in Kontent.ai UI.
* Selected options in Multiple choice elements in content item variants are in the same order as in content types.

Here are a few tips to help you deal with backward-compatible changes in the Delivery API:

* Ignore unknown content elements.
* Parse HTML with HTML 5 parses. If possible, do not use regular expressions.
* Ignore unknown HTML elements – keep their content, but ignore their opening and closing tags.

## Returned data

The Delivery API returns data in the JSON format both for successful and [unsuccessful](#errors) requests.

### Object model descriptions of the returned data

When you query endpoints for retrieving all `/items`, `/types`, or `/taxonomies`, the Delivery API returns a paginated listing response with content that matches your specific query. In each case, the listing response contains a pagination object with information about the number of retrieved items, and a link to the next page of results.

You can find [descriptions and examples of the listing responses](https://kontent.ai/learn/reference/delivery-api/#operation/list-content-items) for content items, content types, and taxonomy groups in our API reference.

For details on the structure of the individual objects returned by the Delivery API (such as content items, content types, and taxonomy groups), see these pages in the API reference:

* [Content item model](https://kontent.ai/learn/reference/delivery-api/#section/Content-item-object)
* [Content type model](https://kontent.ai/learn/reference/delivery-api/#section/Content-type-object)
* [Taxonomy group model](https://kontent.ai/learn/reference/delivery-api/#section/Taxonomy-group-object)

### Content element data types

Content element | Data type | Limitation
--- | --- | ---
Text | string | Hard limit of 100,000 characters.
Rich text | string | Hard limit of 100,000 characters. **Note**: The limit includes HTML markup and whitespace characters.
Multiple choice | array of strings (selected options) | Can contain up to 250 predefined options.
Number | decimal | Numbers have the following format: "##########.##########" – 10 numbers before and after the decimal separator.
Date & time | string | [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) formatted string in the following format: YYYY-MM-DDTHH:MM:SSZ.
Asset | asset reference | The maximum asset size is 2 GB.
Modular content | array of strings (codenames of selected content items) | No limitation.
URL slug | string | Hard limit of 100,000 characters.
Custom element | string | Hard limit of 100,000 characters. **Note**: Custom elements behave as simple text-based elements.

You can find [example JSON values for each of the content elements](https://kontent.ai/learn/reference/delivery-api/#tag/Content-elements) in our API reference.

### Empty response vs. 404 error

The Delivery API differentiates between a query that doesn't bring any results (and gives a 200 OK response) and a query that gives a 404 Not found response.

For example, when you request a single item by codename and the item doesn't exist, the Delivery API responds with a 404 Not found. On the other hand, if you request items (with optional filter) and no item (or items matching the filtering criteria) exists, the Delivery API responds with 200 OK and returns an empty array of items. This applies to endpoints such as `/items`, `/types`, and `/taxonomies`.

### Errors

The Delivery API returns errors in the JSON format. Every error message has the same format. Here's an example of a general error message:

```json
{
  "message": "No HTTP resource was found that matches the request. Please read API documentation at https://kontent.ai/learn/reference/delivery-api.",
  "request_id": "|38a7b46fc8597a4aa1ab0169449cac54.c37263a1_",
  "error_code": 1,
  "specific_code": 0
}
```

The `message` property in the error message provides a clear specification of what went wrong when making the request.

## Sending HTTP headers

### Analytics

To gauge how much your SDK is used, we strongly recommend that the SDK sends a `X-KC-SDKID` header with each request to the API. The value of the header should be formatted like this `<SDK package origin>;<SDK name>;<SDK version>`, for example, `nuget.org;Kontent.Delivery;9.0.0` for version 9 of the [Delivery .NET SDK](https://github.com/kontent-ai/delivery-sdk-net).

This way we can identify the requests coming from your SDK and later provide you with information about its usage.

### Requesting latest content

By default, the Delivery API might serve stale content (if cached by the Fastly CDN) for performance reasons. In some cases, such as when [reacting to webhook notifications](https://kontent.ai/learn/reference/webhooks-reference/), you might want to request the latest content from your Kontent.ai project.

To do this within your SDK, include a `X-KC-Wait-For-Loading-New-Content` header and set it to `true` when sending requests to the API.

Including the header will cause the Delivery API to explicitly fetch new content from the specified Kontent.ai project.

## Delivery API limits

### Rate limitation

Delivery API requests are primarily served from a CDN network. For such requests, no rate limiting is in place. For uncached requests, a rate limitation of 100 per second and 2000 per minute is enforced. If a limit is reached, server responds with a 429 error and provides a `Retry-After` header, which tells you how long to wait before a request can be reattempted. All failed requests are safe to retry.

### URL length

The length of URLs for the GET requests is limited to 2048 characters. The Delivery API returns an [error](#errors) for URLs longer than 2048 characters.

## Naming conventions

> Check also the [official naming conventions for Kontent.ai GitHub](./Naming-conventions.md#tagging).

Here are some recommendations on naming conventions for the SDK project name, the namespaces in your project, and package names. Following these recommendations will help others find the SDK more easily.

### Namespaces

* Main namespace: Kontent.ai
* Sub namespace: Name of the API (e.g., Delivery)
* Namespace separator: as per target platform
* Letter case: as per target platform

### Project name (GitHub repo name)

We recommend that the project for your SDK uses the following naming pattern: `[Main namespace] <Sub Namespace> <Platform> [Paradigm] SDK`. For example, for the Delivery .NET SDK, the project name would be _Kontent.ai Delivery .NET SDK_.

## OS project requirements

For your SDK to be included within the list Kontent.ai SDKs, the SDK project has to comply with our checklist for [Publishing an OS project](https://github.com/kontent-ai/.github/wiki/Checklist-for-publishing-a-new-OS-project). You can also read [Guidelines for building tools around Kontent.ai APIs](https://github.com/kontent-ai/.github/wiki/Guidelines-for-Kontent-related-tools) that should help you meet the requirements for creating a successful tool.
