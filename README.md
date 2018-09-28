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

## Simple Scalars

This package comes with a bunch of scalars that are ready-to-use and just work out of the box.

### Email

The Email scalar validates that a given value is a RFC 5321 compliant email.

## Advanced Scalars

### The Regex Scalar

The `Regex` class allows you to define a custom scalar that validates that the given
value matches a regular expression.

The quickest way to define a custom scalar is the `make` factory method. Just provide
a name and a regular expression and you will receive a ready-to-use custom regex scalar.

```php
<?php

use MLL\GraphQLScalars\Regex;

$hexValue = Regex::make(
    'HexValue',
    'A hexadecimal color is specified with: #RRGGBB, where RR (red), GG (green) and BB (blue) are hexadecimal integers between 00 and FF specifying the intensity of the color.',
    '/^#?([a-f0-9]{6}|[a-f0-9]{3})$/'
);
```

You may also define your regex scalar as a class.

```php
<?php

use MLL\GraphQLScalars\Regex;

// The name is implicitly set through the class name here
class HexValue extends Regex
{
    public $description = 'A hexadecimal color is specified with: #RRGGBB, where RR (red), GG (green) and BB (blue) are hexadecimal integers between 00 and FF specifying the intensity of the color.';
    
    protected function regex() : string
    {
        return '/^#?([a-f0-9]{6}|[a-f0-9]{3})$/';
    }
}
```

### The StringBase Scalar

The `StringScalar` encapsulate all the boilerplate associated with creating a string-based Scalar type.
It does the proper string checking for you and let's you focus on the minimal logic that is specific to your use case.

All you have to specify is a function that checks if the given string is valid. Use the factory method
to generate an instance on the fly.

```php
<?php

use MLL\GraphQLScalars\StringScalar;

$coolName = StringScalar::make(
    'CoolName',
    'A name that is most definitely cool.',
    function(string $name): bool {
        return in_array($name, [
           'Vladar',
           'Benedikt',
           'Christopher',
        ]);
    }
);
```

Or you may simply extend the class, check out the implementation of the [Email](src/Email.php) scalar to see how.
