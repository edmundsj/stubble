# Comment Templating Language (CTL)

The comment templating language was designed to be a flexible way to inject generated code into existing codebases.

## Whitespace
Rule: The CTL respects whitespace before the comment sequence. Any indentation should be preserved for generated code. For example:
```
def my_func(value):
    # {{replacement}}
    pass
```
On replaced with a multi-line statement, should become:

```
def my_func(value):
    new_value = value * 2
    derived_value = new_value **3
    pass
```
However, the CTL is not language indentation-aware. It will not take a string of
```
if(something):
do something_else
```
and automatically indent it during injection. The user is responsible for indenting injected code if it requires internal indents.

Rule: The CTL ignores trailing whitespace

Rule: The CTL ignores whitespace inside replacement brackets `{{ }}`
For example, all the following are equivalent to the CTL:
```
{{ replacement}}
{{ replacement }}
{{replacement}}
{{\t\n\treplacement   }}
```

Rule: The CTL removes whitespace between the comment sequence and the start of the replacement line.
For example, the following replacement:
```
# def my_code({{replacement}}):
```
will be replaced with:
```
def my_code(thing_you_replaced):
```

## Whole-line replacements
When a replacement is the only thing on its line, for example:

```
# {{replacement}}
```

This line will be replaced in its entirety with the contents specified by the replacement.


## Partial-line replacements
Rule: When a replacement is embedded inside other text, that text will be preserved, and only the replacement substituted, with the exception of trailing whitespace and whitespace between the comment sequence and the start of the text. For example:
```
// Serial.println({{replacement}});
```
Would be replaced with:
```
Serial.println(thing_to_replace);
```

## Multiple Replacements
Rule: If multiple replacements are present on a single line, those replacements will be treated as partial in-line replacements. For example:

```
# def my_func({{arg1}},{{arg2}}):
```
Will be replaced with:
```
# def my_func(arg1,arg2):
```

Rule: Whole-line replacements do not support multiple replacements per line

