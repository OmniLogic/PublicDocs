## Introduction

Sales API provides a set of tools that can be used in several projects to humanize the selling process.

We believe that vending should be effortless.

## API Services

Here you can find information about each available endpoint:

### process-user-input
Natural language understanding (NLU) for digital sales intents and entities with semantic offer search.

## Changelog

All notable changes to this project will be documented in this session.

## 1.1.0 - Released - 2020-08-26

### Added
- included spell checking for /process-user-input.
- available sku count for /process-user-input response.
- metadata filters with sku counts for /process-user-input response.
- user_email and user_phone collection in /process-user-input.
- included price range recognition for /process-user-input.
- included search behaviour (sort) recognition for /process-user-input.

### Fixed
- fixed a bug when using non-semantic-search with page_size = 1

## 1.1.0beta - Released - 2020-08-19

### Added
- process-user-input service:
   - spell checking
   - available sku count
   - metadata filters with sku counts

### Changed
- process-user-input service:
   - page_size field request to search_size field name

## 1.0.0 - Released - 2020-08-14

### Added

- process-user-input service:
   - entity recoginition for product request by user
   - context process infos 
   - structured data from user request
   - metadata sorted by relevance 
   - metadata translation
   - semantic offers search  
   - non-semantic offers search