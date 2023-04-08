# url-metadata

Request an http(s) url and scrape its metadata. Many of the metadata fields returned are [Open Graph Protocol (og:)](http://ogp.me/) so far.

Support also added for [JSON-LD](https://moz.com/blog/json-ld-for-beginners).

Under the hood, this package does some post-request processing on top of the javascript native `fetch` API.

To report a bug or request a feature please open an issue or pull request in [GitHub](https://github.com/laurengarcia/url-metadata).


## Usage
Works with Node.js version `>=18.0.0` or in the browser when bundled (with browserify or webpack for example).

Use previous version `2.5.0` which uses the (now-deprecated) `request` module instead if you don't have access to javascript-native `fetch` API in your target environment.

Install:
```
$ npm install url-metadata --save`
```

In your project file:
```javascript
const urlMetadata = require('url-metadata')

urlMetadata('https://www.npmjs.com/package/url-metadata')
.then((metadata) => {
  console.log(metadata)
  // do stuff with the metadata
},
(err) => {
  console.log(err)
})
```

To override the default options (see below), pass in a second argument:
```javascript
const urlMetadata = require('url-metadata')

urlMetadata('https://www.npmjs.com/package/url-metadata', {
  requestHeaders: {
    'User-Agent': 'foo',
    'From': 'bar@bar.com'
  }
}).then((metadata) => {
  console.log(metadata)
  // do stuff with the metadata
}).catch((err) => {
  console.log(err)
})
```

### Options & Defaults
This module's default options are the values below that you can override:
```javascript
{
  // custom request headers
  requestHeaders: {
    'User-Agent': 'url-metadata/3.0 (npm module)',
    'From': 'example@example.com',
  }

  // how `fetch` API handles caching
  cache: 'no-cache',

  // timeout in milliseconds, default is 10 seconds
  timeout: 10000,

  // number of characters to truncate description to
  descriptionLength: 750,

  // force image urls in selected tags to use https,
  // valid for 'image', 'og:image' and 'og:image:secure_url' tags
  ensureSecureImageRequest: true,

  // return raw response body as string
  includeResponseBody: false
}
```

### Returns
Returns a promise that gets resolved with the following metadata if the request response returns successfully. Note that the `url` field returned below will be the last hop in the request chain. So if you passed in a url that was generated by a link shortener, for example, you'll get back the final destination of the link as the `url`.
```javascript
{
    'url': '',
    'canonical': '',
    'title': '',
    'image': '',
    'author': '',
    'description': '',
    'keywords': '',
    'price': '',
    'priceCurrency': '',
    'availability': '',
    'robots': '',
    'og:url': '',
    'og:locale': '',
    'og:locale:alternate': '',
    'og:title': '',
    'og:type': '',
    'og:description': '',
    'og:determiner': '',
    'og:site_name': '',
    'og:image': '',
    'og:image:secure_url': '',
    'og:image:type': '',
    'og:image:width': '',
    'og:image:height': '',
    'twitter:title': '',
    'twitter:image': '',
    'twitter:image:alt': '',
    'twitter:card': '',
    'twitter:site': '',
    'twitter:site:id': '',
    'twitter:account_id': '',
    'twitter:creator': '',
    'twitter:creator:id': '',
    'twitter:player': '',
    'twitter:player:width': '',
    'twitter:player:height': '',
    'twitter:player:stream': '',
    'jsonld': {},
    'responseBody': ''
}
```

Additional fields are also returned if the url has an `og:type` set to `article`. These fields are:
```javascript
{
  'article:published_time'     : '',
  'article:modified_time'      : '',
  'article:expiration_time'    : '',
  'article:author'             : '',
  'article:section'            : '',
  'article:tag'                : '',
  'og:article:published_time'  : '',
  'og:article:modified_time'   : '',
  'og:article:expiration_time' : '',
  'og:article:author'          : '',
  'og:article:section'         : '',
  'og:article:tag'             : ''
}
```
