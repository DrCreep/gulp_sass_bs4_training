# gulp_sass_bs4_training
# Gulp SASS Bootstrap 4 Workflow

1.	Node.js has to be installed => see Node.js Job aid
2.	Gulp-cli has to be installed => from terminal type: npm install -g gulp-cli
3.	Create directory for the new project.  (mkdir)
4.	Change to that directory. (cd)
5.    At this point, I use GitHub Desktop to statt capturing versions of the project updates.  This step is optional.
6.	In the terminal type without the quotes: "npm init" to create an NPM JSON file. This creates a configuration for the project.  This will track dependencies for the project, e.g. if bootstrap 4 is added, then bootstrap 4 will be downloaded and installed if this project was needed on another environment.
7.	Install Bootstrap 4 and add it to dependency list by typing: npm install bootstrap@4.0.0 -save
8.	Open project folder in IDE of choice.  I am using Brackets on a Macbook Pro as of Feb 20, 2018.
9.	Create src folder in the project folder's root directory.
10.	Create index.html file in new src folder.  Copy bootstrap starter template from getbootstrap.com and paste it inside the new html file.
11.	Add style.scss file under the scss folder that was created after installing bootstrap and add the code $bg-color: blue; body {background: $bg-color; } and save to the style.scss file.  This will allow us to check later that sass compiler is correctly linked if we see a screen with an all blue background.
12.	Gulp is a JavaScript task runner which includes sass compiling and browser reloading.  Use npm to install gulp.  Type: npm install gulp browser-sync gulp-sass -save-dev. This adds a browser sync and sass utility to gulp and saves it as a developer dependency.
13.	Upgrade any files listed during the gulp install.  I had to add jquery@^3 and popper.js@^1.12.9.
14.	Create gulpfile.js in root directory of project and add:

		var gulp		  = require('gulp');
		var browserSync  = require('browser-sync').create();
		var sass		  = require('gulp-sass');

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

15.   Once the gulp file is created, you can change to the project folder using Terminal and type gulp.  If properly installed, a list of functions will be initiated and a browser window will open with a local server started on port 3000.  The browser will show a blue screen to prove the scss file is linked.

