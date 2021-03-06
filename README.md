# express-http-proxy [![NPM version](https://badge.fury.io/js/express-http-proxy.svg)](http://badge.fury.io/js/express-http-proxy) [![Build Status](https://travis-ci.org/villadora/express-http-proxy.svg?branch=master)](https://travis-ci.org/villadora/express-http-proxy) [![Dependency Status](https://gemnasium.com/villadora/express-http-proxy.svg)](https://gemnasium.com/villadora/express-http-proxy)


*NOTE*: As work content changed, I have no spare time to maintaining this node module, It's appreciated if anyone want to take or keep maintaining, just contact me via jky239@gmail.com with Title contains: "Wanted: npm package xxxx". Thx.


Express proxy middleware to forward request to another host and pass response back

## Install

```bash
$ npm install express-http-proxy --save
```

## Usage
If you want to set up proxy for URL starting with `/proxy` 
```js
var proxy = require('express-http-proxy');

var app = require('express')();

app.use('/proxy', proxy('www.google.com', {
  forwardPath: function(req, res) {
    return require('url').parse(req.url).path;
  }
}));

```

If you only want to proxy get request


```js
app.use('/proxy', proxy('www.google.com', {
  filter: function(req, res) {
     return req.method == 'GET';
  },
  forwardPath: function(req, res) {
    return require('url').parse(req.url).path;
  }
}));
```



You can also intercept the response before sending it back to the client, or change the request options before it is sent to the target:

```js
app.use('/proxy', proxy('www.google.com', {
  forwardPath: function(req, res) {
    return require('url').parse(req.url).path;
  },
  intercept: function(rsp, data, req, res, callback) {
       // rsp - original response from the target
       data = JSON.parse(data.toString('utf8'));
       callback(null, JSON.stringify(data));
  },
  decorateRequest: function(req) {
       req.headers['Content-Type'] = '';
       req.method = 'GET';
       req.bodyContent = wrap(req.bodyContent);
       return req;
  }
}));

```

You can copy the host HTTP header to the proxied express server using the `preserveHostHdr` option.

```
app.use('/proxy', proxy('www.google.com', {
  forwardPath: function(req, res) {
    return require('url').parse(req.url).path;
  },
  preserveHostHdr: true
}));
```

## Release Notes

| Release | Notes |
| --- | --- |
| 0.4.0 | Signature of `intercept` callback changed from `function(data, req, res, callback)` to `function(rsp, data, req, res, callback)` where `rsp` is the original response from the target |

## Licence

MIT
<!-- do not want to make nodeinit to complicated, you can edit this whenever you want. -->
