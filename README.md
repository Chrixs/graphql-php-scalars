# graphql-php-scalars

A collection of custom scalar types for usage with https://github.com/webonyx/graphql-php

[![Build Status](https://travis-ci.org/mll-lab/graphql-php-scalars.svg?branch=master)](https://travis-ci.org/mll-lab/graphql-php-scalars)
[![codecov](https://codecov.io/gh/mll-lab/graphql-php-scalars/branch/master/graph/badge.svg)](https://codecov.io/gh/mll-lab/graphql-php-scalars)
[![StyleCI](https://github.styleci.io/repos/150426104/shield?branch=master)](https://github.styleci.io/repos/150426104)
[![GitHub license](https://img.shields.io/github/license/mll-lab/graphql-php-scalars.svg)](https://github.com/mll-lab/graphql-php-scalars/blob/master/LICENSE)
[![Packagist](https://img.shields.io/packagist/v/mll-lab/graphql-php-scalars.svg)](https://packagist.org/packages/mll-lab/graphql-php-scalars)
[![Packagist](https://img.shields.io/packagist/dt/mll-lab/graphql-php-scalars.svg)](https://packagist.org/packages/mll-lab/graphql-php-scalars)

## Installation

    composer require mll-lab/graphql-php-scalars

## Usage

You can use the provided Scalars just like any other type in your schema definition.
Check [SchemaUsageTest](tests/SchemaUsageTest.php) for an example. 

### The Regex Scalar

The `Regex` class allows you to define a custom scalar that validates that the given
value matches a regular expression.

The quickest way to define a custom scalar is the `make` factory method. Just provide
a name and a regular expression and you will receive a ready-to-use custom regex scalar.

```php
<?php

use MLL\GraphQLScalars\Regex;

$hexValue = Regex::make('HexValue', '/^#?([a-f0-9]{6}|[a-f0-9]{3})$/');

$hexValue instanceof \GraphQL\Type\Definition\ScalarType; // true
```

You may also define your regex scalar as a class.

```php
<?php

use MLL\GraphQLScalars\Regex;

// The name is implicitly set through the class name here
class HexValue extends Regex
{
    protected function regex() : string
    {
        return '/^#?([a-f0-9]{6}|[a-f0-9]{3})$/';
    }
}
```
