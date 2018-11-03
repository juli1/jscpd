# Copy/paste detector for programming source code.

`jscpd` is a tool for detect copy/paste "design pattern" in programming source code.

| _Supported languages_ |              |               |
|-----------------------|--------------|---------------|
| JavaScript            | Java         | YAML          |
| CoffeeScript          | C++          | Haxe          |
| PHP                   | C#           | TypeScript    |
| Go                    | Python       | Mixed HTML    |
| Ruby                  | C            | SCSS          |
| Less                  | CSS          | erlang        |
| Swift                 | xml/xslt     | Objective-C   |
| Puppet                | Twig         | Vue.js        |
| Scala                 | Lua          | Perl          |
| Kotlin                | PowerShell   |               |

If you need support language not from list feel free to create [request](https://github.com/kucherenko/jscpd/issues/new).

## Status

[![npm](https://img.shields.io/npm/v/jscpd.svg?style=flat-square)](https://www.npmjs.com/package/jscpd)
[![license](https://img.shields.io/github/license/kucherenko/jscpd.svg?style=flat-square)](https://github.com/kucherenko/jscpd/blob/master/LICENSE)
[![Travis](https://img.shields.io/travis/kucherenko/jscpd.svg?style=flat-square)](https://travis-ci.org/kucherenko/jscpd)
[![npm](https://img.shields.io/npm/dw/jscpd.svg?style=flat-square)](https://www.npmjs.com/package/jscpd)
[![Coveralls](https://img.shields.io/coveralls/kucherenko/jscpd.svg?style=flat-square)](https://coveralls.io/github/kucherenko/jscpd)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fkucherenko%2Fjscpd.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fkucherenko%2Fjscpd?ref=badge_shield)
[![Backers on Open Collective](https://opencollective.com/jscpd/backers/badge.svg)](#backers) 
[![Sponsors on Open Collective](https://opencollective.com/jscpd/sponsors/badge.svg)](#sponsors) 

[![NPM](https://nodei.co/npm/jscpd.png)](https://nodei.co/npm/jscpd/)

## Installation

    npm install jscpd -g

## Usage

```
jscpd --help
Usage:
  jscpd [OPTIONS]

Options:
  -l, --min-lines NUMBER min size of duplication in code lines
  -t, --min-tokens NUMBERmim size of duplication in code tokens
  -c, --config FILE      path to config file
  -f, --files STRING     glob pattern for find code
  -e, --exclude STRING   directory to ignore
      --skip-comments    skip comments in code
  -b, --blame BOOLEAN    blame authors of duplications (get information
                         about authors from git)
      --languages-exts STRINGlist of languages with file extensions
                             (language:ext1,ext2;language:ext3)
  -g, --languages STRING list of languages which scan for duplicates,
                         separated with comma
  -o, --output PATH      path to report file
  -r, --reporter STRING  reporter to use
  -x, --xsl-href STRING  path to xsl for include to xml report
      --verbose          show full info about copies
  -d, --debug            show debug information(options list and selected
                         files)
  -p, --path PATH        path to code
      --limit FLOAT      limit of allowed duplications, if real duplications
                         percent more then limit jscpd exit with error
  -v, --version          Display the current version
  -h, --help             Display help and usage details
```

Examples of using as CLI:

    jscpd --path my_project/ --languages javascript,coffee

    jscpd -f "**/*.js" -e "**/node_modules/**"

    jscpd --files "**/*.js" --exclude "**/*.min.js" --output report.xml

    jscpd --files "**/*.js" --exclude "**/*.min.js" --reporter json --output report.json

    jscpd --languages-exts javascript:es5,es6,es7,js;php:php5

    jscpd --config test/.cpd.yaml

Pre-defined options:

If you have file `.cpd.yaml` in your directory
```yaml
#.cpd.yaml
languages:
  - javascript
  - coffeescript
  - typescript
  - php
  - python
  - jsx
  - haxe
  - yaml
  - css
  - ruby
  - go
  - swift
  - twig
  - java
  - clike    # c++, c, objective-c source
  - csharp      # c# source
  - htmlmixed   # html mixed source like knockout.js templates
files:
  - "test/**/*"
exclude:
  - "**/*.min.js"
  - "**/*.mm.js"
reporter: json

languages-exts:
    coffeescript:
        - coffee
    javascript:
        - es
        - es5
        - es6
        - es7
```

and run `jscpd` command, you will check code for duplicates according config from .cpd.yaml

Run `jscpd` from source:

```coffeescript
# coffeescript
jscpd = require('jscpd')
result = jscpd::run
  path: 'my/project/folder'
  files: '**/*.js'
  exclude: ['**/*.min.js', '**/node_modules/**']
  reporter: json
```

Please see the [minimatch documentation](https://github.com/isaacs/minimatch) for more details.


## Options:

| Option             | Type      | Default       | Description
|--------------------|-----------|---------------|-------------------------------------------------------------
| -l, --min-lines    | [NUMBER]  | 5             | min size of duplication in code lines
| -t, --min-tokens   | [NUMBER]  | 70            | min size of duplication in code tokens
| -f, --files        | [STRING]  | *             | glob pattern for find code
| -r, --reporter     | [STRING]  | xml           | reporter name or path
| -x, --xsl-href     | [STRING]  | -             | path to xsl file for include to xml report
| -e, --exclude      | [STRING]  | -             | directory to ignore
|    --languages-exts| [STRING]  | -             | list of languages with file extensions (e.g. language:ext1,ext2;language:ext3)
| -g, --languages    | [STRING]  | All supported | list of languages which scan for duplicates, separated with comma
| -o, --output       | [PATH]    | -             | path to report file
| -c, --config       | [PATH]    | -             | path to config yml file  (e.g. .cpd.yml)
|     --verbose      |           | -             | show full info about copies
|     --skip-comments| false     | -             | skip comments in code when duplications finding
| -b, --blame        | false     | -             | blame authors of duplications (get information about authors from git)
| -p, --path         | [PATH]    | Current dir   | path to code
|     --limit        | [FLOAT ]  | 50.0          | limit of allowed duplications, if real duplications percent more then limit jscpd exit with error
| -d, --debug        |           | -             | show debug information (options list and selected files)
| -v, --version      |           | -             | Display the current version
| -h, --help         |           | -             | Display help and usage details

Verbose output:

![verbose duplication](images/jscpd_verbose_screenshot.png)

Blame mode use information from git blame and concat it with duplications report:

![blame duplication](images/jscpd_blame_screenshot.png)

## Reporters

`jscpd` shipped with two standard reporters `xml` and [`json`](test/reporters/json-report.schema.json). It is possible to write custom reporter script too. For hooking reporter up wrap it into node module and provide path to it as `reporter` parameter e.g. `./scripts/jscpd-custom-reporter.coffee` (works with javascript too).

Custom reporter is a function which is executed into context of `Report` (`report.coffee`) class and thus has access to the report object and options. Expected output is array with following elements:

`[raw, dump, log]`

- `raw` is raw report object which will be passed through.
- `dump` is report which will be written into output file if any provided.
- `log` custom log output for cli.

At least one of `raw` or `dump` needs to be provided, `log` is fully optional.


## XSLT reports support

You can point xsl file for add it to xml report

```
jscpd -x reporters-xslt/simple.xsl -p test/fixtures/ -r xml -o report.xml
```

In this case report.xml will include following lines:

```
<?xml version='1.0' encoding='UTF-8' ?>
<?xml-stylesheet type="text/xsl" href="reporters-xslt/simple.xsl"?>
<pmd-cpd>
    <!-- ... -->
</pmd-cpd>
```
If you open xml file in browser template from `reporters-xslt/simple.xsl` will apply to your xml and show pretty html report.
You can find example of xsl template in reporters-xslt folder.

## Errors

 - **JSCPD Error 01**: {language} in not supported  -  error will show if you try to find duplication in not supported language
 - **JSCPD Error 02**: can't read config file {path} - you use wrong path to file or file is broken

## Run tests

```
  npm test
```

```
  npm run coverage
```

## Changelog


[Project changelog](https://github.com/kucherenko/jscpd/blob/master/changelog.md)

## TODO

[Project plans](https://github.com/kucherenko/jscpd/blob/master/todo.md)

## Contributors

This project exists thanks to all the people who contribute. 
<a href="https://github.com/kucherenko/jscpd/contributors"><img src="https://opencollective.com/jscpd/contributors.svg?width=890&button=false" /></a>


## Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/jscpd#backer)]

<a href="https://opencollective.com/jscpd#backers" target="_blank"><img src="https://opencollective.com/jscpd/backers.svg?width=890"></a>


## Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/jscpd#sponsor)]

<a href="https://opencollective.com/jscpd/sponsor/0/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/1/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/2/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/3/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/4/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/5/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/6/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/7/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/8/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/jscpd/sponsor/9/website" target="_blank"><img src="https://opencollective.com/jscpd/sponsor/9/avatar.svg"></a>



## License

[The MIT License](https://github.com/kucherenko/jscpd/blob/master/LICENSE)


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fkucherenko%2Fjscpd.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fkucherenko%2Fjscpd?ref=badge_large)

## Thanks

Thanks to [Mathieu Desv?](https://github.com/mazerte) for [grunt-jscpd](https://github.com/mazerte/grunt-jscpd).
Thanks to [Yannick Croissant](https://yannick.cr/) for [gulp-jscpd](https://github.com/yannickcr/gulp-jscpd).
Thanks to [linslin](https://github.com/linslin) for [grunt-jscpd-reporter](https://github.com/linslin/grunt-jscpd-reporter).

Project developed with [PyCharm](http://www.jetbrains.com/pycharm/)
![alt pycharm](http://www.jetbrains.com/img/logos/pycharm_logo.gif)
Thanks to [JetBrains](http://www.jetbrains.com/) company for license key.
Feel free to contribute this project and you will have chance to get license key too.