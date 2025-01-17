# vue-jalali-moment for vue 3 
[![npm version](https://badge.fury.io/js/vue-jalali-moment.svg)](https://badge.fury.io/js/vue-moment) [![Build Status](https://travis-ci.org/brockpetrie/vue-moment.svg?branch=master)](https://travis-ci.org/fingerpich/vue-jalali-moment)

[jalali](https://github.com/fingerpich/jalali-moment)(khorshidi, shamsi) filters for your [Vue.js](http://vuejs.org/) project.

## Installation

Install via NPM...

```sh
$ npm install vue3-jalali-moment
```

...and require the plugin like so:

```js
import { createApp } from 'vue'
const app = createApp({
    ...
})

app.use(require('vue3-jalali-moment'));
```

## Usage

Simply set `moment` as the filtering function and you're good to go. At least one argument is expected, which the filter assumes to be a `format` string if the argument doesn't match any of the other filtering methods.

```html
<span>{{ $filter.moment(someDate, "dddd, MMMM Do YYYY") }}</span>
<!-- or create a new date from 'now' -->
<span>{{ $filter.moment(new Date(), "dddd, MMMM Do YYYY") }}</span>

```

## Passing Your Date (*** Not Available on this Update ***)

Moment.js expects your input to be either: a valid ISO 8601 formatted string (see <http://momentjs.com/docs/#/parsing/string/>), a valid `Date` object, a Unix timestamp (must be passed as a Number), or a date string with an accompanying format pattern (i.e. when you know the format of the date input). For the latter, `vue-moment` allows you to pass your date and format pattern(s) as an array, like such:

```html
<span>{{ [ someDate, "MM.DD.YY" ] | moment("jdddd, jMMMM Do jYYYY") }}</span>
<span>{{ [ jalaliDateString, "jYY-jMM-jDD" ] | moment("dddd, MMMM Do YYYY") }}</span>
<!-- or when you want to parse against more than one pattern -->
<span>{{ [ someDate, ["MM.DD.YY", "MM-DD-YY", "MM-DD-YYYY"] ] | moment("dddd, MMMM Do YYYY") }}</span>
```

As of 3.0.0, passing an empty or invalid input will no longer initiate moment with a new `Date` object.

## Filtering Methods

### format (default)

This is the default filtering option. Formats the date against a string of tokens. See <http://momentjs.com/docs/#/displaying/format/> for a list of tokens and examples.

**Default**

```html
<span>{{ $filter.moment(someDate, "YYYY") }}</span>
<!-- e.g. "2021" -->
<span>{{ $filter.moment(someDate, "jYYYY") }}</span>
<!-- e.g. "1400" -->
<span>{{ $filter.moment(someDate, "ddd, hA") }}</span>
<!-- e.g. "Mon, 10AM" -->
<span>{{ $filter.moment(someDate, "dddd, MMMM Do YYYY, h:mm:ss a") }}</span>
<!-- e.g. "Monday, June 14th 2021, 10:23:48 am" -->
```

For more information about `moment#format`, check out <http://momentjs.com/docs/#/displaying/format/>.


### from

Display a moment in relative time, either from now or from a specified date.

**Default** (calculates from current time)

```html
<span>{{ $filter.moment(someDate, "from", "now") }}</span>
<!-- or shorthanded -->
<span>{{ $filter.moment(someDate, "from") }}</span>
```

**With a reference time given**

```html
<span>{{ $filter.moment(someDate, "from", "Jan. 11th, 1985") }}</span>
```

**With suffix hidden** (e.g. '4 days ago' -> '4 days')

```html
<span>{{ $filter.moment(someDate, "from", "now", true) }}</span>
<!-- or -->
<span>{{ $filter.moment(someDate, "from", true) }}</span>
<!-- or with a reference time -->
<span>{{ $filter.moment(someDate, "from", "Jan. 11th, 2000", true) }}</span>
```

For more information about `moment#fromNow` and `moment#from`, check out <http://momentjs.com/docs/#/displaying/fromnow/> and <http://momentjs.com/docs/#/displaying/from/>.


### calendar

Formats a date with different strings depending on how close to a certain date (today by default) the date is.

**Default** (calculates from current time)

```html
<span>{{ $filter.moment(someDate, "calendar") }}</span>
<!-- e.g. "Last Monday 2:30 AM" -->
```

**With a reference time given**

```html
<span>{{ $filter.moment(someDate, "calendar", "July 10 2021") }}</span>
<!-- e.g. "7/10/2021" -->
```

For more information about `moment#calendar`, check out <http://momentjs.com/docs/#/displaying/calendar-time/>.


### add

Mutates the original moment by adding time.

```html
<span>{{ $filter.moment(someDate, "add", "7 days") }}</span>
<!-- or with multiple keys -->
<span>{{ $filter.moment(someDate, "add", "1 year, 3 months, 30 weeks, 10 days") }}</span>
```

For more information about `moment#add`, check out <http://momentjs.com/docs/#/manipulating/add/>.


### subtract

Works the same as `add`, but mutates the original moment by subtracting time.

```html
<span>{{ $filter.moment(someDate, "subtract", "3 hours") }}</span>
```

For more information about `moment#subtract`, check out <http://momentjs.com/docs/#/manipulating/subtract/>.

### timezone

Convert the date to a certain timezone

```html
<span>{{ $filter.moment(Date, "timezone", "America/Los_Angeles", "LLLL ss")}}</span>
```

**To use this filter you will need to pass `moment-timezone` through to the plugin**

```js
// main.js
import { createApp } from 'vue'
import VueMoment from 'vue-moment'
import moment from 'moment-timezone'

const app = createApp({
    ...
})

app.use(VueMoment, {
    moment,
})
```

For more information about `moment#timezone`, check out <https://momentjs.com/timezone/docs/#/using-timezones/converting-to-zone/>.


## Chaining

There's some built-in (and not thoroughly tested) support for chaining, like so:

```html
<span>{{ $filter.moment(someDate, "add", "2 years, 8 days", "subtract", "3 hours", "ddd, hA") }}</span>
```

This would add 2 years and 8 days to the date, then subtract 3 hours, then format the resulting date.


## Configuration

`vue-moment` should respect any global Moment customizations, including i18n locales. For more info, check out <http://momentjs.com/docs/#/customization/>.

You can also pass a custom Moment object through with the plugin options. This technique is especially useful for overcoming the browserify locale bug demonstrated in the docs <http://momentjs.com/docs/#/use-it/browserify/>

```js
const moment = require('moment')
require('moment/locale/es')

Vue.use(require('vue-moment'), {
    moment
})

console.log(Vue.moment().locale()) //es
```
