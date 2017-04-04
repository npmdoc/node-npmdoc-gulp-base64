# api documentation for  [gulp-base64 (v0.1.3)](http://github.com/Wenqer/gulp-base64)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-base64.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-base64) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-base64.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-base64)
#### gulp task to encode images to data URI

[![NPM](https://nodei.co/npm/gulp-base64.png?downloads=true)](https://www.npmjs.com/package/gulp-base64)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-base64/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-base64_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-base64/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-base64/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-base64/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Yaroslav 2gis"
    },
    "bugs": {
        "url": "https://github.com/Wenqer/gulp-base64/issues"
    },
    "dependencies": {
        "async": "~0.2.10",
        "buffers": "~0.1.1",
        "extend": "~1.2.1",
        "mime": "~1.2.11",
        "request": "~2.33.0",
        "through2": "~0.4.1"
    },
    "description": "gulp task to encode images to data URI",
    "devDependencies": {},
    "directories": {},
    "dist": {
        "shasum": "164bd9d4f336dc16d669b331cebc139343579145",
        "tarball": "https://registry.npmjs.org/gulp-base64/-/gulp-base64-0.1.3.tgz"
    },
    "gitHead": "e68b16ed02deab92a2cf438b8d5c2f4b449446d5",
    "homepage": "http://github.com/Wenqer/gulp-base64",
    "keywords": [
        "gulpplugin",
        "data-uri",
        "base64"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "wenqer",
            "email": "wenqeer@gmail.com"
        }
    ],
    "name": "gulp-base64",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/Wenqer/gulp-base64.git"
    },
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "version": "0.1.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-base64](#apidoc.module.gulp-base64)
1.  object <span class="apidocSignatureSpan">gulp-base64.</span>encode
1.  object <span class="apidocSignatureSpan">gulp-base64.</span>fetch

