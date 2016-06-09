# Grunt - The JavaScript Task Runner

## Getting started

### Installing the CLI
```bash
$ npm install -g grunt-cli
```
### package.json
* Most grunt-init templates will automatically create a project-specific package.json file.
* The npm init command will create a basic package.json file.

### Installing Plug-ins
Not only will this install <module> locally, but it will automatically be added to the devDependencies section, using a tilde version range.
```bash
$ npm install <module> --save-dev
```
### Autoloading grunt tasks using globbing patterns
```bash
$ npm install load-grunt-tasks --save-dev
```
```javascript
// Gruntfile.js
module.exports = grunt => {
    // load all grunt tasks matching the ['grunt-*', '@*/grunt-*'] patterns
    require('load-grunt-tasks')(grunt);
    // ...
};
```
### The Gruntfile
A Gruntfile is comprised of the following parts:

* The "wrapper" function
* Project and task configuration
* Loading Grunt plugins and tasks
* Custom tasks

In the following Gruntfile, project metadata is imported into the Grunt config from the project's package.json file and the grunt-contrib-uglify plugin's uglify task is configured to minify a source file and generate a banner comment dynamically using that metadata. When grunt is run on the command line, the uglify task will be run by default.
```javascript
// Gruntfile.js
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
```
## Running Grunt

### Ways to run Grunt
```javascript
// Gruntfile.js
module.exports = function(grunt) {
    // load all grunt tasks matching the ['grunt-*', '@*/grunt-*'] patterns
    require('load-grunt-tasks')(grunt);

    // A very basic default task.
    grunt.registerTask('default', 'Log some stuff.', function() {
        grunt.log.write('Logging some stuff...').ok();
    });
};
```
```bash
$ grunt
Running "default" task
Logging some stuff...OK

Done.
```
```bash
$ grunt <task>
$ grunt watch
```
### Initializing a Project
Grunt-init is a scaffolding tool used to automate project creation. In order to use grunt-init, you'll want to install it globally.
```bash
$ npm install -g grunt-init
```
Once grunt-init is installed, place this template in your ~/.grunt-init/ directory. It's recommended that you use git to clone this template into that directory, as follows:
```bash
$ git clone https://github.com/gruntjs/grunt-init-gruntfile.git ~/.grunt-init/gruntfile
```
At the command-line, cd into an empty directory, run this command and follow the prompts.
```bash
$ grunt-init gruntfile
```
## Code Compiling

### Using Grunt Connect - A static web server
Install grunt connect plugin in console to your local project
```bash
$ npm install grunt-contrib-connect --save-dev
```
Configure it in Gruntfile
```javascript
grunt.initConfig({
  connect: {
    server: {
      options: {
        port: 9000,
        protocol: "http"
        hostname: "localhost",
        base: ".",
        directory: null,
        open: true,
        keepalive: true
      }
    }
  }
});
```
Run in console
```bash
$ grunt connect
```
### Compile Sass to CSS using Compass
Install grunt compass plugin in console to your local project
```bash
$ npm install grunt-contrib-compass --save-dev
```
Configure it in Gruntfile
```javascript
grunt.initConfig({
  compass: {                  // Task
    dist: {                   // Target
      options: {              // Target options
        sassDir: 'sass',
        cssDir: 'css'
      }
    }
  },
  watch: {
    sass: {
      files: '**/*.scss',
      tasks: ['compass']
    }
  }
});
```
Run in console
```bash
$ grunt compass
```
### Using Targets
```javascript
grunt.initConfig({
  compass: {                  // Task
    dev: {                   // Target
      options: {              // Target options
        sassDir: 'sass',
        cssDir: 'css',
        outputStyle: 'nested'
      }
    },
    prod: {
      options: {
        sassDir: 'sass',
        cssDir: 'css',
        outputStyle: 'compressed',
        noLineComments: true
      }
    }
  }
});
```
Run in console
```bash
$ grunt compass:dev
```
## File Manipulation

