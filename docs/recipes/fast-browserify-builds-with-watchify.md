# Fast browserify builds with watchify

As a [browserify](http://github.com/substack/node-browserify) project begins to
expand, the time to bundle it slowly gets longer and longer. While it might
start at 1 second, it's possible to be waiting 30 seconds for your project to
build on particularly large projects.

That's why [substack](http://github/substack) wrote
[watchify](http://github.com/substack/watchify), a persistent browserify
bundler that watches files for changes and *only rebuilds what it needs to*.
This way, that first build might still take 30 seconds, but subsequent builds
can still run in under 100ms – which is a huge improvement.

Watchify doesn't have a gulp plugin, but it doesn't need one either: you can
use [vinyl-source-stream](http://github.com/hughsk/vinyl-source-stream) to pipe
the bundle stream into your gulp pipeline.

``` javascript
var source = require('vinyl-source-stream')
var watchify = require('watchify')
var gulp = require('gulp')

var bundler = watchify('./src/index.js')

gulp.task('browserify', rebundle)
gulp.task('watch', ['browserify'], function() {
  bundler.on('update', rebundle)
})

function rebundle() {
  return bundler.bundle()
    .pipe(source('bundle.js'))
    .pipe(gulp.dest('./dist'))
}

// Optionally, you can apply transforms
// and other configuration options on the
// bundler just as you would with browserify
bundler.transform('brfs')
```