#### [module gulp-base64.encode](#apidoc.module.gulp-base64.encode)
1.  [function <span class="apidocSignatureSpan">gulp-base64.encode.</span>getDataURI (mimeType, img)](#apidoc.element.gulp-base64.encode.getDataURI)
1.  [function <span class="apidocSignatureSpan">gulp-base64.encode.</span>image (img, opts, done)](#apidoc.element.gulp-base64.encode.image)
1.  [function <span class="apidocSignatureSpan">gulp-base64.encode.</span>stylesheet (file, opts, done)](#apidoc.element.gulp-base64.encode.stylesheet)

#### [module gulp-base64.fetch](#apidoc.module.gulp-base64.fetch)
1.  [function <span class="apidocSignatureSpan">gulp-base64.fetch.</span>image (url, done)](#apidoc.element.gulp-base64.fetch.image)



# <a name="apidoc.module.gulp-base64"></a>[module gulp-base64](#apidoc.module.gulp-base64)



# <a name="apidoc.module.gulp-base64.encode"></a>[module gulp-base64.encode](#apidoc.module.gulp-base64.encode)

#### <a name="apidoc.element.gulp-base64.encode.getDataURI"></a>[function <span class="apidocSignatureSpan">gulp-base64.encode.</span>getDataURI (mimeType, img)](#apidoc.element.gulp-base64.encode.getDataURI)
- description and source-code
```javascript
getDataURI = function (mimeType, img) {
  var ret = "data:";
  ret += mimeType;
  ret += ";base64,";
  ret += img.toString("base64");
  return ret;
}
```
- example usage
```shell
...
  // External URL?
} else if(rExternal.test(img)) {
  // grunt.log.writeln("Encoding file: " + img);
  fetch.image(img, function(err, src, cacheable) {
    var encoded, type;
    if (err == null) {
      type = mime.lookup(img);
      encoded = exports.getDataURI(type, src);
    }
    complete(err, encoded, cacheable);
  } );

  // Local file?
} else {
  // Does the image actually exist?
...
```

#### <a name="apidoc.element.gulp-base64.encode.image"></a>[function <span class="apidocSignatureSpan">gulp-base64.encode.</span>image (img, opts, done)](#apidoc.element.gulp-base64.encode.image)
- description and source-code
```javascript
image = function (img, opts, done) {

  // Shift args
  if(typeof opts === "function") {
    done = opts;
    opts = {};
  }

  // Set default, helper-specific options
  opts = extend({
    maxImageSize: 32768
  }, opts);

  var complete = function(err, encoded, cacheable) {
    // Too long?
    if(cacheable && encoded && opts.maxImageSize && encoded.length > opts.maxImageSize) {
      err = "Skipping " + img + " (greater than " + opts.maxImageSize + " bytes)";
    }

    // Return the original source if an error occurred
    if(err) {
      // grunt.log.error(err);
      done(err, img, false);

      // Otherwise cache the processed image and return it
    } else {
      done(null, encoded, cacheable);
    }
  };

  // Already base64 encoded?
  if(rData.test(img)) {
    complete(null, img, false);

    // External URL?
  } else if(rExternal.test(img)) {
    // grunt.log.writeln("Encoding file: " + img);
    fetch.image(img, function(err, src, cacheable) {
      var encoded, type;
      if (err == null) {
        type = mime.lookup(img);
        encoded = exports.getDataURI(type, src);
      }
      complete(err, encoded, cacheable);
    } );

    // Local file?
  } else {
    // Does the image actually exist?
    if(!fs.existsSync(img) || !fs.lstatSync(img).isFile()) {
      // grunt.fail.warn("File " + img + " does not exist");
      if (opts.debug) {
        console.warn("File " + img + " does not exist");
      }
      complete(true, img, false);
      return;
    }

    // grunt.log.writeln("Encoding file: " + img);
    if (opts.debug) {
      console.info("Encoding file: " + img);
    }

    // Read the file in and convert it.
    var src = fs.readFileSync(img);
    var type = mime.lookup(img);
    var encoded = exports.getDataURI(type, src);
    complete(null, encoded, true);
  }
}
```
- example usage
```shell
...
          }

          // Test for scheme less URLs => "//example.com/image.png"
          if (!is_local_file && rSchemeless.test(loc)) {
            loc = 'http:' + loc;
          }

          exports.image(loc, opts, function (err, resp, cacheable) {
            if (err == null) {
var url = "url(" + resp + ")";
result += url;

if(cacheable !== false) {
  cache[img] = url;
}
...
```

#### <a name="apidoc.element.gulp-base64.encode.stylesheet"></a>[function <span class="apidocSignatureSpan">gulp-base64.encode.</span>stylesheet (file, opts, done)](#apidoc.element.gulp-base64.encode.stylesheet)
- description and source-code
```javascript
stylesheet = function (file, opts, done) {
  opts = opts || {};

  // Cache of already converted images
  var cache = {};

  // Shift args if no options object is specified
  if(typeof opts === "function") {
    done = opts;
    opts = {};
  }

  var deleteAfterEncoding = opts.deleteAfterEncoding;
  var src = file.contents.toString();
  var result = "";
  var match, img, line, tasks, group;

  async.whilst(function() {
    group = rImages.exec(src);
    return group != null;
  },
  function(complete) {
    // if there is another url to be processed, then:
    //    group[1] will hold everything up to the url declaration
    //    group[2] will hold the complete url declaration (useful if no encoding will take place)
    //    group[3] will hold the contents of the url declaration
    //    group[4] will be undefined
    // if there is no other url to be processed, then group[1-3] will be undefined
    //    group[4] will hold the entire string

    // console.log(group[2]);

    if(group[4] == null) {
      result += group[1];

      var rawUrl = group[3].trim();
      img = rawUrl
        .replace(rQuotes, "")
        .replace(rParams, ""); // remove query string/hash parmams in the filename, like foo.png?bar or foo.png#bar

      var test = true;
      if (opts.extensions) { //test for extensions if it provided
        var imgExt = img.split('.').pop();
        test = opts.extensions.some(function (ext) {
          return (ext instanceof RegExp) ? ext.test(rawUrl) : (ext === imgExt);
        });
      }
      if (test && opts.exclude) { //test for extensions to exclude if it provided
        test = !opts.exclude.some(function (pattern) {
          return (pattern instanceof RegExp) ? pattern.test(rawUrl) : (rawUrl.indexOf(pattern) > -1);
        });
      }
      if (!test) {
        if (opts.debug) {
          console.log(img + ' skipped by extension or exclude filters');
        }
        result += group[2];
        return complete();
      }
      // see if this img was already processed before...
      if(cache[img]) {
        // grunt.log.error("The image " + img + " has already been encoded elsewhere in your stylesheet. I'm going to do it again
, but it's going to make your stylesheet a lot larger than it needs to be.");
        result = result += cache[img];
        complete();
      } else {
        // process it and put it into the cache
        var loc = img,
            is_local_file = !rData.test(img) && !rExternal.test(img);

        // Resolve the image path relative to the CSS file
        if (is_local_file) {
          // local file system.. fix up the path
          // loc = path.join(path.dirname(file.path), img);

          loc = opts.baseDir ? path.join(opts.baseDir, img) :
              path.join(path.dirname(file.path), img);

          // If that didn't work, try finding the image relative to
          // the current file instead.
          if(!fs.existsSync(loc)) {
            if (opts.debug) {
              console.log('in ' + loc + ' file doesn\'t exist');
            }
            loc = path.join(file.cwd, img);
          }
        }

        // Test for scheme less URLs => "//example.com/image.png"
        if (!is_local_file && rSchemeless.test(loc)) {
          loc = 'http:' + loc;
        }

        exports.image(loc, opts, function (err, resp, cacheable) {
          if (err == null) {
            var url = "url(" + resp + ")";
            result += url;

            if(cacheable !== false) {
              cache[img] = url;
            }

            if(deleteAfterEncoding && is_local_file) {
              // grunt.log.writeln("deleting " + loc);
              fs.unlinkSync(loc);
            }
          } else {
            result += group[2];
          }

          complete();
        });
      }
    } else {
      result += group[4];
      complete();
    }
  },
  function() {
    done(null, result);
  });
}
```
- example usage
```shell
...
var encode = require('./lib/encode');

module.exports = function (opts) {

    function rebase(file, encoding, callback) {
        var self = this;

        encode.stylesheet(file, opts, function (err, src) {
if (err) {
    console.error(err);
}
file.contents = new Buffer(src);

self.push(file);
callback();
...
```



# <a name="apidoc.module.gulp-base64.fetch"></a>[module gulp-base64.fetch](#apidoc.module.gulp-base64.fetch)

#### <a name="apidoc.element.gulp-base64.fetch.image"></a>[function <span class="apidocSignatureSpan">gulp-base64.fetch.</span>image (url, done)](#apidoc.element.gulp-base64.fetch.image)
- description and source-code
```javascript
image = function (url, done) {
    var resultBuffer;
    var buffList = buffers();
    var imageStream = new stream.Stream();

    imageStream.writable = true;
    imageStream.write = function (data) { buffList.push(new Buffer(data)); };
    imageStream.end = function () { resultBuffer = buffList.toBuffer(); };

    request(url, function (error, response/*, body*/) {
        if (error) {
            done("Unable to get " + url + ". Error: " + error.message);
            return;
        }

        // Bail if we get anything other than 200
        if (response.statusCode !== 200) {
            done("Unable to get " + url + " because the URL did not return an image. Status code " + response.statusCode + " received
");
            return;
        }

        done(null, resultBuffer, true);
    }).pipe(imageStream);
}
```
- example usage
```shell
...
          }

          // Test for scheme less URLs => "//example.com/image.png"
          if (!is_local_file && rSchemeless.test(loc)) {
            loc = 'http:' + loc;
          }

          exports.image(loc, opts, function (err, resp, cacheable) {
            if (err == null) {
var url = "url(" + resp + ")";
result += url;

if(cacheable !== false) {
  cache[img] = url;
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
