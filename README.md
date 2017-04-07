## 使用

gulp-demo目录结构

![](http://on9plnnvl.bkt.clouddn.com/17-4-6/27567647-file_1491466379649_3159.png)


使用：

```
cnpm install
gulp serve
```

因为每次环境都是一样的，直接把要项目目录进行修改使用即可。

## demo完成过程：

1. 命令行创建npm的配置文件
  
    npm init

2. 在项目文件夹下面添加一个gulp的依赖

    npm install gulp --save-dev
  
3. 在项目根目录下添加一个gulpfile.js文件，这个是gulp的主文件，这个文件名是固定的

4. 在gulpfile中抽象我们需要做的任务



` cnpm install gulp-less gulp-concat gulp-uglify gulp-cssnano gulp-htmlmin --save-dev`

`cnpm install browser-sync --save-dev`


```
'use strict';
/**
 * 1. LESS编译 压缩 合并
 * 2. JS合并 压缩 混淆
 * 3. img复制
 * 4. html压缩
 */

// 在gulpfile中先载入gulp包，因为这个包提供了一些API
var gulp = require('gulp');
var less = require('gulp-less');
var cssnano = require('gulp-cssnano');

// 1. LESS编译 压缩 --合并没有必要，一般预处理CSS都可以导包
gulp.task('style', function() {
  // 这里是在执行style任务时自动执行的
  gulp.src(['src/styles/*.less', '!src/styles/_*.less'])
    .pipe(less())
    .pipe(cssnano())
    .pipe(gulp.dest('dist/styles'))
    .pipe(browserSync.reload({
      stream: true
    }));
});

var concat = require('gulp-concat');
var uglify = require('gulp-uglify');

// 2. JS合并 压缩混淆
gulp.task('script', function() {
  gulp.src('src/scripts/*.js')
    .pipe(concat('all.js'))
    .pipe(uglify())
    .pipe(gulp.dest('dist/scripts'))
    .pipe(browserSync.reload({
      stream: true
    }));
});

// 3. 图片复制
gulp.task('image', function() {
  gulp.src('src/images/*.*')
    .pipe(gulp.dest('dist/images'))
    .pipe(browserSync.reload({
      stream: true
    }));
});

var htmlmin = require('gulp-htmlmin');
// 4. HTML
gulp.task('html', function() {
  gulp.src('src/*.html')
    .pipe(htmlmin({
      collapseWhitespace: true,
      removeComments: true
    }))
    .pipe(gulp.dest('dist'))
    .pipe(browserSync.reload({
      stream: true
    }));
});

var browserSync = require('browser-sync');
gulp.task('serve', function() {
  browserSync({
    server: {
      baseDir: ['dist']
    },
  }, function(err, bs) {
    console.log(bs.options.getIn(["urls", "local"]));
  });

  gulp.watch('src/styles/*.less',['style']);
  gulp.watch('src/scripts/*.js',['script']);
  gulp.watch('src/images/*.*',['image']);
  gulp.watch('src/*.html',['html']);
});

```


browsersync

http://www.browsersync.cn/#install