### File Moving and Renaming
Install grunt copy plugin in console to your local project
```bash
$ npm install grunt-contrib-copy --save-dev
```
Configure it in Gruntfile
```javascript
grunt.initConfig({
  copy: {
    main: {
      files: [
        {
          expand: true, // this enables some extra options
          cwd: 'process/' // this is relative to root
          src: ['**/*.css'], // the double star means all directories and single means all files
          dest: 'dist/css',
          rename: function(dest, src) {
            return dest + "/" + src.substring(0, src.indexOf('.')) + '.prod.css';
          },
          flatten: true
        }
      ]
    }
  }
});
```
Run in console
```bash
$ grunt copy
```
### CSS/JS Concatenation
Install grunt plugins in console to your local project
```bash
$ npm install grunt-contrib-concat --save-dev
$ npm install grunt-contrib-cssmin --save-dev
```
Configure them in Gruntfile
```javascript
grunt.initConfig({
  concat: {
    prod: {
      src: ['css/**/*.css'],
      dest: 'process/site.css'
    }
  },
  cssmin: {
    prod: {
      files: [{
        expand: true,
        cwd: 'dist/css/',
        src: ['*.css', '!*.min.css'],
        dest: 'dist/css/',
        ext: '.min.css'
      }]
    }
  }
});
```
## Testing

### Syntax Testing with JSLint
Install grunt plugin in console to your local project
```bash
$ npm install grunt-jslint --save-dev
```
Configure them in Gruntfile
```javascript
grunt.initConfig({
  jslint: {
    js: {
      src: [
        'js/*.js'
      ]
    }
  }
});

grunt.registerTask('test', ['jslint']);
```
Run in console
```bash
$ grunt test
```
### Unit Testing with QUnit
Install Qunit and grunt plugin in console to your local project
```bash
$ npm install qunit --save-dev
$ npm install grunt-contrib-qunit --save-dev
```
A minimal QUnit test setup
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>QUnit Example</title>
  <link rel="stylesheet" href="https://code.jquery.com/qunit/qunit-1.23.1.css">
</head>
<body>
  <div id="qunit"></div>
  <div id="qunit-fixture"></div>
  <script src="https://code.jquery.com/qunit/qunit-1.23.1.js"></script>
  <script src="tests.js"></script>
</body>
</html>
```
Configure in Gruntfile
```javascript
grunt.initConfig({
  qunit: {
    all: {
      options: {
        urls: [
          'http://grunt.dev/tests/index.html'
        ]
      }
    }
  }
});
```
Run in console
```bash
$ grunt qunit
```
### Behavior Testing with Behat
The easiest way to install Behat is by using Composer. Look @ http://behat.org/ and https://github.com/Behat/Behat
Install grunt plugin in console to your local project
```bash
$ npm install grunt-behat --save-dev
```
Configure in Gruntfile
```javascript
grunt.initConfig({
  behat: {
    test: {
      options: {
        output: true,
        failOnUndefined: false,
        failOnFailed: true
      },
      cmd: 'vendor/bin/behat',
      features: 'features',
      flags: '-f pretty'
    }
  }
});
```
Run in console
```bash
$ grunt behat
```
## Deployment

### Creating a Deployment Package
We tie all our tasks together + we will create a zip file.
Install grunt compress plugin in console to your local project
```bash
$ npm install grunt-contrib-compress --save-dev
```
Configure in Gruntfile
```javascript
grunt.initConfig({
  compress: {
    main: options: {
      archive: 'site.zip'
    },
    files: [{
        expand: true,
        src: ['dist/*'],
        dest: '/'
    }]
  }
});

grunt.registerTask('build', ['concat', 'cssmin', 'compress']);
grunt.registerTask('dev', ['concat:dev', 'cssmin:dev', 'compress:dev']);
```
### Git integration
Install grunt git plugin in console to your local project
```bash
$ npm install grunt-git --save-dev
$ npm install grunt-githooks --save-dev
```
Configure in Gruntfile
```javascript
grunt.initConfig({
  gitcommit: {
    task: {
      options: {
        message: 'Testing',
        noVerify: true,
        noStatus: false
      },
      files: {
        src: ['.']
      }
    }
  },
  githooks: {
    all: {
      'pre-commit': 'test'
    }
  }
});
```
### Moving Files to Production (using Rsync)
Install grunt rsync plugin in console to your local project
```bash
$ npm install grunt-rsync --save-dev
```
## Running Custom Commands
### Run a Basic CLI Command
Install grunt exec plugin in console to your local project
```bash
$ npm install grunt-exec --save-dev
```
Configure in Gruntfile
```javascript
grunt.initConfig({
  exec: {
    drush: {
      command: 'drush status',
      stdout: true
    },
    drush1: {
      command: function() {
        var commmand;
        command = 'drush status';
        if (this.option('buggy')) {
          command += " -d";
        }
        return command;
      },
      stdout: false
    }
  }
});
```
### Using Prompts
Install grunt prompt and notify plugin in console to your local project
```bash
$ npm install grunt-prompt --save-dev
$ npm install grunt-notify --save-dev
```
