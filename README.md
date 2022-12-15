# CORS for PHP (using the PSR-7 HTTP message interfaces)

[![Unit Tests](https://github.com/elephox-dev/php-cors-psr-7/actions/workflows/run-tests.yml/badge.svg)](https://github.com/fruitcake/php-cors/actions)
[![PHPStan Level 9](https://img.shields.io/badge/PHPStan-Level%209-blue)](https://github.com/elephox-dev/php-cors-psr-7/actions)
[![Code Coverage](https://img.shields.io/badge/CodeCoverage-100%25-brightgreen)](https://github.com/elephox-dev/php-cors-psr-7/actions/workflows/run-coverage.yml)
[![Packagist License](https://poser.pugx.org/fruitcake/php-cors/license.png)](http://choosealicense.com/licenses/mit/)
[![Latest Stable Version](https://poser.pugx.org/fruitcake/php-cors/version.png)](https://packagist.org/packages/fruitcake/php-cors)
[![Total Downloads](https://poser.pugx.org/fruitcake/php-cors/d/total.png)](https://packagist.org/packages/fruitcake/php-cors)
[![Fruitcake](https://img.shields.io/badge/Powered%20By-Fruitcake-b2bc35.svg)](https://fruitcake.nl/)

Library and middleware enabling cross-origin resource sharing.
It attempts to implement the [W3C Recommendation] for cross-origin resource sharing.

[W3C Recommendation]: http://www.w3.org/TR/cors/

> Note: This is a standalone ~~fork of https://github.com/asm89/stack-cors~~ fork of https://github.com/fruitcake/php-cors and is ~~compatible~~ almost compatible with the options for CorsService.  
> You need to pass an instance of a PSR-17 HTTP message factory as the first argument. It is used to generate the CORS responses.

## Installation

Require `fruitcake/php-cors` using composer, add this repository as a git source and choose the `dev-feature/psr-7-rewrite` version.

## Usage

This package can be used as a library. ~~You can use it in your framework using:~~

 - ~~[Stack middleware](http://stackphp.com/): https://github.com/asm89/stack-cors~~
 - ~~[Laravel](https://laravel.com): https://github.com/fruitcake/laravel-cors~~
 

### Options

| Option                 | Description                                                | Default value |
|------------------------|------------------------------------------------------------|---------------|
| allowedMethods         | Matches the request method.                                | `[]`          |
| allowedOrigins         | Matches the request origin.                                | `[]`          |
| allowedOriginsPatterns | Matches the request origin with `preg_match`.              | `[]`          |
| allowedHeaders         | Sets the Access-Control-Allow-Headers response header.     | `[]`          |
| exposedHeaders         | Sets the Access-Control-Expose-Headers response header.    | `[]`          |
| maxAge                 | Sets the Access-Control-Max-Age response header.           | `0`           |
| supportsCredentials    | Sets the Access-Control-Allow-Credentials header.          | `false`       |

The _allowedMethods_ and _allowedHeaders_ options are case-insensitive.

You don't need to provide both _allowedOrigins_ and _allowedOriginsPatterns_. If one of the strings passed matches, it is considered a valid origin. A wildcard in allowedOrigins will be converted to a pattern.

If `['*']` is provided to _allowedMethods_, _allowedOrigins_ or _allowedHeaders_ all methods / origins / headers are allowed.

> Note: Allowing a single static origin will improve cacheability.

### Example: using the library

```php
<?php

use Fruitcake\Cors\CorsService;

$cors = new CorsService(
  new PSR17Factory(),
  [
    'allowedHeaders'         => ['x-allowed-header', 'x-other-allowed-header'],
    'allowedMethods'         => ['DELETE', 'GET', 'POST', 'PUT'],
    'allowedOrigins'         => ['http://localhost', 'https://*.example.com'],
    'allowedOriginsPatterns' => ['/localhost:\d/'],
    'exposedHeaders'         => ['Content-Encoding'],
    'maxAge'                 => 0,
    'supportsCredentials'    => false,
  ]
);

$cors->addActualRequestHeaders(Response $response, $origin);
$cors->handlePreflightRequest(Request $request);
$cors->isActualRequestAllowed(Request $request);
$cors->isCorsRequest(Request $request);
$cors->isPreflightRequest(Request $request);
```

## License

Released under the MIT License, see [LICENSE](LICENSE).

> This package is split-off from https://github.com/asm89/stack-cors and developed as stand-alone library since 2022
