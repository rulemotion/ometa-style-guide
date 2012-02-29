# OMeta Style Guide

## Inspiration

The details in this guide have been very heavily inspired by the [CoffeeScript Style Guide][coffeescript-style-guide]:

## Table of Contents

* [The OMeta Style Guide](#guide)
	* [Code Layout](#code_layout)
		* [Tabs or Spaces?](#tabs_or_spaces)
		* [Maximum Line Length](#maximum_line_length)
		* [Blank Lines](#blank_lines)
		* [Trailing Whitespace](#trailing_whitespace)
		* [Encoding](#encoding)
	* [Module Definition](#module_definition)
	* [Whitespace in Expressions and Statements](#whitespace)
	* [Comments](#comments)
		* [Block Comments](#block_comments)
				* [Inline Comments](#inline_comments)
	* [Naming Conventions](#naming_conventions)
	* [Rules](#rules)
	* [Strings](#strings)
	* [Extending Native Objects](#extending_native_objects)
	* [Annotations](#annotations)

<a name="code_layout"/>
## Code layout

<a name="tabs_or_spaces"/>
### Tabs or Spaces?

Use **tabs only**. Never mix tabs and spaces.

<a name="maximum_line_length"/>
### Maximum Line Length

Limit all lines to a maximum of 79 characters.

<a name="blank_lines"/>
### Blank Lines

Separate rule groups with a single blank line.

```
	ometa ometaParser {
		ruleGroup1 =
			:ruleBody,
		
		ruleGroup2a =
			:ruleBody,
		ruleGroup2b
			:ruleBody
	}
```

Separate javascript function definitions with a single blank line.

<a name="trailing_whitespace"/>
### Trailing Whitespace

Do not include trailing whitespace on any lines.

<a name="encoding"/>
### Encoding

UTF-8 is the preferred source file encoding.

<a name="module_definition"/>
## Module Definition

Wrap in a define, with a list of dependencies, these dependencies should be grouped in the following order:
1. Referenced dependencies
2. ometa/ometa-base
3. Non-referenced dependencies.

```
define([..., 'ometa/ometa-base', ...], function(...) {


});
```

Within the define declare a var with the name of OMeta object and then return that variable, this is to stop the global namespace from being polluted:

```
define(['ometa/ometa-base'], function() {
	var ometaParser;
	ometa ometaParser {

	}
	return ometaParser;
});
```

<a name="whitespace"/>
## Whitespace in Expressions and Statements

Avoid extraneous whitespace in the following situations:

- Immediately inside parentheses, brackets or braces

	```
		ometa ometaParser {
			ruleWithArgs :arg =
				empty,
			rule1 =
				ruleWithArgs('a'), // Yes
			rule2 =
				ruleWithArgs('a') // No
		}
	```

- Immediately before a comma

	```
		ometa ometaParser {
			ruleWithArgs :arg :arg2 =
				empty,
			rule1 =
				ruleWithArgs('a', 'b'), // Yes
			rule2 =
				ruleWithArgs('a' , 'b') // No
		}
	```

<a name="comments"/>
## Comments

If modifying code that is described by an existing comment, update the comment such that it accurately reflects the new code. (Ideally, improve the code to obviate the need for the comment, and delete the comment entirely.)

The first word of the comment should be capitalized, unless the first word is an identifier that begins with a lower-case letter.

If a comment is short, the period at the end can be omitted.

<a name="block_comments"/>
### Block Comments

Block comments apply to the block of code that follows them.

Each line of a block comment starts with a `*` and a single space, and should be indented at the same level of the code that it describes.

Paragraphs inside of block comments are separated by a line containing a single `*`.

```
	ometa ometaParser {
		/*
		* This is a block comment. Note that if this were a real block
		* comment, we would actually be describing the proceeding code.
		*
		* This is the second paragraph of the same block comment. Note
		* that this paragraph was separated from the previous paragraph
		* by a line containing a single comment character.
		*/
		rule =
			:ruleBody
	}
```

<a name="inline_comments"/>
### Inline Comments

Inline comments are placed on the line immediately above the statement that they are describing. If the inline comment is sufficiently short, it can be placed on the same line as the statement (separated by a single space from the end of the statement).

All inline comments should start with a `//` and a single space.

The use of inline comments should be limited, because their existence is typically a sign of a code smell.

Do not use inline comments when they state the obvious.

<a name="naming_conventions"/>
## Naming Conventions

Use `camelCase` (with a leading lowercase character) to name all variables, methods, and object properties.

Use `CamelCase` (with a leading uppercase character) to name all classes and rules. _(This style is also commonly referred to as `PascalCase`, `CamelCaps`, or `CapWords`, among [other alternatives][camel-case-variations].)_


<a name="rules"/>
## Rules

Within the OMeta block use one line for the rule and its parameters and start the rule body indented one level on the next line:

```
	ometa ometaParser {
		rule :param1 :param2 =
			:ruleBody,
		rule2 =
			:ruleBody,
		rule3
			:ruleBody
	}
```

For cases where you override the default return it should be on a new line:

```
	ometa ometaParser {
		rule :param1 :param2 =
			:ruleBody
			-> 'ReturnValue',

		rule2 =
			(
				:ruleBodyPart
				-> 'ReturnValue'
			)*:ruleBody
		rule3 =
			(	:ruleBodyPart
				-> 'ReturnValue'
			)*:ruleBody

		rule4 =
			(	:ruleBody
			|	:ruleBody2
				-> 'ReturnValue'
			)
			-> 'ReturnValue'
	}
```

Rules with | in them.

```
	ometa ometaParser {
		rule =
			(	:ruleBody
			|	:ruleBody2
				-> 'ReturnValue'
			)
			-> 'ReturnValue'
		rule2 =
			(
				:ruleBody
			|	:ruleBody2
				-> 'ReturnValue'
			)
			-> 'ReturnValue'
		rule3 = // Avoid
				:ruleBody
			|	:ruleBody2
				-> 'ReturnValue'
	}
```

<a name="strings"/>
## Strings

Use only quoted strings (`''` and `""1) instead of the OMeta specific string options such as (```, `#`).

```
'this is a string' // Yes
"this is a string" // Yes
`string // No
#string // No
```

<a name="#extending_native_objects"/>
## Extending Native Objects

Do not modify native objects.

For example, do not modify `Array.prototype` to introduce `Array#forEach`.

<a name="annotations"/>
## Annotations

Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.

Write the annotation on the line immediately above the code that the annotation is describing.

The annotation keyword should be followed by a colon and a space, and a descriptive note.

```
	ometa ometaParser {
		// TODO: Make this rule do something.
		ruleWithArgs :arg =
			empty,
		rule1 =
			ruleWithArgs('a')
	}
```

If multiple lines are required by the description, indent subsequent lines with a tab:

```
	ometa ometaParser {
		// TODO: Make this rule do something
			and go onto the next line.
		ruleWithArgs :arg =
			empty,
		rule1 =
			ruleWithArgs('a')
	}
```

Annotation types:

- `TODO`: describe missing functionality that should be added at a later date
- `FIXME`: describe broken code that must be fixed
- `OPTIMIZE`: describe code that is inefficient and may become a bottleneck
- `HACK`: describe the use of a questionable (or ingenious) coding practice
- `REVIEW`: describe code that should be reviewed to confirm implementation

If a custom annotation is required, the annotation should be documented in the project's README.


[coffeescript-style-guide]: https://github.com/polarmobile/coffeescript-style-guide