# Algolia Search API Client for PHP

[Algolia Search](https://www.algolia.com) is a hosted search engine capable of delivering realtime results from the first keystroke.

The **Algolia Search API Client for PHP** lets
you easily use the [Algolia Search REST API](https://www.algolia.com/doc/rest-api/search) from
your PHP code.

[![Build Status](https://travis-ci.org/algolia/algoliasearch-client-php.svg?branch=master)](https://travis-ci.org/algolia/algoliasearch-client-php) [![Latest Stable Version](https://poser.pugx.org/algolia/algoliasearch-client-php/v/stable.svg)](https://packagist.org/packages/algolia/algoliasearch-client-php) [![Coverage Status](https://coveralls.io/repos/algolia/algoliasearch-client-php/badge.svg)](https://coveralls.io/r/algolia/algoliasearch-client-php)


If you're a Symfony or Laravel user, you're probably looking for the following integrations:

- **Laravel**: [Laravel Scout](https://www.algolia.com/doc/framework-integration/laravel/getting-started/introduction-to-scout-extended/)
- **Symfony**: [algolia/AlgoliaSearchBundle](https://github.com/algolia/AlgoliaSearchBundle)




## API Documentation

You can find the full reference on [Algolia's website](https://www.algolia.com/doc/api-client/php/).



1. **[Supported platforms](#supported-platforms)**


1. **[Install](#install)**


1. **[Quick Start](#quick-start)**


1. **[Push data](#push-data)**


1. **[Configure](#configure)**


1. **[Search](#search)**


1. **[Search UI](#search-ui)**


1. **[List of available methods](#list-of-available-methods)**


# Getting Started



## Supported platforms

The API client is compatible with PHP version 5.3 and later.

## Install

### With Composer (recommended)

Install the package via [Composer](https://getcomposer.org/doc/00-intro.md):

```bash
composer require algolia/algoliasearch-client-php
```

### Without Composer

1. Download the client from GitHub and unzip it in your project
2. Inside the client root folder, execute `./bin/install-dependencies-without-composer`
3. In your code, require the `autoload.php` in the client root folder

### Framework integrations

We officially provide support for the **Laravel** and **Symfony** frameworks.
If you're using one of these two frameworks, have a look at our [Laravel documentation](https://www.algolia.com/doc/framework-integration/laravel/getting-started/introduction-to-scout-extended/) or [Symfony documentation](https://www.algolia.com/doc/framework-integration/symfony/getting-started/).

## Quick Start

In 30 seconds, this quick start tutorial will show you how to index and search objects.

### Initialize the client

To start, you need to initialize the client. To do this, you need your **Application ID** and **API Key**.
You can find both on [your Algolia account](https://www.algolia.com/api-keys).

```php
// composer autoload
require __DIR__ . '/vendor/autoload.php';

// if you are not using composer
// require_once 'path/to/algolia/folder/autoload.php';

$client = Algolia\AlgoliaSearch\SearchClient::create(
  'YourApplicationID',
  'YourAdminAPIKey'
);

$index = $client->initIndex('your_index_name');
```

## Push data

Without any prior configuration, you can start indexing [500 contacts](https://github.com/algolia/datasets/blob/master/contacts/contacts.json) in the `contacts` index using the following code:

```php
$client = \Algolia\AlgoliaSearch\SearchClient::create('YourApplicationID', 'YourAPIKey');
$index = $client->initIndex('contacts');
$batch = json_decode(file_get_contents('contacts.json'), true);
$index->saveObjects($batch, ['autoGenerateObjectIDIfNotExist' => true]);
```

## Configure

You can customize settings to fine tune the search behavior. For example, you can add a custom ranking by number of followers to further enhance the built-in relevance:

```php
$index->setSettings(['customRanking' => ['desc(followers)']]);
```

You can also configure the list of attributes you want to index by order of importance (most important first).

**Note:** Algolia is designed to suggest results as you type, which means you'll generally search by prefix.
In this case, the order of attributes is crucial to decide which hit is the best.

```php
$index->setSettings(
  [
    'searchableAttributes' => [
      'lastname',
      'firstname',
      'company',
      'email',
      'city',
      'address'
    ]
  ]
);
```

## Search

You can now search for contacts by `firstname`, `lastname`, `company`, etc. (even with typos):

```php
// Search for a first name
var_dump($index->search('jimmie'));

// Search for a first name with typo
var_dump($index->search('jimie'));

// Search for a company
var_dump($index->search('california paint'));

// Search for a first name and a company
var_dump($index->search('jimmie paint'));
```

## Search UI

**Warning:** If you're building a web application, you may be interested in using one of our
[front-end search UI libraries](https://www.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/js/).

The following example shows how to quickly build a front-end search using
[InstantSearch.js](https://www.algolia.com/doc/guides/building-search-ui/what-is-instantsearch/js/)

### index.html

```html
<!doctype html>
<head>
  <meta charset="UTF-8">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/instantsearch.css@7.1.0/themes/algolia.css" />
</head>
<body>
  <header>
    <div>
       <input id="search-input" placeholder="Search for products">
       <!-- We use a specific placeholder in the input to guide users in their search. -->
    
  </header>
  <main>
      
      
  </main>

  <script type="text/html" id="hit-template">
    
      <p class="hit-name">
        {}{ "attribute": "firstname" }{{/helpers.highlight}}
        {}{ "attribute": "lastname" }{{/helpers.highlight}}
      </p>
    
  </script>

  <script src="https://cdn.jsdelivr.net/npm/instantsearch.js@3.0.0"></script>
  <script src="app.js"></script>
</body>
```

### app.js

```js
// Replace with your own values
var searchClient = algoliasearch(
  'YourApplicationID',
  'YourAPIKey' // search only API key, no ADMIN key
);

var search = instantsearch({
  indexName: 'instant_search',
  searchClient: searchClient,
  routing: true,
  searchParameters: {
    hitsPerPage: 10
  }
});

search.addWidget(
  instantsearch.widgets.searchBox({
    container: '#search-input'
  })
);

search.addWidget(
  instantsearch.widgets.hits({
    container: '#hits',
    templates: {
      item: document.getElementById('hit-template').innerHTML,
      empty: "We didn't find any results for the search <em>\"{{query}}\"</em>"
    }
  })
);

search.start();
```




## List of available methods





### Personalization

- [Add strategy](https://algolia.com/doc/api-reference/api-methods/add-strategy/?language=php)
- [Get strategy](https://algolia.com/doc/api-reference/api-methods/get-strategy/?language=php)




### Search

- [Search index](https://algolia.com/doc/api-reference/api-methods/search/?language=php)
- [Search for facet values](https://algolia.com/doc/api-reference/api-methods/search-for-facet-values/?language=php)
- [Search multiple indices](https://algolia.com/doc/api-reference/api-methods/multiple-queries/?language=php)
- [Browse index](https://algolia.com/doc/api-reference/api-methods/browse/?language=php)




### Indexing

- [Add objects](https://algolia.com/doc/api-reference/api-methods/add-objects/?language=php)
- [Save objects](https://algolia.com/doc/api-reference/api-methods/save-objects/?language=php)
- [Partial update objects](https://algolia.com/doc/api-reference/api-methods/partial-update-objects/?language=php)
- [Delete objects](https://algolia.com/doc/api-reference/api-methods/delete-objects/?language=php)
- [Replace all objects](https://algolia.com/doc/api-reference/api-methods/replace-all-objects/?language=php)
- [Delete by](https://algolia.com/doc/api-reference/api-methods/delete-by/?language=php)
- [Clear objects](https://algolia.com/doc/api-reference/api-methods/clear-objects/?language=php)
- [Get objects](https://algolia.com/doc/api-reference/api-methods/get-objects/?language=php)
- [Custom batch](https://algolia.com/doc/api-reference/api-methods/batch/?language=php)




### Settings

- [Get settings](https://algolia.com/doc/api-reference/api-methods/get-settings/?language=php)
- [Set settings](https://algolia.com/doc/api-reference/api-methods/set-settings/?language=php)
- [Copy settings](https://algolia.com/doc/api-reference/api-methods/copy-settings/?language=php)




### Manage indices

- [List indices](https://algolia.com/doc/api-reference/api-methods/list-indices/?language=php)
- [Delete index](https://algolia.com/doc/api-reference/api-methods/delete-index/?language=php)
- [Copy index](https://algolia.com/doc/api-reference/api-methods/copy-index/?language=php)
- [Move index](https://algolia.com/doc/api-reference/api-methods/move-index/?language=php)




### API keys

- [Create secured API Key](https://algolia.com/doc/api-reference/api-methods/generate-secured-api-key/?language=php)
- [Add API Key](https://algolia.com/doc/api-reference/api-methods/add-api-key/?language=php)
- [Update API Key](https://algolia.com/doc/api-reference/api-methods/update-api-key/?language=php)
- [Delete API Key](https://algolia.com/doc/api-reference/api-methods/delete-api-key/?language=php)
- [Restore API Key](https://algolia.com/doc/api-reference/api-methods/restore-api-key/?language=php)
- [Get API Key permissions](https://algolia.com/doc/api-reference/api-methods/get-api-key/?language=php)
- [List API Keys](https://algolia.com/doc/api-reference/api-methods/list-api-keys/?language=php)




### Synonyms

- [Save synonym](https://algolia.com/doc/api-reference/api-methods/save-synonym/?language=php)
- [Batch synonyms](https://algolia.com/doc/api-reference/api-methods/batch-synonyms/?language=php)
- [Delete synonym](https://algolia.com/doc/api-reference/api-methods/delete-synonym/?language=php)
- [Clear all synonyms](https://algolia.com/doc/api-reference/api-methods/clear-synonyms/?language=php)
- [Get synonym](https://algolia.com/doc/api-reference/api-methods/get-synonym/?language=php)
- [Search synonyms](https://algolia.com/doc/api-reference/api-methods/search-synonyms/?language=php)
- [Replace all synonyms](https://algolia.com/doc/api-reference/api-methods/replace-all-synonyms/?language=php)
- [Copy synonyms](https://algolia.com/doc/api-reference/api-methods/copy-synonyms/?language=php)
- [Export Synonyms](https://algolia.com/doc/api-reference/api-methods/export-synonyms/?language=php)




### Query rules

- [Save rule](https://algolia.com/doc/api-reference/api-methods/save-rule/?language=php)
- [Batch rules](https://algolia.com/doc/api-reference/api-methods/batch-rules/?language=php)
- [Get rule](https://algolia.com/doc/api-reference/api-methods/get-rule/?language=php)
- [Delete rule](https://algolia.com/doc/api-reference/api-methods/delete-rule/?language=php)
- [Clear rules](https://algolia.com/doc/api-reference/api-methods/clear-rules/?language=php)
- [Search rules](https://algolia.com/doc/api-reference/api-methods/search-rules/?language=php)
- [Replace all rules](https://algolia.com/doc/api-reference/api-methods/replace-all-rules/?language=php)
- [Copy rules](https://algolia.com/doc/api-reference/api-methods/copy-rules/?language=php)
- [Export rules](https://algolia.com/doc/api-reference/api-methods/export-rules/?language=php)




### A/B Test

- [Add A/B test](https://algolia.com/doc/api-reference/api-methods/add-ab-test/?language=php)
- [Get A/B test](https://algolia.com/doc/api-reference/api-methods/get-ab-test/?language=php)
- [List A/B tests](https://algolia.com/doc/api-reference/api-methods/list-ab-tests/?language=php)
- [Stop A/B test](https://algolia.com/doc/api-reference/api-methods/stop-ab-test/?language=php)
- [Delete A/B test](https://algolia.com/doc/api-reference/api-methods/delete-ab-test/?language=php)




### Insights

- [Clicked Object IDs After Search](https://algolia.com/doc/api-reference/api-methods/clicked-object-ids-after-search/?language=php)
- [Clicked Object IDs](https://algolia.com/doc/api-reference/api-methods/clicked-object-ids/?language=php)
- [Clicked Filters](https://algolia.com/doc/api-reference/api-methods/clicked-filters/?language=php)
- [Converted Objects IDs After Search](https://algolia.com/doc/api-reference/api-methods/converted-object-ids-after-search/?language=php)
- [Converted Object IDs](https://algolia.com/doc/api-reference/api-methods/converted-object-ids/?language=php)
- [Converted Filters](https://algolia.com/doc/api-reference/api-methods/converted-filters/?language=php)
- [Viewed Object IDs](https://algolia.com/doc/api-reference/api-methods/viewed-object-ids/?language=php)
- [Viewed Filters](https://algolia.com/doc/api-reference/api-methods/viewed-filters/?language=php)




### MultiClusters

- [Assign or Move userID](https://algolia.com/doc/api-reference/api-methods/assign-user-id/?language=php)
- [Get top userID](https://algolia.com/doc/api-reference/api-methods/get-top-user-id/?language=php)
- [Get userID](https://algolia.com/doc/api-reference/api-methods/get-user-id/?language=php)
- [List clusters](https://algolia.com/doc/api-reference/api-methods/list-clusters/?language=php)
- [List userIDs](https://algolia.com/doc/api-reference/api-methods/list-user-id/?language=php)
- [Remove userID](https://algolia.com/doc/api-reference/api-methods/remove-user-id/?language=php)
- [Search userID](https://algolia.com/doc/api-reference/api-methods/search-user-id/?language=php)




### Advanced

- [Get logs](https://algolia.com/doc/api-reference/api-methods/get-logs/?language=php)
- [Configuring timeouts](https://algolia.com/doc/api-reference/api-methods/configuring-timeouts/?language=php)
- [Set extra header](https://algolia.com/doc/api-reference/api-methods/set-extra-header/?language=php)
- [Wait for operations](https://algolia.com/doc/api-reference/api-methods/wait-task/?language=php)





## Getting Help

- **Need help**? Ask a question to the [Algolia Community](https://discourse.algolia.com/) or on [Stack Overflow](http://stackoverflow.com/questions/tagged/algolia).
- **Found a bug?** You can open a [GitHub issue](https://github.com/algolia/algoliasearch-client-php/issues).

