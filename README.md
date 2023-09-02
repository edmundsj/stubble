# Stubble
A native, inline code-generation tool

## The Problem
One of the most difficult aspects of code generation is integrating generated code into existing codebases. Templating systems typically require full ownership of files or whole directories.

## The solution
Stubble aims to solve this by allowing you to provide comment-based "stubs" in your existing codebase, to allow for injection of additional code. Your templates can be written in your native language and will be valid code on their own in their native language.

## Example
Let's say you have a python file. You can easily add a stub that `stubble` will find and replace:
```
... before your code
def main():
	do_something()
	do_something_else()
	# {{replace_this}}

```

## Getting Started
The easiest way to install stubble is via pip:

```
pip install stubble
```

### From the command-line
And then it can be invoked as a command-line tool:
```
python3 -m stubble --template my_file.cpp --json replacements.json
```

By default, stubble produces code in a `generated` folder in the same location as it is in invoked.

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

Or, if you would rather

## TODO:
- Support input directories rather than just individual files
- Complete command-line interface
