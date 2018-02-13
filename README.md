# gulp_sass_bs4_training
# Gulp SASS Bootstrap 4 Workflow

1.	Node.js has to be installed => see Node.js Job aid
2.	Gulp-cli has to be installed => from terminal type: npm install -g gulp-cli
3.	Create directory for the new project.  (mkdir)
4.	Change to that directory. (cd)
5.	Type: npm init to create an NPM JSON file. This creates a configuration for the project.  This will track dependencies for the project.  If bootstrap 4 is added, then bootstrap 4 will be downloaded and installed if this project was needed on another environment.
6.	Type: npm install bootstrap -save
7.	Open project folder in IDE of choice.  Using Brackets as of Feb 12, 2018.
8.	Create src folder in the project folder's root directory.
9.	Create index.html file in new src folder.  Copy bootstrap starter template from getbootrap.com and paste it inside the new html file.
10.	Add style.scss file under the scss folder and add $bg-color: blue; body {background: $bg-color; } and save.  This will allow us to check later that sass compiler is correctly linked.
11.	Gulp is a JavaScript task runner which includes sass compiling and browser reloading.  Use npm to install gulp.  Type: npm install gulp browser-sync gulp-sass -save-dev. This adds a browser sync and sass utility to gulp and saves it as a developer dependency.
12.	Upgrade any files listed during the gulp install.  I had to add jquery@^3 and popper.js@^1.12.9.
13.	Create gulpfile.js in root directory of project and add:
		var gulp		= require('gulp');
		var browserSync	= require('browser-sync').create();
		var sass		= require('gulp-sass');

		// Compile sass into CSS & auto-inject into browsers
		gulp.task('sass', function() {
			return gulp.src(['node_modules/bootstrap/scss/bootstrap.scss', 'src/scss/*.scss'])
				.pipe(sass())
				.pipe(gulp.dest("src/css"))
				.pipe(browserSync.stream());
		});

		// Move the javascript files inot our /src/js folder
		gulp.task('js', function() {
			return gulp.src(['node_modules/jquery/dist/jquery.min.js',   'node_modules/popper.js/dist/umd/popper.min.js', 'node_modules/bootstrap/dist/js/bootstrap.min.js'])
				.pipe(gulp.dest("src/js"))
				.pipe(browserSync.stream());
		});

		// Static Server + watching scss/html files
		gulp.task('serve', ['sass'], function() {

			browserSync.init({
				server: "./src"
			});

			gulp.watch(['node_modules/bootstrap/scss/bootstrap.scss', 'src/scss/*.scss'], ['sass']);
			gulp.watch("src/*.html").on('change', browserSync.reload);
		});

		gulp.task('default', ['js', 'serve']);

14.  