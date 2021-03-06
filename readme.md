

# pwmetrics

> Progressive web metrics at your fingertipz. 💅

CLI tool and lib to gather performance metrics via [Lighthouse](https://github.com/GoogleChrome/lighthouse/). _IN BETA_.

![image](https://cloud.githubusercontent.com/assets/39191/19417867/7aead922-93af-11e6-88ec-917dad6e89d2.png)

 Documentation on these metrics in the works. If you hit bugs in the metrics collection, report at [Lighthouse issues](https://github.com/GoogleChrome/lighthouse/issues).

### Install [![NPM pwmetrics package](https://img.shields.io/npm/v/pwmetrics.svg)](https://npmjs.org/package/pwmetrics)
```sh
$ npm install --global pwmetrics
# or
$ npm install --save pwmetrics
```


### CLI Usage

```sh
# --runs=n     Does n runs (eg. 3, 5), and reports the median run's numbers.
#              Median run selected by run with the median TTI.
pwmetrics http://example.com/ --runs=3


# --json       Reports json details to stdout.
pwmetrics --json http://example.com/

# returns...
# {runs: [{
#   "timings": [
#     {
#       "name": "First Contentful Paint",
#       "value": 289.642
#     },
#     {
#       "name": "First Meaningful Paint",
#       "value": 289.6
#     },
#     ...


# Useful for CI
# --expectations       Expectations from metrics results. Compares Lighthouse metrics with set expectations.

pwmetrics --expectations
# uses configurations from packages.json

pwmetrics --expectations=your-own-file.js
# uses path to your own file

```

### Defining expectations

```sh
# run pwmetrics with config in package.json
pwmetrics --expectations
```

`package.json`
```json
...
  "pwmetrics": {
    "expectations": {
      "url": "http://example.com/",
      "metrics": {
        "ttfmp": {
          "warn": ">=3000",
          "error": ">=5000"
        },
        "psi": {
          "warn": ">=1500",
          "error": ">=3200"
        }
      }
    }
  }
...
```


```sh
# run pwmetrics with config in your-own-file.js
pwmetrics --expectations=your-own-file.js
```

`your-own-file.js`
```js
module.exports = {
  expectations: {
    url: 'http://example.com/',
    metrics: {
      ttfmp: {
        warn: '>=3000',
        error: '>=5000'
      },
      psi: {
        warn: '>=1500',
        error: '>=3200'
      }
    }
  }
}

```

#### Available metrics: 

 - `ttfcp` - First Contentful Paint
 - `ttfmp` - First Meaningful Paint
 - `psi` - Perceptual Speed Index
 - `fv` - First Visual Change
 - `vc` - Visually Complete 100%
 - `tti` - Time to Interactive
 - `vc85` - Visually Complete 85%


### Default Lighthouse options

 - `disableCpuThrottling` is set `false` by default. It means that CPU throttling `5x` is enabled. To turn it off, run `pwmetrcis http:example.com --disableCpuThrottling`


### API

```js
const PWMetrics = require('pwmetrics');

const pwMetrics = new PWMetrics('http://example.com/', opts);
pwMetrics.start(); // returns Promise
```


### License

Apache 2.0. Google Inc.
