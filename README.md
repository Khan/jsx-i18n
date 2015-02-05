# jsx-i18n

Simple [jstransform](https://github.com/facebook/jstransform) pass for use with the JSX transformer in React 0.12+ to convert JSX `<$_>...</$_>` tags (only) to `$_(...)` calls, to simulate how React 0.11 behaved.

## Usage

You can write a simple script along the line of the following:

```
var visitors = require("react-tools/vendor/fbtransform/visitors");
var jstransform = require('jstransform');
var jsxI18n = require('jsx-i18n');


process.stdin.resume();
process.stdin.setEncoding("utf-8");

var js = "";
process.stdin.on("data", function(data) {
    js += data;
});

process.stdin.on("end", function() {
    var visitorList = jsxI18n.getVisitorList([
        "$_"
    ]).concat(visitors.getAllVisitors());
    js = jstransform.transform(visitorList, js).code;

    process.stdout.write(js);
});
```

This script reads JSX from stdin and writes JS to stdout. Example:

```
// in
var x = <div><$_ name={world}>hello %(name)s</$_></div>;

// out
var x = React.createElement("div", null, $_({name: world}, "hello %(name)s"));
```
