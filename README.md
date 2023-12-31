# Stubble
A native, inline code-generation tool using the comment templating language.
For more information on the comment templating language, see `ctl.md`

## The Problem
One of the most difficult aspects of code generation is integrating generated code into existing codebases. Templating systems typically require full ownership of files or whole directories.

## The solution
Stubble aims to solve this by allowing you to provide comment-based "stubs" in your existing codebase, to allow for 
injection of additional code. Your templates can be written in your native language and will be valid code on their 
own in their native language.

## Example
Let's say you have a python file. You can easily add a stub that `stubble` will find and replace:
```
... before your code
def main():
	do_something()
	do_something_else()
	# {{replace_this}}

```

Or maybe you have a C++ file, and you want to fill in the argument inside a function call. `stubbles` can handle that, too.
```
void myFunction(int arg1) {
	// Serial.println({{SOMETHING_GOES_HERE}});
	Serial.println("hello");
}

```

## Getting Started
The easiest way to install stubble is via pip:

```
pip install stubble
```

### From the command-line
And then it can be invoked as a command-line tool:
```
python3 -m stubble --template my_file.cpp --replacements replacements.json
```

By default, stubble produces code in a `generated` folder in the same location as it is in invoked, but this can be 
overriden using the `--output-dir` option.



### Using as a python package
If you want to use `stubbles` as a python package, you can do that too. If you want to provide input files, you can invoke the `main()` method directly:

```
from stubbles.__main__ import main

main(
	template=your_template, 
	replacements_file=your_replacements_file, 
	output_dir=your_output_dir
)
```

Or, if you already have `replacements` as a dictionary, the template as a string, and you know your target language:
```
from stubbles import populate, Language

your_template = "# {{replacement1}}"
your_replacement_dict = {"replacement1": "value1"}

populated_template = populate(
	template=your_template,
	replacements=your_replacement_dict,
	lang=Language.PYTHON
)

```
`stubbles` also provides the helper functions `read_dict_from_file`, which reads a dict from an arbitrary key-value pair file, and `language`, which takes a filename and figures out, from its extension, what language that file is using.

## Fetaures
- All stubble template files are valid code in their native language
- Simple, lightweight, logicless template replacement
- Allows for inline replacements and whole-line replacements
- Support for individual input files and input directories
- Respects indentation level of comment, auto-indents inserted code.

## Supported replacements file types
Currently, the following replacements file types are supported:

- JSON 
- YAML
- TOML
- XML
- .ini
- CSV

If you want an additional replacements file type supported, please submit an issue, it's very easy to add.

## Supported Languages

- C++
- C
- Python
- Ruby
- TypeScript
- Javascript
- CSS / SASS / SCSS
- Java
- Golang
- Rust

In principle, any language that supports comments can be supported by `stubbles`. If you want to add a language, just submit an issue. It's extremely easy to add.


## TODO:
- Add CI, code coverage
- Add separate comment templating language specification

