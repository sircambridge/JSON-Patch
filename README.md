JSON-Patch
==========

A leaner and meaner implementation of JSON-Patch. Small footprint. High performance.
Also support dual directions! I.e. you can both apply patches and generate patches.

## Why you should use JSON-Patch

JSON-Patch [(RFC6902)](http://tools.ietf.org/html/rfc6902) is a new standard format that 
allows you to update a JSON document by sending the changes rather than the whole document. 
JSON Patch plays well with the HTTP PATCH verb (method) and REST style programming.

Mark Nottingham has a [nice blog]( http://www.mnot.net/blog/2012/09/05/patch) about it.

## Footprint
0.5 KB minified and gzipped (1.1 KB minified)

## Performance
![Fast](http://www.rebelslounge.com/res/jsonpatch/chart3.png)


## Features
* Allows you to apply patches on object trees for incoming traffic.
* Allows you to freely manipulate object trees and then generate patches for outgoing traffic.
* ES6/7 Object.observe() is used when available.

## Roadmap

* A /bin directory will be added with minified versions
* More unit tests

## Usage

Include `json-patch.js` if you want support for applying patches **or**
include `json-patch-duplex.js` if you also want to generate patches.

Applying patches:
```js
var myobj = { firstName:"Albert", contactDetails: { phonenumbers: [ ] } };
var patches = [
   {op:"replace", path:"/firstName", value:"Joachim" },
   {op:"add", path:"/lastName", value:"Wester" }
   {op:"add", path:"/contactDetails/phonenumbers", value:{ number:"555-123" }  }
   ];
jsonpatch.apply( myobj, patches );
// myobj == { firstName:"Joachim", lastName:"Wester", contactDetails:{ phoneNumbers[ {number:"555-123"} ] } };
```
Generating patches:
```js
var myobj = { firstName:"Joachim", lastName:"Wester", contactDetails: { phonenumbers: [ { number:"555-123" }] } };
observer = jsonpatch.observe( myobj );
myobj.firstName = "Albert";
myobj.contactDetails.phonenumbers[0] = "123";
myobj.contactDetails.phonenumbers.push({number:"456"});
jsonpatch.generate(observer);
// patches  == [
//   { op:"replace", path="/firstName", value:"Albert"},
//   { op:"replace", path="/contactDetails/phonenumbers/0", value:"123"},
//   { op:"add", path="/contactDetails/phonenumbers", value:{number:"456"}}];
```

## Testing

### In a web browser

1. Testing **json-patch.js**
 - Load `src/patchtest.html` in your web browser
2. Testing **json-patch-duplex.js**
 - Load `src/test-duplex.html` in your web browser

Each of the test suite files contains *Jasmine* unit test suite and *JSLitmus* performance test suite.

To run *JSLitmus* performance tests, press "Run Tests" button.

### In Node.js

1. Go to directory where you have cloned the repo
2. Install Jasmine Node.js module by running command `npm install jasmine-node -g`
3. Testing **json-patch.js**
 - Run command `jasmine-node --matchall --config duplex no src/test.js`
4. Testing **json-patch-duplex.js**
 - Run command `jasmine-node --matchall --config duplex yes src/test.js src/test-duplex.js`
