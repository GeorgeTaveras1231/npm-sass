# npm sass

Import installed node modules in sass with no extra configuration.

Because of the way npm installs modules in a nested tree structure, there is no reliable way to ensure the location of locally installed dependencies. In particular - modules are not necessarily installed into ./node_modules. This causes trouble when trying to use npm dependencies in sass files where `--includePath ./node_modules` can fail unexpectedly if a dependency is shared with an ancestor (and so installed further up the tree).

This module will automatically allow access to all locally installed npm modules, irrespective of their install location.

## Install

```
npm install npm-sass
```

## Usage

From the command line:

```
npm-sass ./assets/styles/app.scss > ./public/css/output.css
```

Programmatically:

```javascript
require('npm-sass')('./assets/sass/app.scss', function (err, result) {
    ...
});
```

In gulp:

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
  gulp.src(['./src/*.scss'])
    .pipe(sass({
      importer: require('npm-sass').importer
    }))
    .pipe(gulp.dest('./src/generated/css'));
});
```

## Imports

Imports on npm modules are defined in the usual way in your sass files.

If an imported module contains a `"style"` declaration in its package.json, then this file is imported

Example:

```sass
@import "bootstrap";
```

This will import `./node_modules/bootstrap/dist/css/bootstrap.css` since this is [defined in bootstrap's package.json](https://github.com/twbs/bootstrap/blob/master/package.json#L20)

If no `"style"` declaration exists in the package.json of the imported module, then sass will attempt to load `./styles.scss` from the root of the module.

Alternatively, a specific file can be specified from the module.

Example:

```sass
@import "font-awesome/scss/font-awesome";
```

