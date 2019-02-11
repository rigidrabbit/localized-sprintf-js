# localized-sprintf-js

**localized-sprintf-js** is a customized version of [**sprintf-js**](https://github.com/alexei/sprintf.js) library which can be used for localize your numbers, prices and date representations.

**Note: you might need some polyfills for older environments. See [Support](#support) section below.**

## How to get your numbers localized

`%n` specifier can be used for formatting localized numbers like below:

    var localized = require('localized-sprintf-js').localized({
        locale:'en-UK', currency:'GBP'
    })
    var sprintf = localized.sprintf
    sprintf('%n', 1234567.89) // "1,234,567.89"

Also, `%m` specifier allows you to format localized numbers with currency symbols like below:

    sprintf('%m', 1234567.89) // "£1,234,567.89"

You can change your locale and/or currency even after instantiating `localized` object by calling `localized.locale()` and/or `localized.currency()` method. 

    // change the locale to Spanish.
    localized.locale('es')
    sprintf('%n', 1234567.89) // "1.234.567,89"

    // change the locale to Arabic.
    localized.locale('ar')
    sprintf('%n', 1234567.89) // "١٬٢٣٤٬٥٦٧٫٨٩"

    // change the locale and the currency to French and Euro.
    localized.locale('fr')
    localized.currency('EUR')
    sprintf('%m', 1234567.89) // "1 234 567,89 €"

#### IMPORTANT NOTICE #####

You should not let the `sprintf` function round your numbers or your prices.

Though `sprintf` function will round your numbers if needed because of the precision you passed with `%n` or `%m` specifier, the results might be unexpectable and therefore can be dangerous.

Thus, if you need your numbers rounded, then you do round your numbers by yourself before passing it into the function. Do not let the library function do that. 

## How to get your date localized

In order to have your date time representations localized, you need to import Moment.js as well as localized-sprintf-js library.

Once you get moment, then you can use `%l` and/or '%L' specifiers for formatting localized numbers like below:

    var moment = require('moment')
    var localized = require('localized-sprintf-js').localized({
        locale: 'en-US', moment: moment
    })
    var sprintf = localized.sprintf
    sprintf('%l', '2019-12-31 21:34:56.789')   // '9:34 PM',                            
    sprintf('%.0l', '2019-12-31 21:34:56.789') // '9:34 PM',                            
    sprintf('%.1l', '2019-12-31 21:34:56.789') // '12/31/2019',                         
    sprintf('%.2l', '2019-12-31 21:34:56.789') // 'Dec 31, 2019',                       
    sprintf('%.3l', '2019-12-31 21:34:56.789') // 'Dec 31, 2019 9:34 PM',               
    sprintf('%.4l', '2019-12-31 21:34:56.789') // 'Tue, Dec 31, 2019 9:34 PM',          
    sprintf('%L', '2019-12-31 21:34:56.789')   // '9:34:56 PM',                         
    sprintf('%.0L', '2019-12-31 21:34:56.789') // '9:34:56 PM',                         
    sprintf('%.1L', '2019-12-31 21:34:56.789') // '12/31/2019',                         
    sprintf('%.2L', '2019-12-31 21:34:56.789') // 'December 31, 2019',                  
    sprintf('%.3L', '2019-12-31 21:34:56.789') // 'December 31, 2019 9:34 PM',          
    sprintf('%.4L', '2019-12-31 21:34:56.789') // 'Tuesday, December 31, 2019 9:34 PM', 

A value for `%l` or `%L` specifiers can be either a Date object or a datetime number value or a string that can be parsed by moment(). 

* the mapping rules between specifier with precision and moment format are the following: 
    * `%l` — moment().format('LT')
    * `%L` — moment().format('LTS')
    * `%.1l` — moment().format('l')
    * `%.2l` — moment().format('ll')
    * `%.3l` — moment().format('lll')
    * `%.4l` — moment().format('llll')
    * `%.1L` — moment().format('L')
    * `%.2L` — moment().format('LL')
    * `%.3L` — moment().format('LLL')
    * `%.4L` — moment().format('LLLL')

## Installation

### NPM

    npm install localized-sprintf-js

## API

### `sprintf`

Returns a formatted string:

    string sprintf(string format, mixed arg1?, mixed arg2?, ...)

### `vsprintf`

Same as `sprintf` except it takes an array of arguments, rather than a variable number of arguments:

    string vsprintf(string format, array arguments?)

## Format specification

The placeholders in the format string are marked by `%` and are followed by one or more of these elements, in this order:

* An optional number followed by a `$` sign that selects which argument index to use for the value. If not specified, arguments will be placed in the same order as the placeholders in the input string.
* An optional `+` sign that forces to preceed the result with a plus or minus sign on numeric values. By default, only the `-` sign is used on negative numbers.
* An optional padding specifier that says what character to use for padding (if specified). Possible values are `0` or any other character precedeed by a `'` (single quote). The default is to pad with *spaces*.
* An optional `-` sign, that causes `sprintf` to left-align the result of this placeholder. The default is to right-align the result.
* An optional number, that says how many characters the result should have. If the value to be returned is shorter than this number, the result will be padded. When used with the `j` (JSON) type specifier, the padding length specifies the tab size used for indentation.
* An optional precision modifier, consisting of a `.` (dot) followed by a number, that says how many digits should be displayed for floating point numbers. When used with the `g` type specifier, it specifies the number of significant digits. When used on a string, it causes the result to be truncated.
* A type specifier that can be any of:
    * `%` — yields a literal `%` character
    * `b` — yields an integer as a binary number
    * `c` — yields an integer as the character with that ASCII value
    * `d` or `i` — yields an integer as a signed decimal number
    * `e` — yields a float using scientific notation
    * `u` — yields an integer as an unsigned decimal number
    * `f` — yields a float as is; see notes on precision above
    * `g` — yields a float as is; see notes on precision above
    * `o` — yields an integer as an octal number
    * `s` — yields a string as is
    * `t` — yields `true` or `false`
    * `T` — yields the type of the argument<sup><a href="#fn-1" name="fn-ref-1">1</a></sup>
    * `v` — yields the primitive value of the specified argument
    * `x` — yields an integer as a hexadecimal number (lower-case)
    * `X` — yields an integer as a hexadecimal number (upper-case)
    * `j` — yields a JavaScript object or array as a JSON encoded string
    * `l` — yields a Date oject as a localized date string printed by moment.js with LT, l, ll, lll, llll format
    * `L` — yields a Date oject as a localized date string printed by moment.js with LTS, L, LL, LLL, LLLL format

## Features

### Argument swapping

You can also swap the arguments. That is, the order of the placeholders doesn't have to match the order of the arguments. You can do that by simply indicating in the format string which arguments the placeholders refer to:

    sprintf('%2$s %3$s a %1$s', 'cracker', 'Polly', 'wants')

And, of course, you can repeat the placeholders without having to increase the number of arguments.

### Named arguments

Format strings may contain replacement fields rather than positional placeholders. Instead of referring to a certain argument, you can now refer to a certain key within an object. Replacement fields are surrounded by rounded parentheses - `(` and `)` - and begin with a keyword that refers to a key:

    var user = {
        name: 'Dolly',
    }
    sprintf('Hello %(name)s', user) // Hello Dolly

Keywords in replacement fields can be optionally followed by any number of keywords or indexes:

    var users = [
        {name: 'Dolly'},
        {name: 'Molly'},
        {name: 'Polly'},
    ]
    sprintf('Hello %(users[0].name)s, %(users[1].name)s and %(users[2].name)s', {users: users}) // Hello Dolly, Molly and Polly

Note: mixing positional and named placeholders is not (yet) supported

### Computed values

You can pass in a function as a dynamic value and it will be invoked (with no arguments) in order to compute the value on the fly.

    sprintf('Current date and time: %s', function() { return new Date().toString() })

### AngularJS

You can use `sprintf` and `vsprintf` (also aliased as `fmt` and `vfmt` respectively) in your AngularJS projects. See `demo/`.

## Support

### Node.js

`localized-sprintf-js` runs in all active Node versions (4.x+).

#### IMPORTANT NOTICE #####

Unfortunately the intl implementation of node.js is not perfect, thus you should be aware that the results will be very different from the ones you get in major browsers.

### Browser

Though `localized-sprintf-js` should work in all modern browsers, you might need polyfills for the following:

 - `String.prototype.repeat()` (any IE)
 - `Array.isArray()` (IE < 9)
 - `Object.create()` (IE < 9)

YMMV

## License

**localized-sprintf-js** is licensed under the terms of the 3-clause BSD license.

## Notes

<small><sup><a href="#fn-ref-1" name="fn-1">1</a></sup> `sprintf` doesn't use the `typeof` operator. As such, the value `null` is a `null`, an array is an `array` (not an `object`), a date value is a `date` etc.</small>
