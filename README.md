# RX-HTTP-Request

<div style="float:right;">
    <a href="https://babeljs.io/docs/learn-es2015/">
        <img src="http://image.slidesharecdn.com/4-es6metbabel-150513100342-lva1-app6891/95/es6-with-babeljs-1-638.jpg"
             align="right" alt="ES2015 logo" width="50" height="50" />
    </a>
    <a href="https://github.com/Reactive-Extensions/RxJS">
        <img src="http://reactivex.io/assets/Rx_Logo_S.png"
             align="right" alt="ReactiveX logo" width="50" height="50"/>
    </a>
</div>

[![NPM](https://nodei.co/npm/rx-http-request.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/rx-http-request/)

[![Build Status](https://travis-ci.org/njl07/rx-http-request.svg?branch=master)](https://travis-ci.org/njl07/rx-http-request)
[![Coverage Status](https://coveralls.io/repos/github/njl07/rx-http-request/badge.svg?branch=master)](https://coveralls.io/github/njl07/rx-http-request?branch=master)
[![Dependencies](https://david-dm.org/njl07/rx-http-request.svg)](https://david-dm.org/njl07/rx-http-request)
[![DevDependencies](https://david-dm.org/njl07/rx-http-request/dev-status.svg)](https://david-dm.org/njl07/rx-http-request#info=devDependencies)

The world-famous HTTP client [Request](https://github.com/request/request) now [RxJS](https://github.com/Reactive-Extensions/RxJS) compliant and wrote in full [ES2015](https://babeljs.io/docs/learn-es2015/).

## Table of contents

* [Super simple to use](#super-simple-to-use)
* [API in Detail](#api-in-detail)
    * [.request](#request)
    * [.details(options)](#detailsoptions)
    * [.get(uri[,options])](#geturi-options)
        * [Crawl a webpage](#crawl-a-webpage)
        * [GET something from a JSON REST API](#get-something-from-a-json-rest-api)
    * [.post(uri[,options])](#posturi-options)
        * [POST data to a JSON REST API](#post-data-to-a-json-rest-api)
        * [POST like HTML forms do](#post-like-html-forms-do)
    * [.put(uri[,options])](#puturi-options)
    * [.patch(uri[,options])](#patchuri-options)
    * [.delete(uri[,options])](#deleteuri-options)
    * [.head(uri[,options])](#headuri-options)
* [Contributing](#contributing)
* [Change History](#change-history)
* [License](#license)

## Super simple to use

**RX-HTTP-Request** is designed to be the simplest way possible to make http calls.

It's fully `ES2015` wrotten so you can import it :

```javascript
import {RxHttpRequest} from 'rx-http-request';
```

or use `CommonJS`:

```javascript
const RxHttpRequest = require('rx-http-request').RxHttpRequest;
```

Now, it's easy to perform a `HTTP` request:

```javascript
RxHttpRequest.get('http://www.google.fr').subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the HTML for the Google homepage.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

## API in Detail

**RX-HTTP-Request** uses [Request](https://github.com/request/request) **API** to perform calls and returns [RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md).

All **options** to pass to **API** **methods** can be found [here](https://github.com/request/request#requestoptions-callback).

All **methods** to execute on **response objects** can be found [here](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#observable-instance-methods).

--------


### .request

Returns the original [Request](https://github.com/request/request#requestoptions-callback) **API** to perform calls without `RxJS.Observable` response but with a **callback method**.

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.request({uri: 'http://www.google.fr'}, (error, response, body) => {

    if (!error && response.statusCode == 200) {
        console.log(body); // Show the HTML for the Google homepage.
    }
});
```

### .details(options)

This method **returns a wrapper** around the normal **RX-HTTP-Request API**  that defaults to whatever options you pass to it.

**Note:** `RxHttpRequest.defaults()` **does not** modify the global API; instead, it returns a wrapper that has your default settings applied to it.

**Note:** You can call `.defaults()` on the wrapper that is returned from `RxHttpRequest.defaults()` to add/override defaults that were previously defaulted.

For example:
 
```javascript
// requests using baseRequest will set the 'x-token' header
const baseRequest = RxHttpRequest.defaults({
    headers: {'x-token': 'my-token'}
});

// requests using specialRequest will include the 'x-token' header set in
// baseRequest and will also include the 'special' header
const specialRequest = baseRequest.defaults({
    headers: {special: 'special value'}
});
```

### .get(uri[, options])

Performs a request with `get` http method.

#### Crawl a webpage

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.get('http://www.google.fr').subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the HTML for the Google homepage.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

#### GET something from a JSON REST API
     
```javascript
import {RxHttpRequest} from 'rx-http-request';

const options = {
    qs: {
        access_token: 'xxxxx xxxxx' // -> uri + '?access_token=xxxxx%20xxxxx'
    },
    headers: {
        'User-Agent': 'RX-HTTP-Request'
    },
    json: true // Automatically parses the JSON string in the response
};

RxHttpRequest.get('https://api.github.com/user/repos', options).subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the JSON response object.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

### .post(uri[, options])

Performs a request with `post` http method.

#### POST data to a JSON REST API

```javascript
import {RxHttpRequest} from 'rx-http-request';

const options = {
    body: {
        some: 'payload'
    },
    json: true // Automatically stringifies the body to JSON
};

RxHttpRequest.post('http://posttestserver.com/posts', options).subscribe(
    (data) => {

        if (data.response.statusCode === 201) {
            console.log(data.body); // Show the JSON response object.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

#### POST like HTML forms do

```javascript
import {RxHttpRequest} from 'rx-http-request';

const options = {
    form: {
        some: 'payload' // Will be urlencoded
    },
    headers: {
        /* 'content-type': 'application/x-www-form-urlencoded' */ // Set automatically
    }
};

RxHttpRequest.post('http://posttestserver.com/posts', options).subscribe(
    (data) => {

        if (data.response.statusCode === 201) {
            console.log(data.body); // POST succeeded...
        }
    },
    (err) => console.error(err) // Show error in console
);
```

### .put(uri[, options])

Performs a request with `put` http method.

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.put(uri).subscribe(...);
```

### .patch(uri[, options])

Performs a request with `patch` http method.

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.patch(uri).subscribe(...);
```

### .delete(uri[, options])

Performs a request with `delete` http method.

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.delete(uri).subscribe(...);
```

### .head(uri[, options])

Performs a request with `head` http method.

```javascript
import {RxHttpRequest} from 'rx-http-request';

RxHttpRequest.head(uri).subscribe(...);
```

## Contributing

To set up your development environment:

1. clone the repo to your workspace,
2. in the shell `cd` to the main folder,
3. hit `npm install`,
4. hit `npm install gulp -g` if you haven't installed gulp globally yet, and
5. run `gulp`. (Or run node ./node_modules/.bin/gulp if you don't want to install gulp globally.)

`gulp` watches all source files and if you save some changes it will lint the code and execute all tests. The test coverage report can be viewed from `./coverage/lcov-report/index.html`.

## Change History

* v1.0.0 (2016-03-27)
    * Carefully rewritten from scratch to make RX-HTTP-Request a drop-in replacement for Request

## License

Copyright (c) 2016 **Nicolas Jessel** Licensed under the [MIT license](https://github.com/njl07/rx-http-request/tree/master/LICENSE.md).