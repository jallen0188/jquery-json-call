# $.jsonCall

<a href="http://apostrophenow.org/"><img src="https://raw.github.com/punkave/jquery-json-call/master/logos/logo-box-madefor.png" align="right" /></a>

`$.jsonCall` makes it easy to make an API request to a URL with JSON in BOTH directions. Say goodbye to the limitations of the query string format. Your nulls will stay null! Your numbers will remain numbers!

## Why JSON in both directions?

"Wait, I don't quite get it. Why is this better than a regular `$.post` call?"

When you use `$.post` to send a data structure, everything comes through as an array, an object, or a string. By sending your request as JSON, you can keep the distinctions between `null`, `false`, numbers and strings alive.

(Of course you still need to sanitize the data you receive on the server side. Never trust a browser!)

## How to Use

    $.jsonCall('/my-api', { limit: 5, type: 'blue' }, function(results) {
      // Iterate over the results array output by the server on success
    }, function() {
      // Optional second function to be called on failure. This means
      // a failure at the HTTP level (404, 500, etc)
    });

Here's a node.js Express example on the server side.

    app.post('/my-api', function(req, res) {
      collection.find({ type: req.body.type }).limit(req.body.limit).toArray(function(err, results) {
        return res.send(results);
      });
    });

Notice we didn't have to do anything special on the server side to handle JSON? Express is great like that.

## Additional Options

Insert an additional argument *before* the data object to pass additional options.

Supported options are:

* `async`: if you explicitly set `async` to `false` the call is made synchronously. Normally you want to avoid this. However, it is crucial if you want your call to complete during a `beforeunload` event when the user leaves the page.

* `dataType`: normally a JSON response is expected. You may change that by setting the `dataType` option. You may set this to `*` for the default "intelligent guess" behavior of `$.ajax`. `html` is another useful value. This is handy if you wish to send JSON, but receive an HTML fragment. See the [jquery $.ajax documentation](http://api.jquery.com/jquery.ajax/) for details on this option.

## Requirements

You need jQuery, of course. `$.jsonCall` is actively supported with jQuery 1.9 and up but should work fine with somewhat older versions.

If you wish your code to work in browsers that do not support `JSON.stringify`, you will need [Douglas Crockford's JSON2 shim](https://github.com/douglascrockford/JSON-js) or a similar shim that provides `JSON.stringify`. However note that even IE8 supports it out of the box.

The server on the other end must understand requests posted with the `application/json` content type.

This is a pain in PHP, but extremely easy with node.js and Express: you will find the object you passed to `$.jsonCall` in `req.body`. Boom! That's it. [If you are stuck with PHP, you may find this helpful.](http://stackoverflow.com/questions/3063787/handle-json-request-in-php)

Unless you override with the `dataType` option, the server's response must also be JSON. However you don't have to set the content type header correctly as long as what you output is valid JSON.

Again, with Express you can just write:

    res.send({ an: object, with: properties, so: cool });

Arrays are valid responses too.

## About P'unk Avenue and Apostrophe

`$.jsonCall` was created at [P'unk Avenue](http://punkave.com) for use in Apostrophe, an open-source content management system built on node.js. If you like `jquery-json-call` you should definitely [check out apostrophenow.org](http://apostrophenow.org). Also be sure to visit us on [github](http://github.com/punkave).

## Support

Feel free to open issues on [github](http://github.com/punkave/jquery-json-call).

<a href="http://punkave.com/"><img src="https://raw.github.com/punkave/jquery-json-call/master/logos/logo-box-builtby.png" /></a>

