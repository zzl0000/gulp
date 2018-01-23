# 前端 gulp 静态页面快速构建模板

# 构建命令 gulp package.json 文件可配置\

# gulp 常用插件


gulp-load-plugins：自动加载
{
	var gulp = require('gulp'),
	plugins= require('gulp-load-plugins')();
	
	plugins.rename = require('gulp-rename');
	plugins.rubySass = require('gulp-ruby-sass');
}

gulp-rename：重命名{

	var gulp = require('gulp'),
    rename = require('gulp-rename'),
    uglify = require("gulp-uglify");

	gulp.task('rename', function () {
	    gulp.src('js/jquery.js')
	    .pipe(uglify())  //压缩
	    .pipe(rename('jquery.min.js')) //会将jquery.js重命名为jquery.min.js
	    .pipe(gulp.dest('js'));
	    //关于gulp-rename的更多强大的用法请参考https://www.npmjs.com/package/gulp-rename
	});
}

gulp-uglify：压缩{

	var gulp = require('gulp'),
    uglify = require("gulp-uglify");

	gulp.task('minify-js', function () {
	    gulp.src('js/*.js') // 要压缩的js文件
	    .pipe(uglify())  //使用uglify进行压缩,更多配置请参考：
	    .pipe(gulp.dest('dist/js')); //压缩后的路径
	});
}


gulp-minify-css：css压缩{

	var gulp = require('gulp'),
    minifyCss = require("gulp-minify-css");

	gulp.task('minify-css', function () {
	    gulp.src('css/*.css') // 要压缩的css文件
	    .pipe(minifyCss()) //压缩css
	    .pipe(gulp.dest('dist/css'));
	});
}


gulp-minify-html：html压缩{

	var gulp = require('gulp'),
    minifyHtml = require("gulp-minify-html");

	gulp.task('minify-html', function () {
	    gulp.src('html/*.html') // 要压缩的html文件
	    .pipe(minifyHtml()) //压缩
	    .pipe(gulp.dest('dist/html'));
	});
}

gulp-jshint：js检查{

	var gulp = require('gulp'),
    jshint = require("gulp-jshint");

	gulp.task('jsLint', function () {
	    gulp.src('js/*.js')
	    .pipe(jshint())
	    .pipe(jshint.reporter()); // 输出检查结果
	});
}

gulp-concat：文件合并{

	var gulp = require('gulp'),
    concat = require("gulp-concat");

	gulp.task('concat', function () {
	    gulp.src('js/*.js')  //要合并的文件
	    .pipe(concat('all.js'))  // 合并匹配到的js文件并命名为 "all.js"
	    .pipe(gulp.dest('dist/js'));
	});
}

gulp-sass,gulp-less  ：less和sass的编译{

	var gulp = require('gulp'),
    less = require("gulp-less");
	gulp.task('compile-less', function () {
	    gulp.src('less/*.less')
	    .pipe(less())
	    .pipe(gulp.dest('dist/css'));
	});

	var gulp = require('gulp'),
    sass = require("gulp-sass");
	gulp.task('compile-sass', function () {
	    gulp.src('sass/*.sass')
	    .pipe(sass())
	    .pipe(gulp.dest('dist/css'));
	});
}

gulp-imagemin ：图片压缩 {

	var gulp = require('gulp');
	var imagemin = require('gulp-imagemin');
	var pngquant = require('imagemin-pngquant'); //png图片压缩插件

	gulp.task('default', function () {
	    return gulp.src('src/images/*')
	        .pipe(imagemin({
	            progressive: true,
	            use: [pngquant()] //使用pngquant来压缩png图片
	        }))
	        .pipe(gulp.dest('dist'));
	});
}

**....
gulp-livereload ：自动刷新 {

	var gulp = require('gulp'),
    less = require('gulp-less'),
    livereload = require('gulp-livereload');

	gulp.task('less', function() {
	  gulp.src('less/*.less')
	    .pipe(less())
	    .pipe(gulp.dest('css'))
	    .pipe(livereload());
	});

	gulp.task('watch', function() {
	  livereload.listen(); //要在这里调用listen()方法
	  gulp.watch('less/*.less', ['less']);
	});
}
***....
 gulp-connect 配置本地预览服务器,

{
	var gulp = require('gulp'),
  	connect = require('gulp-connect');
	gulp.task('myServer', function() {
	  connect.server({
	    root: 'app',
	    port: 8000,
	    livereload: true
	  });
	});
}

gulp-autoprefixer 自动补齐兼容css
{
	//引入插件
	var gulp = require('gulp');
	var autoprefixer = require('gulp-autoprefixer');
	 
	 //默认执行任务
	gulp.task('default', function () {
	    //找到src目录下app.css，为其补全浏览器兼容的css
	    return gulp.src('src/app.css')
	        .pipe(autoprefixer({
	            browsers: ['last 2 versions'],
	            cascade: false
	        }))
	        //输出到dist文件夹
	        .pipe(gulp.dest('dist'));
	});
}

