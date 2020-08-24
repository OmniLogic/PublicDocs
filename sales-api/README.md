## Introduction

Sales API provides a set of tools that can be used in several projects to humanize the selling process.

We believe that vending should be effortless.

## Public Endpoint

This is public endpoint for Sales API services:

http://assistant.omnilogic.ai/sales/api/

## API Services

Here you can find information about each available endpoint:

### /process-user-input
Natural language understanding (NLU) for digital sales intents and entities with semantic offer search.

## Changelog

All notable changes to this project will be documented in this session.

### [1.1.0] - Unreleased

#### Work in progress
- process-user-input service:
   - price selection entity recognition and search filtered
   - most viewed entity recognition and search filtered
   - best seller entity recognition and search filtered
   - lower price entity recognition and search filtered
   - best promotions entity recognition and search filtered

### [1.1.0beta] - Released - 2020-08-19

#### Added
- process-user-input service:
   - spell checking
   - available sku count
   - metadata filters with sku counts

#### Changed
- process-user-input service:
   - page_size field request to search_size fied name

### [1.0.0] - Released - 2020-08-14

#### Added

- process-user-input service:
   - entity recoginition for product request by user
   - context process infos 
   - structured data from user request
   - metadata sorted by relevance 
   - metadata translation
   - semantic offers search  
   - non-semantic offers search

