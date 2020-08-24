
## Introduction

Sales API provides a set of tools that can be used in several projects to humanize the selling process.

We believe that vending should be effortless.

## Authorization

All API requests require the use of a generated API key. Please contact Omnilogic to get your API key.

To authenticate an API request, you should provide your API key in the `Authorization` header.

## API Endpoints

Here you can find information about each available endpoint:

* [/process-user-input]
* [/get-suggestion]
* [/get-similar-offers]
* [/get-non-semantic-suggestion]
* [/get-product-metadata]
* [/get-attributes-by-relevance]
* [/get-cart-url]
* [/shorten-url]
* [/save-log]


[//]: # (These are reference links used in the body of this file.)

[/process-user-input]: <docs/process_user_input.md>
[/get-suggestion]: <docs/get_suggestion.md>
[/get-similar-offers]: <docs/get_similar_offers.md>
[/get-non-semantic-suggestion]: <docs/get_non_semantic_suggestion.md>
[/get-product-metadata]: <docs/get_product_metadata.md>
[/get-attributes-by-relevance]: <docs/get_attributes_by_relevance.md>
[/get-cart-url]: <docs/get_cart_url.md>
[/shorten-url]: <docs/shorten_url.md>
[/save-log]: <docs/save_log.md>

# Changelog

All notable changes to this project will be documented in this session.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [x.x.x] - Roadmap
- refactoring for sales bot optimization

## [1.1.0] - Unreleased

### Work in progress
- include price filter for /process-user-input.

### Added
- included spell checking for /process-user-input.
- available sku count for /process-user-input response.
- metadata filters with sku counts for /process-user-input response.
- user_email and user_phone collection in /process-user-input.

### Changed

### Fixed

## [1.0.0] - Released - 2020-08-14

### Added

- process-user-input service included.
- get-suggestion service included.
- get-similar-offer service included.
- get-non-semantic-suggestion included.
- get-product-metadata included.
- get-attributes-by-relevance included.
- get-cart-url included.
- shorten-url included.
- save-log included.
- Shopfácil and Via Vini stores integrated.