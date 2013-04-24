express-enrouten
==================


Route configuration middleware for expressjs.


### API
#### `enrouten(app).withRoutes(options)`
```javascript
var express = require('express'),
    enrouten = require('express-enrouten');

var app = express();
enrouten(app).withRoutes({ ... });
```


### Configuration
express-enrouten supports routes via configuration and convention.
```javascript
enrouten(app).withRoutes({
    directory: '',
    routes: [{
        method: 'get',
        path: '/foo',
        handler: function (req, res) {
            // ...
        }
    }]
});
```

- `directory` (optional) - String or array of path segments. Specify a directory to have enrouten scan all files recursively
to find files that match the controller-spec API.
```javascript
enrouten(app).withRoutes({
    directory: 'controllers'
});
```

A 'controller' is defined as any javascript file (extension of `.js`)
that exports a function which accepts a single argument. **NOTE: Any file in the directory tree that matches the API will
be invoked/initialized with the server object.**
```javascript
// Good :)
// controllers/controller.js
module.exports = function (app) {
    app.get('/', function (req, res) {
        // ...
    });
};

// Bad :(
// Function does not get returned when `require`-ed, use `module.exports`
exports = function (app) {
    // ...
};

// Bad :(
// controllers/other-file-in-same-controller-directory.js
modules.exports = function (config) {
    // `config` will the express application
    // ...
};

// Acceptable :)
// controllers/config.json - A non-js file
// controllers/README.txt - A non-js file
// controllers/util.js - A js file that has a different API than the spec
module.exports = {
    importantHelper: function () {

    }
};
```

- `routes` (optional) An array of route definition objects. Each definition must have a `path` and `handler` property and
can have an optional `method` property (`method` defaults to 'GET').
```javascript
enrouten(app).withRoutes({
    routes: [
        { path: '/',    method: 'GET', handler: require('./controllers/index') },
        { path: '/foo', method: 'GET', handler: require('./controllers/foo') }
    ]
});
```