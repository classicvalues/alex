<!--lint disable no-html first-heading-level maximum-line-length-->

<h1 align="center">
  <img width="400" src="https://rawgit.com/wooorm/alex/master/media/logo.svg" alt="alex">
  <br>
  <br>
</h1>

> 📝 **alex** — Catch insensitive, inconsiderate writing.

[![Build Status][travis-badge]][travis] [![Coverage Status][codecov-badge]][codecov]

Whether your own or someone else’s writing, **alex** helps you find gender
favouring, polarising, race related, religion inconsiderate, or other
**unequal** phrasing.

For example, when `We’ve confirmed his identity` is given to **alex**,
it will warn you and suggest using `their` instead of `his`.

> Suggestions, feature requests, and issues are more than welcome!

Give **alex** a spin on the [Online demo »][demo].

## Why

*   [x] To get better at considerate writing;
*   [x] Catches many possible offenses;
*   [x] Suggests helpful alternatives;
*   [x] Reads plain-text and markdown as input;
*   [x] Stylish.

## Install

[npm][] (with [Node.js][node]):

```sh
$ npm install alex --global
```

## Table of Contents

*   [Command Line](#command-line)

*   [API](#api)

    *   [alex(value)](#alexvalue)
    *   [alex.markdown(value, allow)](#alexmarkdownvalue-allow)
    *   [alex.text(value)](#alextextvalue)

*   [Integrations](#integrations)

*   [Support](#support)

*   [Ignoring files](#ignoring-files)

*   [.alexignore](#alexignore)

*   [Ignoring messages](#ignoring-messages)

    *   [.alexrc](#alexrc)
    *   [package.json](#packagejson)
    *   [Control](#control)

*   [Workflow](#workflow)

*   [FAQ](#faq)

    *   [Why is this named alex?](#why-is-this-named-alex)
    *   [Alex didn’t check ‘X’!](#alex-didnt-check-x)

*   [Contributing](#contributing)

*   [License](#license)

## Command Line

![Example of how alex looks on screen][screenshot]

Let’s say `example.md` looks as follows:

```md
The boogeyman wrote all changes to the **master server**. Thus, the slaves
were read-only copies of master. But not to worry, he was a cripple.
```

Then, run **alex** on `example.md`:

```sh
$ alex example.md
```

Yields:

```txt
example.md
   1:5-1:14  warning  `boogeyman` may be insensitive, use `boogey` instead
  1:42-1:48  warning  `master` / `slaves` may be insensitive, use `primary` / `replica` instead
  2:52-2:54  warning  `he` may be insensitive, use `they`, `it` instead
  2:59-2:66  warning  `cripple` may be insensitive, use `person with a limp` instead
```

See `$ alex --help` for more information.

> When no input files are given to **alex**, it searches for markdown and
> text files in the current directory, `doc`, and `docs`.

## API

[npm][]:

```sh
$ npm install alex --save
```

**alex** is also available for [bower][] and [duo][], and as an AMD,
CommonJS, and globals module, [uncompressed and compressed][releases].

### `alex(value)`

### `alex.markdown(value, allow)`

**Example**

```js
alex('We’ve confirmed his identity.').messages;
/*
 * [ { [1:17-1:20: `his` may be insensitive, use `their`, `theirs` instead]
 *   name: '1:17-1:20',
 *   file: '',
 *   reason: '`his` may be insensitive, use `their`, `theirs` instead',
 *   line: 1,
 *   column: 17,
 *   fatal: false } ]
 */
```

**Parameters**

*   `value` ([`VFile`][vfile] or `string`) —
    Markdown or plain-text;

*   `allow` (`Array.<string>`, optional) —
    List of allowed rules.

**Returns**

[`VFile`][vfile]. You’ll probably be interested in its
[`messages`][vfile-message] property, as demonstrated in the example
above, as it holds the possible violations.

### `alex.text(value)`

Works just like [`alex()`][alex-api], but does not parse as markdown
(thus things like code are not ignored)

**Example**

```js
alex('The `boogeyman`.').messages; // []

alex.text('The `boogeyman`.').messages;
/*
 * [ { [1:6-1:15: `boogeyman` may be insensitive, use `boogey` instead]
 *   name: '1:6-1:15',
 *   file: '',
 *   reason: '`boogeyman` may be insensitive, use `boogey` instead',
 *   line: 1,
 *   column: 6,
 *   fatal: false } ]
 */
```

## Integrations

*   Atom — [`wooorm/atom-linter-alex`](https://github.com/wooorm/atom-linter-alex)
*   Sublime — [`sindresorhus/SublimeLinter-contrib-alex`](https://github.com/sindresorhus/SublimeLinter-contrib-alex)
*   Visual Studio Code — [`shinnn/vscode-alex`](https://github.com/shinnn/vscode-alex)
*   Gulp — [`dustinspecker/gulp-alex`](https://github.com/dustinspecker/gulp-alex)
*   Slack — [`keoghpe/alex-slack`](https://github.com/keoghpe/alex-slack)

## Support

**alex** checks for many patterns of English language, and warns for:

*   Gendered work-titles, for example warning about `garbageman` and suggesting
    `garbage collector` instead;

*   Gendered proverbs, such as warning about `like a man` and suggesting
    `bravely` instead, or `ladylike` and suggesting `courteous`;

*   Blunt phrases, such as warning about `cripple` and suggesting
    `person with a limp` instead;

*   Intolerant phrasing, such as warning about using `master` and `slave`
    together, and suggesting `primary` and `replica` instead.

**alex** ignores words meant literally, so `“he”`, `He — ...`, and [the
like][literals] are not warned about

## Ignoring files

**alex** CLI searches for files with a markdown or text extension when given
directories (e.g., `$ alex .` will find `readme.md` and `foo/bar/baz.txt`).
To prevent files from being found by **alex**, add an
[`.alexignore`][alexignore] file.

## `.alexignore`

The **alex** CLI will sometimes [search for files][ignoring-files]. To prevent
files from being found, add a file named `.alexignore` in one of the
directories above the current working directory. The format of these files is
similar to [`.eslintignore`][eslintignore] (which is in turn similar to
`.gitignore` files).

For example, when working in `~/alpha/bravo/charlie`, the ignore file can be
in `charlie`, but also in `~`.

The ignore file for [this project itself][.alexignore]
looks as follows:

```txt
# Both `node_modules` and `bower_components` are
# ignored by default:
# node_modules/
# bower_components/

components/
example.md
```

## Ignoring messages

### `.alexrc`

### `package.json`

**alex** can silence message through `.alexrc` files:

```json
{
  "allow": ["boogeyman-boogeywoman"]
}
```

...or `package.json` files:

```txt
{
  ...
  "alex": {
    "allow": ["butt"]
  },
  ...
}
```

All `allow` fields in all `package.json` and `.alexrc` files are
detected and used when processing.

### Control

Sometimes, **alex** makes mistakes:

```md
A window will pop up.
```

Yields:

```txt
readme.md
  1:15-1:18  warning  `pop` may be insensitive, use `parent` instead  dad-mom

⚠ 1 warning
```

**alex** can silence message through HTML comments in markdown:

```md
<!--alex ignore dad-mom-->

A window will **not** pop up.
```

Yields:

```txt
readme.md: no issues found
```

`ignore` turns off messages for the thing after the comment (in this
case, the paragraph).
It’s also possible to turn off messages after a comment by using
`disable`, and, turn those messages back on using `enable`:

```md
<!--alex disable dad-mom-->

A window will **not** pop up.

A window will **not** pop up.

<!--alex enable dad-mom-->

A window will pop up.
```

Yields:

```txt
readme.md
  9:15-9:18  warning  `pop` may be insensitive, use `parent` instead  dad-mom

⚠ 1 warning
```

Multiple messages can be controlled in one go:

```md
<!--alex disable he-her his-hers dad-mom-->
```

...and all messages can be controlled by omitting all rule identifiers:

```md
<!--alex ignore-->
```

## Workflow

The recommended workflow is to add **alex** locally and to run it with your
tests.

A `package.json` file with [npm scripts][npm-scripts],
and additionally using [AVA][] for unit tests, could look
as follows:

```json
{
  "scripts": {
    "test-api": "ava",
    "test-doc": "alex",
    "test": "npm run test-api && npm run test-doc"
  },
  "devDependencies": {
    "alex": "^1.0.0",
    "ava": "^0.1.0"
  }
}
```

## FAQ

<!--lint disable no-heading-punctuation-->

### Why is this named alex?

It’s a nice androgynous/unisex name, it was free on npm, I like it! :smile:

### Alex didn’t check ‘X’!

See
[CONTRIBUTING.md][contributing] on how to get ‘X’ checked by alex.

<!--lint enable no-heading-punctuation-->

## Contributing

See [CONTRIBUTING.md][contributing].

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions. -->

[travis-badge]: https://img.shields.io/travis/wooorm/alex.svg

[travis]: https://travis-ci.org/wooorm/alex

[codecov-badge]: https://img.shields.io/codecov/c/github/wooorm/alex.svg

[codecov]: https://codecov.io/github/wooorm/alex

[demo]: http://alexjs.com/#demo

[npm]: https://docs.npmjs.com/cli/install

[node]: https://nodejs.org/en/download/

[screenshot]: screenshot.png

[bower]: http://bower.io/#install-packages

[duo]: http://duojs.org/#getting-started

[releases]: https://github.com/wooorm/alex/releases

[vfile]: https://github.com/wooorm/vfile

[vfile-message]: https://github.com/wooorm/vfile#vfilemessages

[alex-api]: #alexvalue

[literals]: https://github.com/wooorm/nlcst-is-literal#isliteralparent-index

[alexignore]: #alexignore

[ignoring-files]: #ignoring-files

[eslintignore]: http://eslint.org/docs/user-guide/configuring.html#ignoring-files-and-directories

[.alexignore]: https://github.com/wooorm/alex/blob/master/.alexignore

[npm-scripts]: https://docs.npmjs.com/misc/scripts

[ava]: http://ava.li

[contributing]: CONTRIBUTING.md

[license]: LICENSE

[author]: http://wooorm.com
