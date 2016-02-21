# Sublime Libsass

Build system for SASS/SCSS files in Sublime Text 2/3 with no external dependencies

It uses libsass (via the official sassc front-end) for faster compile times compared to the official Ruby implementation.

https://github.com/blitzrk/sublime_libsass

## Configuration

By default, sassc is given no arguments and css files are put in `build/css` relative to the assumed root of the project (the outermost parent directory containing a sass file from the file being compiled). The default output directory and sassc flags can be modified `Preferences->Package Settings->Libsass Build->Settings - User`. There is a skeleton in `Settings - Default` or see below.

A project-specific configuration can written to `.libsass.json` file. Additionally, the location of this file will make explicit the root of the project as far as where sass imports and the output directory is based. You can edit this config or generate an example by selecting the menu item `Tools`->`Libsass Build`->`Edit Project Config`.

Example:

You have the directory structure:

```
/path/to/projects/example
|-- lib/
|-- static/
|   |-- js/
|   |-- css/
|   |   +-- main.css
|   +-- images/
|-- .libsass.json
+-- styles
    |-- partials
    |   +-- _reset.scss
    +-- main.scss (has @import 'partials/reset';)
```

Then you could use:

`.libsass.json`:

```js
{
	"output_dir": "static/css",
	"options": {
		"line-comments": true,
		"line-numbers":  true,
		"style":         "nested", // Comments allowed in `.libsass.json`, but not `*.sublime-settings`
		"load-path":     "/path/to/projects/example/styles" // You DO NOT need this line
	}
}
```

or alternatively in `/path/to/projects/example/styles/.libsass.json`:

```js
{
	"output_dir": "../static/css", // This path gets normalized, so `..`s get handled
	"options": {
		"line-comments": true,
		"line-numbers":  true,
		"style":         "nested",
		"load-path":     "/path/to/node_modules/bourbon" // Get your fill of bourbon or point to compass!
	}
}
```

## Usage

Configuration is entirely optional, so in the simplest case, simply build with `Ctrl+Shift+B`. If you have another SASS build system installed, you may need to select libsass in Sublime's menu.

## Troubleshooting

### Libsass isn't automatically chosen as the build system

If you have another SASS build system installed, it may be conflicting. If no build system is selected, then you may not have a SASS syntax definition installed or it may be oddly defined (i.e. not using source.sass or source.scss). Try replacing it with another syntax highlighting package such as [Sass](https://github.com/nathos/sass-textmate-bundle).

### My file compiled, but I don't know where it went!

Libsass looks up the directory structure until it finds (or doesn't find) a `.libsass.json` config file. If you're not using a config file, then check that there isn't a config in any parent directory. E.g. if you have your project in `/home/ben/projects/app` and an extraneous config file `/home/ben/projects/.libsass.json`, the css will output relative to `/home/ben/projects` instead of to `/home/ben/projects/app/build/css`.
