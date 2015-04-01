# gulp
###1.安装node.js,打开https://nodejs.org/下载安装
###2.熟练使用npm操作，包括install、uninstall等命令，可以使用npm help帮助
###3.选装cnpm,命令提示符执行：**npm install cnpm -g --registry=https://registry.npm.taobao.org**  官方网址：http://npm.taobao.org(因为npm安装插件是从国外服务器下载，受网络影响大，可能出现异常，如果npm的服务器在中国就好了，所以我们乐于分享的淘宝团队干了这事。)
###4.全局安装gulp.使用命令：**cnpm install gulp -g**，安装后使用**gulp -v**查看版本号
###5.新建package.json文件，推荐使用命令来配置，命令提示符执行：**cnpm init**
###6.本地安装gulp插件
6.1 首先本地安装gulp：**cnpm install gulp --save-dev**
6.2 然后安装所需的插件如：**cnpm install gulp-less --save-dev**
6.3 一些常用插件推荐：gulp-minify-css gulp-jshint gulp-concat gulp-uglify gulp-imagemin imagemin-pngquant gulp-notify gulp-rename browser-sync gulp-cache gulp-htmlmin gulp-rev-append等
npm插件下载地址：https://www.npmjs.com/
###7.新建gulpfile.js,如下
```js
var gulp = require('gulp'),
    gulpLoadPlugins = require('gulp-load-plugins'),
    plugins = gulpLoadPlugins();

//压缩js
gulp.task('js',function(){
    return gulp.src('js/*.js')
        .pipe(plugins.jshint())
        .pipe(plugins.jshint.reporter('default'))
        .pipe(plugins.uglify())
        .pipe(plugins.concat('min.js'))
        .pipe(plugins.rename({ suffix: '.min' }))
        .pipe(gulp.dest('build'))
        .pipe(plugins.notify({ message: 'Javascript task complete' }));
});

//压缩图片
gulp.task('image',function(){
    return gulp.src('image/*')
        .pipe(plugins.imagemin({
            optimizationLevel: 3,
            progressive: true
        }))
        .pipe(gulp.dest('build'))
        .pipe(plugins.notify({ message: 'Images task complete' }));
});

//压缩样式
gulp.task('styles', function() {
    return gulp.src('css/*.css')
        .pipe(plugins.rename({ suffix: '.min' }))
        .pipe(plugins.concat('all.css'))
        .pipe(plugins.minifyCss())
        .pipe(gulp.dest('build'))
        .pipe(plugins.notify({ message: 'Styles task complete' }));
});

//html压缩
gulp.task('Htmlmin', function () {
    var options = {
        removeComments: true,//清除HTML注释
        collapseWhitespace: true,//压缩HTML
        collapseBooleanAttributes: true,//省略布尔属性的值 <input checked="true"/> ==> <input />
        removeEmptyAttributes: true,//删除所有空格作属性值 <input id="" /> ==> <input />
        removeScriptTypeAttributes: true,//删除<script>的type="text/javascript"
        removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
        minifyJS: true,//压缩页面JS
        minifyCSS: true//压缩页面CSS
    };
    return gulp.src('index.html')
        .pipe(plugins.htmlmin(options))
        .pipe(gulp.dest('build'))
        .pipe(plugins.notify({ message: 'Html task complete' }));
});

//添加版本信息
gulp.task('rev', function () {
    gulp.src('demo.html')
        .pipe(plugins.revAppend())
        .pipe(gulp.dest(''))
        .pipe(plugins.notify({ message: 'rev task complete' }));;
});


//watch
gulp.task('watch', function(){
    gulp.watch('js/*.js',['js']);
    gulp.watch('image/*',['image']);
    gulp.watch('css/*.css',['styles']);
    gulp.watch('demo.html',['rev']);
    gulp.watch('index.html',['Htmlmin']);
})

//Default Task
gulp.task('default', ['js','image','styles','rev','Htmlmin','watch']);

```

###8.运行gulp，使用命令提示符执行如：**gulp js**
在webstorm中运行，将项目导入webstorm，右键gulpfile.js 选择”Show Gulp Tasks”打开Gulp窗口，若出现”No task found”，选择右键”Reload tasks”，双击运行即可。

[详细配置参考][1]
[插件使用参考][2]


  [1]: http://www.dtao.org/archives/18
  [2]: http://www.w3ctech.com/topic/134
