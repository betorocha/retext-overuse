# retext-overuse [![Build Status](https://img.shields.io/travis/dunckr/retext-overuse.svg)](https://travis-ci.org/dunckr/retext-overuse) [![Coverage Status](https://img.shields.io/codecov/c/github/dunckr/retext-overuse.svg)](https://codecov.io/github/dunckr/retext-overuse)

Keyword extraction with [**retext**](https://github.com/dunckr/retext).

## Installation

[npm](https://docs.npmjs.com/cli/install):

```bash
npm install retext-overuse
```

**retext-overuse** is also available for [bower](http://bower.io/#install-packages),
[component](https://github.com/componentjs/component), and
[duo](http://duojs.org/#getting-started), and as an AMD, CommonJS, and globals
module, [uncompressed](retext-overuse.js) and
[compressed](retext-overuse.min.js).

## Usage

```javascript
var retext = require('retext');
var nlcstToString = require('nlcst-to-string');
var keywords = require('retext-overuse');

retext().use(keywords).process(
    /* First three paragraphs on Term Extraction from Wikipedia:
     * http://en.wikipedia.org/wiki/Terminology_extraction */
    'Terminology mining, term extraction, term recognition, or ' +
    'glossary extraction, is a subtask of information extraction. ' +
    'The goal of terminology extraction is to automatically extract ' +
    'relevant terms from a given corpus.' +
    '\n\n' +
    'In the semantic web era, a growing number of communities and ' +
    'networked enterprises started to access and interoperate through ' +
    'the internet. Modeling these communities and their information ' +
    'needs is important for several web applications, like ' +
    'topic-driven web crawlers, web services, recommender systems, ' +
    'etc. The development of terminology extraction is essential to ' +
    'the language industry.' +
    '\n\n' +
    'One of the first steps to model the knowledge domain of a ' +
    'virtual community is to collect a vocabulary of domain-relevant ' +
    'terms, constituting the linguistic surface manifestation of ' +
    'domain concepts. Several methods to automatically extract ' +
    'technical terms from domain-specific document warehouses have ' +
    'been described in the literature.' +
    '\n\n' +
    'Typically, approaches to automatic term extraction make use of ' +
    'linguistic processors (part of speech tagging, phrase chunking) ' +
    'to extract terminological candidates, i.e. syntactically ' +
    'plausible terminological noun phrases, NPs (e.g. compounds ' +
    '"credit card", adjective-NPs "local tourist information office", ' +
    'and prepositional-NPs "board of directors" - in English, the ' +
    'first two constructs are the most frequent). Terminological ' +
    'entries are then filtered from the candidate list using ' +
    'statistical and machine learning methods. Once filtered, ' +
    'because of their low ambiguity and high specificity, these terms ' +
    'are particularly useful for conceptualizing a knowledge domain ' +
    'or for supporting the creation of a domain ontology. Furthermore, ' +
    'terminology extraction is a very useful starting point for ' +
    'semantic similarity, knowledge management, human translation ' +
    'and machine translation, etc.',
    function (err, file) {
        var space = file.namespace('retext');

        console.log('Keywords:');

        space.keywords.forEach(function (keyword) {
            console.log(nlcstToString(keyword.matches[0].node));
        });

        console.log();
        console.log('Key-phrases:');

        space.keyphrases.forEach(function (phrase) {
            console.log(phrase.matches[0].nodes.map(nlcstToString).join(''));
        });
    }
);
```

Yields:

```text
Keywords:
term
extraction
Terminology
web
domain

Key-phrases:
terminology extraction
terms
term extraction
knowledge domain
communities
```

## API

### [retext](https://github.com/dunckr/retext#api)\.[use](https://github.com/dunckr/retext#retextuseplugin-options)([keywords](#api)\[, options\])

Extract keywords and key-phrases from the document.

**Parameters**:

*   `keywords` — This module;

*   `options` (`Object`, optional):

    *   `maximum` (default: 5) — Try to detect `words` and `phrases` words;

    Note that actual counts may differ. For example, when two words
    have the same score, both will be returned. Or when too few words
    exist, less will be returned. the same goes for phrases.

The results are stored in the `retext` namespace on the virtual file:
keywords at `file.namespace('retext').keywords` and key-phrases at
`file.namespace('retext').keyphrases`. Both are lists.

A single keyword looks as follows:

```js
{
    'stem': 'term',
    'score': 1,
    'matches': [
        { 'node': Node, 'index': 5, 'parent': Node },
        // ...
    ],
    // ...
}
```

...and a key-phrase:

```js
{
    'score': 1,
    'weight': 11,
    'stems': [ 'terminolog', 'extract' ],
    'value': 'terminolog extract',
    'matches':  [
        { 'nodes': [Node, Node, Node], 'parent': Node },
        // ...
    ]
}
```

## License

[MIT](LICENSE) © [Duncan Beaton](http://dunckr.com)
