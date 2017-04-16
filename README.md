# api documentation for  [sprintf-js (v1.0.3)](https://github.com/alexei/sprintf.js#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-sprintf-js.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-sprintf-js) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-sprintf-js.svg)](https://travis-ci.org/npmdoc/node-npmdoc-sprintf-js)
#### JavaScript sprintf implementation

[![NPM](https://nodei.co/npm/sprintf-js.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/sprintf-js)

[![apidoc](https://npmdoc.github.io/node-npmdoc-sprintf-js/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-sprintf-js/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-sprintf-js/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-sprintf-js/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Alexandru Marasteanu",
        "url": "http://alexei.ro/"
    },
    "bugs": {
        "url": "https://github.com/alexei/sprintf.js/issues"
    },
    "dependencies": {},
    "description": "JavaScript sprintf implementation",
    "devDependencies": {
        "grunt": "*",
        "grunt-contrib-uglify": "*",
        "grunt-contrib-watch": "*",
        "mocha": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "04e6926f662895354f3dd015203633b857297e2c",
        "tarball": "https://registry.npmjs.org/sprintf-js/-/sprintf-js-1.0.3.tgz"
    },
    "gitHead": "747b806c2dab5b64d5c9958c42884946a187c3b1",
    "homepage": "https://github.com/alexei/sprintf.js#readme",
    "license": "BSD-3-Clause",
    "main": "src/sprintf.js",
    "maintainers": [
        {
            "name": "alexei"
        }
    ],
    "name": "sprintf-js",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/alexei/sprintf.js.git"
    },
    "scripts": {
        "test": "mocha test/test.js"
    },
    "version": "1.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module sprintf-js](#apidoc.module.sprintf-js)
1.  [function <span class="apidocSignatureSpan">sprintf-js.</span>sprintf ()](#apidoc.element.sprintf-js.sprintf)
1.  [function <span class="apidocSignatureSpan">sprintf-js.</span>vsprintf (fmt, argv, _argv)](#apidoc.element.sprintf-js.vsprintf)

#### [module sprintf-js.sprintf](#apidoc.module.sprintf-js.sprintf)
1.  [function <span class="apidocSignatureSpan">sprintf-js.</span>sprintf ()](#apidoc.element.sprintf-js.sprintf.sprintf)
1.  [function <span class="apidocSignatureSpan">sprintf-js.sprintf.</span>format (parse_tree, argv)](#apidoc.element.sprintf-js.sprintf.format)
1.  [function <span class="apidocSignatureSpan">sprintf-js.sprintf.</span>parse (fmt)](#apidoc.element.sprintf-js.sprintf.parse)
1.  object <span class="apidocSignatureSpan">sprintf-js.sprintf.</span>cache



# <a name="apidoc.module.sprintf-js"></a>[module sprintf-js](#apidoc.module.sprintf-js)

#### <a name="apidoc.element.sprintf-js.sprintf"></a>[function <span class="apidocSignatureSpan">sprintf-js.</span>sprintf ()](#apidoc.element.sprintf-js.sprintf)
- description and source-code
```javascript
function sprintf() {
    var key = arguments[0], cache = sprintf.cache
    if (!(cache[key] && cache.hasOwnProperty(key))) {
        cache[key] = sprintf.parse(key)
    }
    return sprintf.format.call(null, cache[key], arguments)
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.sprintf-js.vsprintf"></a>[function <span class="apidocSignatureSpan">sprintf-js.</span>vsprintf (fmt, argv, _argv)](#apidoc.element.sprintf-js.vsprintf)
- description and source-code
```javascript
vsprintf = function (fmt, argv, _argv) {
    _argv = (argv || []).slice(0)
    _argv.splice(0, 0, fmt)
    return sprintf.apply(null, _argv)
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.sprintf-js.sprintf"></a>[module sprintf-js.sprintf](#apidoc.module.sprintf-js.sprintf)

#### <a name="apidoc.element.sprintf-js.sprintf.sprintf"></a>[function <span class="apidocSignatureSpan">sprintf-js.</span>sprintf ()](#apidoc.element.sprintf-js.sprintf.sprintf)
- description and source-code
```javascript
function sprintf() {
    var key = arguments[0], cache = sprintf.cache
    if (!(cache[key] && cache.hasOwnProperty(key))) {
        cache[key] = sprintf.parse(key)
    }
    return sprintf.format.call(null, cache[key], arguments)
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.sprintf-js.sprintf.format"></a>[function <span class="apidocSignatureSpan">sprintf-js.sprintf.</span>format (parse_tree, argv)](#apidoc.element.sprintf-js.sprintf.format)
- description and source-code
```javascript
format = function (parse_tree, argv) {
    var cursor = 1, tree_length = parse_tree.length, node_type = "", arg, output = [], i, k, match, pad, pad_character, pad_length
, is_positive = true, sign = ""
    for (i = 0; i < tree_length; i++) {
        node_type = get_type(parse_tree[i])
        if (node_type === "string") {
            output[output.length] = parse_tree[i]
        }
        else if (node_type === "array") {
            match = parse_tree[i] // convenience purposes only
            if (match[2]) { // keyword argument
                arg = argv[cursor]
                for (k = 0; k < match[2].length; k++) {
                    if (!arg.hasOwnProperty(match[2][k])) {
                        throw new Error(sprintf("[sprintf] property '%s' does not exist", match[2][k]))
                    }
                    arg = arg[match[2][k]]
                }
            }
            else if (match[1]) { // positional argument (explicit)
                arg = argv[match[1]]
            }
            else { // positional argument (implicit)
                arg = argv[cursor++]
            }

            if (get_type(arg) == "function") {
                arg = arg()
            }

            if (re.not_string.test(match[8]) && re.not_json.test(match[8]) && (get_type(arg) != "number" && isNaN(arg))) {
                throw new TypeError(sprintf("[sprintf] expecting number but found %s", get_type(arg)))
            }

            if (re.number.test(match[8])) {
                is_positive = arg >= 0
            }

            switch (match[8]) {
                case "b":
                    arg = arg.toString(2)
                break
                case "c":
                    arg = String.fromCharCode(arg)
                break
                case "d":
                case "i":
                    arg = parseInt(arg, 10)
                break
                case "j":
                    arg = JSON.stringify(arg, null, match[6] ? parseInt(match[6]) : 0)
                break
                case "e":
                    arg = match[7] ? arg.toExponential(match[7]) : arg.toExponential()
                break
                case "f":
                    arg = match[7] ? parseFloat(arg).toFixed(match[7]) : parseFloat(arg)
                break
                case "g":
                    arg = match[7] ? parseFloat(arg).toPrecision(match[7]) : parseFloat(arg)
                break
                case "o":
                    arg = arg.toString(8)
                break
                case "s":
                    arg = ((arg = String(arg)) && match[7] ? arg.substring(0, match[7]) : arg)
                break
                case "u":
                    arg = arg >>> 0
                break
                case "x":
                    arg = arg.toString(16)
                break
                case "X":
                    arg = arg.toString(16).toUpperCase()
                break
            }
            if (re.json.test(match[8])) {
                output[output.length] = arg
            }
            else {
                if (re.number.test(match[8]) && (!is_positive || match[3])) {
                    sign = is_positive ? "+" : "-"
                    arg = arg.toString().replace(re.sign, "")
                }
                else {
                    sign = ""
                }
                pad_character = match[4] ? match[4] === "0" ? "0" : match[4].charAt(1) : " "
                pad_length = match[6] - (sign + arg).length
                pad = match[6] ? (pad_length > 0 ? str_repeat(pad_character, pad_length) : "") : ""
                output[output.length] = match[5] ? sign + arg + pad : (pad_character === "0" ? sign + pad + arg : pad + sign + arg
)
            }
        }
    }
    return output.join("")
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.sprintf-js.sprintf.parse"></a>[function <span class="apidocSignatureSpan">sprintf-js.sprintf.</span>parse (fmt)](#apidoc.element.sprintf-js.sprintf.parse)
- description and source-code
```javascript
parse = function (fmt) {
    var _fmt = fmt, match = [], parse_tree = [], arg_names = 0
    while (_fmt) {
        if ((match = re.text.exec(_fmt)) !== null) {
            parse_tree[parse_tree.length] = match[0]
        }
        else if ((match = re.modulo.exec(_fmt)) !== null) {
            parse_tree[parse_tree.length] = "%"
        }
        else if ((match = re.placeholder.exec(_fmt)) !== null) {
            if (match[2]) {
                arg_names |= 1
                var field_list = [], replacement_field = match[2], field_match = []
                if ((field_match = re.key.exec(replacement_field)) !== null) {
                    field_list[field_list.length] = field_match[1]
                    while ((replacement_field = replacement_field.substring(field_match[0].length)) !== "") {
                        if ((field_match = re.key_access.exec(replacement_field)) !== null) {
                            field_list[field_list.length] = field_match[1]
                        }
                        else if ((field_match = re.index_access.exec(replacement_field)) !== null) {
                            field_list[field_list.length] = field_match[1]
                        }
                        else {
                            throw new SyntaxError("[sprintf] failed to parse named argument key")
                        }
                    }
                }
                else {
                    throw new SyntaxError("[sprintf] failed to parse named argument key")
                }
                match[2] = field_list
            }
            else {
                arg_names |= 2
            }
            if (arg_names === 3) {
                throw new Error("[sprintf] mixing positional and named placeholders is not (yet) supported")
            }
            parse_tree[parse_tree.length] = match
        }
        else {
            throw new SyntaxError("[sprintf] unexpected placeholder")
        }
        _fmt = _fmt.substring(match[0].length)
    }
    return parse_tree
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
