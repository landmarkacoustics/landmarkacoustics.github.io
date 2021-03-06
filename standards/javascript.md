---
title: Javascript Code Standards
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Javascript Code Standards

## File Organization

We will use [ECMAScript 6](http://www.ecma-international.org/ecma-262/6.0/) to
organize our files as modules. Each class will be in its own file, or module.
The application will have a root file that is called in a `script` element in
HTML, and that file will then `import` all of the other modules.

### File Hierarchy

There is no strict file hierarchy so far in how we organize our modules/classes
into groups. It seems like grouping files by associated purposes may make the
most sense.

### Modules

In general, there will be one class per file. Javascript calls individual files
"modules," or at least it has since ECMAScript 6. All of the file's contents
are private, except for things that are explicitly marked with `export`. There
are a lot of ways to use the `export` keyword, but we are going to do just one.
The method that we will use is motivated by JSDoc documentation and the Emacs
FlyC linter.

If the module only exports one thing, then place an `export default` statement
immediately following the class or function definition. If there are multiple
exports, then use an `export { ... }` statement.

If something has been exported with `export default` then you can import it in
another module with `import ... from 'path/to/module_file.js';`. If there
were multiple exports then you need to use `import {...} from etc...`.

## Documentation

We will to use [JSDoc](https://jsdoc.app) as my comments-to-documentation tool.
The command-line argument is
```{console}
jsdoc input_directory -r -d output_directory --verbose -c ~/.conf.jsdoc.json
```
It uses a syntax similar to, but not identical to, Python markdown, but it's
close enough that I don't mind.

```{javascript}
/**
 * @file descriptions for every file!
 * @copyright (C) YEAR by Landmark Acoustics LLC
 */

/** Class descriptions go before the declaration
 *
 * @extends superclass
 *
 * @property {type} name -
 *  data member descriptions can go here.
 */
class YourClass extends Superclass {

    /**
     * @param {type} parameter_name -
     *  describe the 1st argument to the constructor
     * @param {type} another_parameter_name -
     *  describe the 2nd argument
     */
     constructor (parameter_name, another_parameter_name) {
         ...
		 /** Optionally describe a property here
		  * @member {type} member_name -
		  */
		  this.member_name = member_value;
	     ...
     }
     
     /** Describe the method
      *
      * @param {type} argument -
      *  describe the argument.
      *
      * @returns {type} Describe the return value
      */
      method (argument) {
          ...
      }
}
```
