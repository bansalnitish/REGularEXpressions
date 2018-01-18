

![Regular Expressions](https://i.imgur.com/KfyCQBI.png)


## Contents

- [1. Introduction](#1-introduction)
- [2. Character Matching](#2-character-matching)
- [3. Meta Characters](#3-meta-characters)
- [4. Special Quantifiers](#4-special-quantifiers)
- [5. Shorthand Character Set](#5-shorthand-character-set)
- [6. Good Examples](#6---good-examples)
- [7. Command Line Usage](#7---command-line-usage)
- [8. Look Around](#8-lookaround)
- [9. Flags](#9-flags)
- [10. Substitutions](#10-substitutions)
- [11. When not to use Regex](#11-when-not-to-use-regex)
- [12. Cheat Sheets](#12-cheat-sheets-for-regular-expressions)


## 1. Introduction

REGEX mean REGular EXpression which in turn is nothing but sequence of characters. These expressions say for example [0–9] means that the expression should contain numbers.
Regular expressions are used in many situation in computer programming. Majorly in search, pattern matching, parsing, filtering of results and so on.

In lay man words, its a kind of rule which programmer tells to the computer to understand.
You may see this in website forms, where you are only forced to enter ONLY numbers or ONLY characters or MINIMUM 8 characters and so on, these are controlled by REGEX behind the screen.Imagine you are writing an application and you want to set the rules for when a
user chooses their username. We want to allow the username to contain letters, numbers, underscores and hyphens. We also want to limit the number of characters in username so it does not look ugly. We use the following regular expression to validate a username:
<br/><br/>
<p align="center">
  <img src="https://i.imgur.com/CNmKXTy.png" alt="Regular expression">
</p>


Above regular expression would accept `bansalnitish` but not `bansalnitish020297` (too long). 


![User_name](https://i.imgur.com/t7TO3QG.png)


![User name](https://i.imgur.com/TquUV4C.png)


A good thing about regex is that it is almost language independent. Implementation might vary but the expression itself is more or less the same. See below  each script will read the `test.txt` file, search it using our regular expression:`^[0-9]+$`, and print all the numbers in file to the console. As of now 
consider just assume `[0-9]` means any digit from 0-9, `^` means start of string, `+` means anything greater than 0 and `$` means end of the string.

If we use the following file as test.txt:
```
1234
abcde
12db2
5362

1
```
The output would be: `('1234', '5362', '1') ` in all the following cases - 


## Javascript / Node.js / Typescript
```
const fs = require('fs')
const testFile = fs.readFileSync('test.txt', 'utf8')
const regex = /^([0-9]+)$/gm
let results = testFile.match(regex)
console.log(results)
```

## Python

```
import re
with open('test.txt', 'r') as f:
  test_string = f.read()
  regex = re.compile(r'^([0-9]+)$', re.MULTILINE)
  result = regex.findall(test_string)
  print(result)
```

## PHP
```
<?php
$myfile = fopen("test.txt", "r") or die("Unable to open file.");
$test_str = fread($myfile,filesize("test.txt"));
fclose($myfile);
$re = '/^[0-9]+$/m';
preg_match_all($re, $test_str, $matches, PREG_SET_ORDER, 0);
var_dump($matches);
?>
```

## Java
```
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;

class FileRegexExample {
  public static void main(String[] args) {
    try {
      String content = new String(Files.readAllBytes(Paths.get("test.txt")));
      Pattern pattern = Pattern.compile("^[0-9]+$", Pattern.MULTILINE);
      Matcher matcher = pattern.matcher(content);
      ArrayList<String> matchList = new ArrayList<String>();

      while (matcher.find()) {
        matchList.add(matcher.group());
      }

      System.out.println(matchList);
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
## C++
```
#include <string>
#include <fstream>
#include <iostream>
#include <sstream>
#include <regex>
using namespace std;

int main () {
  ifstream t("test.txt");
  stringstream buffer;
  buffer << t.rdbuf();
  string testString = buffer.str();

  regex numberLineRegex("(^|\n)([0-9]+)($|\n)");
  sregex_iterator it(testString.begin(), testString.end(), numberLineRegex);
  sregex_iterator it_end;

  while(it != it_end) {
    cout << it -> str();
    ++it;
  }
}
```

## Why Regex?

Regular expressions (regex or regexp) are extremely useful in extracting information from any text by searching for one or more matches of a specific search pattern (i.e. a specific sequence of ASCII or unicode characters).
Fields of application range from validation to parsing/replacing strings, passing through converting data to other formats and web scraping.
One of the most interesting features is that once you’ve learned the syntax, you can actually use this tool in (almost) all programming languages (JavaScript, Java, VB, C #, C / C++, Python, Perl, Ruby, Delphi, R, Tcl, an many others) with the slightest distinctions about the support of the most advanced features and syntax versions supported by the engines).


## 2. Character matching

A regex can be used for simple searching and matching of a sequence of characters.

- For example, the regular expression `hat` means: the letter `h`, followed by the letter `a`, followed by the letter `t`.
`hat` => She wore a pretty blue `hat`. Her friend wore a red `hat`. All `hat`s are cool.

In this way, you can find a match for any sequence of characters you want to. The characters will be searched throughtout the input provided by you. Note that regex is case sensitive.

## 3. Meta Characters

Meta characters are the building blocks of the regular expressions.  Meta characters do not stand for themselves but instead are interpreted in some
special way. Some meta characters have a special meaning and are written inside square brackets. The meta characters are as follows:

|Meta character|Description|
|:----:|----|
|.|Period matches any single character except a line break.|
|[ ]|Character class. Matches any character contained between the square brackets.|
|[^ ]|Negated character class. Matches any character that is not contained between the square brackets|
|*|Matches 0 or more repetitions of the preceding symbol.|
|+|Matches 1 or more repetitions of the preceding symbol.|
|?|Makes the preceding symbol optional.|
|{n,m}|Braces. Matches at least "n" but not more than "m" repetitions of the preceding symbol.|
|(xyz)|Character group. Matches the characters xyz in that exact order.|
|&#124;|Alternation. Matches either the characters before or the characters after the symbol.|
|&#92;|Escapes the next character. This allows you to match reserved characters <code>[ ] ( ) { } . * + ? ^ $ \ &#124;</code>|
|^|Matches the beginning of the input.|
|$|Matches the end of the input.|

## 3.1 Full stop

Full stop `.` is the simplest example of meta character. The meta character `.` matches any single character. It will not match return or newline characters.
For example, the regular expression `.ar` means: any character, followed by the letter `a`, followed by the letter `r`.

<pre>
".ar" => The <strong>car</strong> <strong>par</strong>ked in the <strong>gar</strong>age.
</pre>


## 3.2 Character set

Character sets are also called character class. Square brackets are used to specify character sets. Use a hyphen inside a character set to specify the
characters' range. The order of the character range inside square brackets doesn't matter. For example, the regular expression `[Tt]he` means: an uppercase
`T` or lowercase `t`, followed by the letter `h`, followed by the letter `e`.

<pre>
"[Tt]he" => <strong>The</strong> car parked in <strong>the</strong> garage.
</pre>


A period inside a character set, however, means a literal period. The regular expression `ar[.]` means: a lowercase character `a`, followed by letter `r`,
followed by a period `.` character.

<pre>
"ar[.]" => A garage is a good place to park a c<strong>ar.</strong>
</pre>


### 3.2.1 Negated character set

In general, the caret symbol represents the start of the string, but when it is typed after the opening square bracket it negates the character set. For
example, the regular expression `[^c]ar` means: any character except `c`, followed by the character `a`, followed by the letter `r`.

<pre>
"[^c]ar" => The car <strong>par</strong>ked in the <strong>gar</strong>age.
</pre>


## 3.3 Repetitions

Following meta characters `+`, `*` or `?` are used to specify how many times a subpattern can occur. These meta characters act differently in different
situations.

### 3.3.1 The Star

The symbol `*` matches zero or more repetitions of the preceding matcher. The regular expression `a*` means: zero or more repetitions of preceding lowercase
character `a`. But if it appears after a character set or class then it finds the repetitions of the whole character set. For example, the regular expression
`[a-z]*` means: any number of lowercase letters in a row.

<pre>
"[a-z]*" => T<strong>he</strong> <strong>car</strong> <strong>parked</strong> <strong>in</strong> <strong>the</strong> <strong>garage</strong> #21.
</pre>


The `*` symbol can be used with the meta character `.` to match any string of characters `.*`. The `*` symbol can be used with the whitespace character `\s`
to match a string of whitespace characters. For example, the expression `\s*cat\s*` means: zero or more spaces, followed by lowercase character `c`,
followed by lowercase character `a`, followed by lowercase character `t`, followed by zero or more spaces.

<pre>
"\s*cat\s*" => The fat<strong> cat </strong>sat on the con<strong>cat</strong>enation.
</pre>


### 3.3.2 The Plus

The symbol `+` matches one or more repetitions of the preceding character. For example, the regular expression `c.+t` means: lowercase letter `c`, followed by at least one character, followed by the lowercase character `t`. It needs to be clarified that `t` is the last `t` in the sentence.

<pre>
"c.+t" => The fat <strong>cat sat on the mat</strong>.
</pre>


### 3.3.3 The Question Mark

In regular expression the meta character `?` makes the preceding character optional. This symbol matches zero or one instance of the preceding character. For example, the regular expression `[T]?he` means: Optional the uppercase letter `T`, followed by the lowercase character `h`, followed by the lowercase
character `e`.

<pre>
"[T]he" => <strong>The</strong> car is parked in the garage.
</pre>

<pre>
"[T]?he" => <strong>The</strong> car is parked in t<strong>he</strong> garage.
</pre>


## 3.4 Braces

In regular expression braces that are also called quantifiers are used to specify the number of times that a character or a group of characters can be
repeated. For example, the regular expression `[0-9]{2,3}` means: Match at least 2 digits but not more than 3 ( characters in the range of 0 to 9).

<pre>
"[0-9]{2,3}" => The number was 9.<strong>999</strong>7 but we rounded it off to <strong>10</strong>.0.
</pre>


We can leave out the second number. For example, the regular expression `[0-9]{2,}` means: Match 2 or more digits. If we also remove the comma the regular expression `[0-9]{3}` means: Match exactly 3 digits.

<pre>
"[0-9]{2,}" => The number was 9.<strong>9997</strong> but we rounded it off to <strong>10</strong>.0.
</pre>


<pre>
"[0-9]{3}" => The number was 9.<strong>999</strong>7 but we rounded it off to 10.0.
</pre>


## 3.5 Character Group

Character group is a group of sub-patterns that is written inside Parentheses `(...)`. As we discussed before that in regular expression if we put a quantifier after a character then it will repeat the preceding character. But if we put quantifier after a character group then it repeats the whole character group. For example, the regular expression `(ab)*` matches zero or more repetitions of the character "ab". We can also use the alternation `|` meta character inside character group. For example, the regular expression `(c|g|p)ar` means: lowercase character `c`, `g` or `p`, followed by character `a`, followed by character `r`.

<pre>
"(c|g|p)ar" => The <strong>car</strong> is <strong>par</strong>ked in the <strong>gar</strong>age.
</pre>


## 3.6 Alternation

In regular expression Vertical bar `|` is used to define alternation.Alternation is like a condition between multiple expressions. Now, you may be thinking that character set and alternation works the same way. But the big difference between character set and alternation is that character set works on character level but alternation works on expression level. For example, the regular expression `(T|t)he|car` means: uppercase character `T` or lowercase `t`, followed by lowercase character `h`, followed by lowercase character `e` or lowercase character `c`, followed by lowercase character `a`, followed by lowercase character `r`.

<pre>
"(T|t)he|car" => <strong>The</strong> <strong>car</strong> is parked in <strong>the</strong> garage.
</pre>

## 3.7 Escaping special character

Backslash `\` is used in regular expression to escape the next character. This allows us to specify a symbol as a matching character including reserved
characters `{ } [ ] / \ + * . $ ^ | ?`. To use a special character as a matching character prepend `\` before it.

For example, the regular expression `.` is used to match any character except newline. Now to match `.` in an input string the regular expression
`(f|c|m)at\.?` means: lowercase letter `f`, `c` or `m`, followed by lowercase character `a`, followed by lowercase letter `t`, followed by optional `.`
character.

<pre>
"(f|c|m)at\.?" => The <strong>fat</strong> <strong>cat</strong> sat on the <strong>mat.</strong>
</pre>


## 3.8 Anchors

In regular expressions, we use anchors to check if the matching symbol is the starting symbol or ending symbol of the input string. Anchors are of two types:
First type is Caret `^` that check if the matching character is the start character of the input and the second type is Dollar `$` that checks if matching character is the last character of the input string.

### 3.8.1 Caret

Caret `^` symbol is used to check if matching character is the first character of the input string. If we apply the following regular expression `^a` (if a is the starting symbol) to input string `abc` it matches `a`. But if we apply regular expression `^b` on above input string it does not match anything.Because in input string `abc` "b" is not the starting symbol. Let's take a look at another regular expression `^(T|t)he` which means: uppercase character `T` or lowercase character `t` is the start symbol of the input string, followed by lowercase character `h`, followed by lowercase character `e`.

<pre>
"(T|t)he" => <strong>The</strong> car is parked in <strong>the</strong> garage.
</pre>

<pre>
"^(T|t)he" => <strong>The</strong> car is parked in the garage.
</pre>


### 3.8.2 Dollar

Dollar `$` symbol is used to check if matching character is the last character of the input string. For example, regular expression `(at\.)$` means: a
lowercase character `a`, followed by lowercase character `t`, followed by a `.` character and the matcher must be end of the string.

<pre>
"(at\.)" => The fat c<strong>at.</strong> s<strong>at.</strong> on the m<strong>at.</strong>
</pre>


<pre>
"(at\.)$" => The fat cat. sat. on the m<strong>at.</strong>
</pre>


## 4. Special quantifiers

- **Greedy Quantifier**: matches as many characters as possible. `a.*a` => greedy c`an be dangerous a`t times 
- **Lazy Quantifier** : matches as few characters as possible. `a*?` => `a` p`a`rt of `a`n `a`dventurous trip. 
- **Possessive Quantifier** : matches as many characters as possible; backtracking can't reduce the number of characters matched. Since it is greedy, it will match all the way to the last digit, leaving nothing else for the `.` to match. Without backtracking, this regex fails to produce a match.

## 5. Shorthand Character Set 

Regular expression provides shorthands for the commonly used character sets, which offer convenient shorthands for commonly used regular expressions. The
shorthand character sets are as follows:

|Shorthand|Description|
|:----:|----|
|.|Any character except new line|
|\w|Matches alphanumeric characters: `[a-zA-Z0-9_]`|
|\W|Matches non-alphanumeric characters: `[^\w]`|
|\d|Matches digit: `[0-9]`|
|\D|Matches non-digit: `[^\d]`|
|\s|Matches whitespace character: `[\t\n\f\r\p{Z}]`|
|\S|Matches non-whitespace character: `[^\s]`|

### 5.1 A bit more about `\b`

`\b` may be a bit confusing to some of you so I tried to expand this a bit more. `\b` is an anchor like the caret and the dollar sign. It matches at a position that is called a "word boundary". This match is zero-length.

There are three different positions that qualify as word boundaries:

- Before the first character in the string, if the first character is a word character.
- After the last character in the string, if the last character is a word character.
- Between two characters in the string, where one is a word character and the other is not a word character.

> A word character is all alphanumeric alphabets `[a-zA-Z0-9]` and underscore `_`. Any character not a word character is non word or not a word character.

A few examples will may you much clear about how `\b` works.
The regex `\bcat\b` would match cat in a black cat, but it wouldn't match it in catatonic, tomcat or certificate. Removing one of the boundaries, `\bcat` would match cat in catfish, and cat\b would match cat in tomcat, but not vice-versa. Both, of course, would match cat on its own. The `\bcat\b` will not match cat in _cat or in cat25 because there is no boundary between an underscore and a letter, nor between a letter and a digit: these all belong to what regex defines as word characters (as defined above).

<pre>
"\bm" => <strong>m</strong>oon
</pre>

`oo\b` does not match the `oo` in "moon", because `oo` is followed by `n` which is a word character.

`\B` matches all positions where \b doesn't match. Therefore, it matches:

- When neither side is a word character, for instance at any position in the string $=(@-%++) (including the beginning and end of the string)
- When both sides are a word character, for instance between the `H` and the `i` in `Hi!`.

## 6 - Good Examples

I feel examples explain the content in the best way possible, clearing all your doubts. Here we have two examples they will make you confident that as of now you can write and play with regex. 

### 6.1 - URL Matching
Another highly useful regex recipe is matching URLs in text.

Here an example URL matching expression from [Stack Overflow](https://stackoverflow.com/questions/3809401/what-is-a-good-regular-expression-to-match-a-url).

```text
(https?:\/\/)(www\.)?(?<domain>[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6})(?<path>\/[-a-zA-Z0-9@:%_\/+.~#?&=]*)?
```

- `(https?:\/\/)` - Match http or https followed by \\
- `(www\.)?` - Optional "www" prefix
- `(?<domain>[-a-zA-Z0-9@:%._\+~#=]{2,256}` - Match a valid domain name
- `\.[a-z]{2,6})` - Match a domain extension extension (i.e. ".com" or ".org")
- `(?<path>\/[-a-zA-Z0-9@:%_\/+.~#?&=]*)?` - Match URL path (`/posts`), query string (`?limit=1`), and/or file extension (`.html`), all optional.

#### 6.1.1 - Named capture groups

You'll notice here that some of the capture groups now begin with a `?<name>` identifier.  This is the syntax for a *named capture group*, which makes the data extraction cleaner.

#### 6.1.2 - Real-World Example - Parse Domain Names From URLs on A Web Page

Here's how we could use named capture groups to extract the domain name of each URL in a web page using Python.

```python
import re
import urllib.request

html = str(urllib.request.urlopen("https://moz.com/top500").read())
regex = r"(https?:\/\/)(www\.)?(?P<domain>[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6})(?P<path>\/[-a-zA-Z0-9@:%_\/+.~#?&=]*)?"
matches = re.finditer(regex, html)

for match in matches:
  print(match.group('domain'))
```

In the above code we have three groups, the last two of which have name **domain** and **path** respectively.

<br>

The script will print out each domain name it finds in the raw web page HTML content.
```text
...
facebook.com
twitter.com
google.com
youtube.com
linkedin.com
wordpress.org
instagram.com
pinterest.com
wikipedia.org
wordpress.com
...
```

Note that you can also access groups using the group number i.e you can get the same output by just replacing
```
print(match.group('domain'))
```
with

```
print(match.group(2))
```
Just to make this more clear try to follow the image below, here we have 3 groups and three matchings. When we print the matching of a group two none and a matching would be out.



<br>
![console](https://i.imgur.com/VObOJ26.png)

### 6.2 Matching an IP Address
Regex is :
```
^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$
```
Observer this and you feel see the first group is to be repeated 3 times as {3} is mentioned and also all three groups should end with a dot. Inside the bracket we have ``25`` followed by a number between `0` and `5`; or the string `2` and a number between `0` and `4` and any number; or an optional zero or one followed by two numbers, with the second being optional. Making it a number less than 256.
Similarly we have the last group, just the dot is removed. Since dot is meta character so backslash is used to provide its literal meaning.
So the string 252.60.129.255 will be matched


![Regex](https://i.imgur.com/b22UFNP.png)

while the string 256.60.129.255 will not be matched

![Regex](https://i.imgur.com/9PXD5xL.png)

## 7 - Command Line Usage

Regular expressions are also supported by many Unix command line utilities!  We'll walk through how to use them with `grep` to find specific files, and with `sed` to replace text file content in-place.

### 7.1 - Real-World Example - Image File Matching With `grep`

We'll define another basic regular expression, this time to match image files.

```text
^.+\.(?i)(png|jpg|jpeg|gif|webp)$
```

- `^` - Start of line.
- `.+` - Match any character (letters, digits, symbols), expect for `\n` (new line), 1+ times.
- `\.` - Match the '.' character.
- `(?i)` - Signifies that the next sequence is case-insensitive.
- `(png|jpg|jpeg|gif|webp)` - Match common image file extensions
- `$` - End of line


Here's how you could list all of the image files in your `Downloads` directory.

```bash
ls ~/Downloads | grep -E '^.+\.(?i)(png|jpg|jpeg|gif|webp)$'
```

- `ls ~/Downloads` - List the files in your downloads directory
- `|` - Pipe the output to the next command
- `grep -E` - Filter the input with regular expression

## 7.2 - Real-World Example - Email Substitution With `sed`

Another good use of regular expressions in bash commands could be redacting emails within a text file.

This can be done quite using the `sed` command, along with a modified version of our email regex from earlier.

```bash
sed -E -i 's/^(.*?\s|)[^@]+@[^\s]+/\1\{redacted\}/g' test.txt
```

- `sed` - The Unix "stream editor" utility, which allows for powerful text file transformations.
- `-E` - Use extended regex pattern matching
- `-i` - Replace the file stream in-place
- `'s/^(.*?\s|)` - Wrap the beginning of the line in a capture group
- `[^@]+@[^\s]+` - Simplified version of our email regex.
- `/\1\{redacted\}/g'` - Replace each email address with `{redacted}`.
- `test.txt` - Perform the operation on the `test.txt` file.

We can run the above substitution command on a sample `test.txt` file.

```bash
My email is patrick.triest@gmail.com
```

<br>

Once the command has been run, the email will be redacted from the `test.txt` file.

```bash
My email is {redacted}
```

> Warning - This command will automatically remove all email addresses from any `test.txt` that you pass it, so be careful where/when you run it, since **this operation cannot be reversed**.  To preview the results within the terminal, instead of replacing the text in-place, simply omit the `-i` flag.


## 8. Lookaround

Lookbehind and lookahead (also called lookaround) are specific types of
***non-capturing groups*** (Used to match the pattern but not included in matching
list). Lookaheads are used when we have the condition that this pattern is
preceded or followed by another certain pattern. For example, we want to get all
numbers that are preceded by `$` character from the following input string
`$4.44 and $10.88`. We will use following regular expression `(?<=\$)[0-9\.]*`
which means: get all the numbers which contain `.` character and  are preceded
by `$` character. Following are the lookarounds that are used in regular
expressions:

|Symbol|Description|
|:----:|----|
|?=|Positive Lookahead|
|?!|Negative Lookahead|
|?<=|Positive Lookbehind|
|?<!|Negative Lookbehind|

### 8.1 Positive Lookahead

The positive lookahead asserts that the first part of the expression must be
followed by the lookahead expression. The returned match only contains the text
that is matched by the first part of the expression. To define a positive
lookahead, parentheses are used. Within those parentheses, a question mark with
equal sign is used like this: `(?=...)`. Lookahead expression is written after
the equal sign inside parentheses. For example, the regular expression
`(T|t)he(?=\sfat)` means: optionally match lowercase letter `t` or uppercase
letter `T`, followed by letter `h`, followed by letter `e`. In parentheses we
define positive lookahead which tells regular expression engine to match `The`
or `the` which are followed by the word `fat`.

<pre>
"(T|t)he(?=\sfat)" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

### 8.2 Negative Lookahead

Negative lookahead is used when we need to get all matches from input string
that are not followed by a pattern. Negative lookahead is defined same as we define
positive lookahead but the only difference is instead of equal `=` character we
use negation `!` character i.e. `(?!...)`. Let's take a look at the following
regular expression `(T|t)he(?!\sfat)` which means: get all `The` or `the` words
from input string that are not followed by the word `fat` precedes by a space
character.

<pre>
"(T|t)he(?!\sfat)" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

### 8.3 Positive Lookbehind

Positive lookbehind is used to get all the matches that are preceded by a
specific pattern. Positive lookbehind is denoted by `(?<=...)`. For example, the
regular expression `(?<=(T|t)he\s)(fat|mat)` means: get all `fat` or `mat` words
from input string that are after the word `The` or `the`.

<pre>
"(?<=(T|t)he\s)(fat|mat)" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>


### 8.4 Negative Lookbehind

Negative lookbehind is used to get all the matches that are not preceded by a
specific pattern. Negative lookbehind is denoted by `(?<!...)`. For example, the
regular expression `(?<!(T|t)he\s)(cat)` means: get all `cat` words from input
string that are not after the word `The` or `the`.

<pre>
"(?&lt;!(T|t)he\s)(cat)" => The cat sat on <a href="#learn-regex"><strong>cat</strong></a>.
</pre>

## 9. Flags

Flags are also called modifiers because they modify the output of a regular expression. A regex usually comes with in this form /abc/, where the search pattern is delimited by two slash characters /. At the end we can specify a flag with these values (we can also combine them with each other):

- `g` (global) does not return after the first match, restarting the subsequent searches from the end of the previous match.
- `m` (multi line) when enabled `^` and `$` will match the start and end of a line, instead of the whole string.
- `i` (insensitive) makes the whole expression case-insensitive (for instance `/aBc/i` would match AbC).

## 10. Substitutions

- `\t` inserts a tab character.
- `\r` inserts a carriage return character.
- `\n` inserts a newline character.
- `\f` inserts a form-feed character.


> Ok, so clearly regex is a powerful, flexible tool.  Are there times when you should avoid writing your own regex expressions? *Yes!*

## 11. When not to use Regex

### 11.1 - Language Parsing

Parsing structured languages, from English to Java to JSON, can be a real pain using regex expressions.

Writing your own regex expression for this purpose is likely to be an exercise in frustration that will result in eventual (or immediate) disaster when an edge case or minor syntax/grammar error in the data source causes the expression to fail.

Battle-hardened parsers are available for virtually all machine-readable languages, and [NLP tools](http://www.nltk.org/) are available for human languages - I strongly recommend that you use one of them instead of attempting to write your own.

#### 11.2 - Security-Critical Input Filtering and Blacklists

It may seem tempting to use regular expressions to filter user input (such as from a web form), to prevent hackers from sending malicious commands (such as SQL injections) to your application.

Using a custom regex expression here is unwise since it is very difficult to cover every potential attack vector or malicious command.  For instance, hackers can use [alternative character encodings to get around naively programmed input blacklist filters](http://www.cgisecurity.com/lib/URLEmbeddedAttacks.html).

This is another instance where I would strongly recommend using the well-tested libraries and/or services, along with [the use of whitelists instead of blacklists](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet), in order to protect your application from malicious inputs.

### 11.3 - Performance Intensive Applications

Regex matching speeds can range from not-very-fast to extremely slow, depending on [how well the expression is written](https://www.loggly.com/blog/regexes-the-bad-better-best/).  This is fine for most use cases, especially if the text being matched is very short (such as an email address form).  For high-performance server applications, however, regex can be a performance bottleneck, especially if expression is poorly written or the text being searched is long.

#### 11.4 - For Problems That Don't Require Regex

Regex is an incredibly useful tool, but that doesn't mean you should use it everywhere.

If there is an alternative solution to a problem, which is simpler and/or does not require the use of regular expressions, **please do not use regex just to feel clever**.  Regex is great, but it is also one of the least readable programming tools, and one that is very prone to edge cases and bugs.

Overusing regex is a great way to make your co-workers (and anyone else who needs to work with your code) very angry with you.

## 12. Cheat Sheets for Regular Expressions

- Essential Guide: http://coding.smashingmagazine.com/2009/06/01/essential-guide-to-regular-expressions-tools-tutorials-and-resources/

- Basic RegEx Tester: http://regexpal.com/ and http://www.myregextester.com/

- JavaScript RegEx Cheat Sheet: https://www.debuggex.com/cheatsheet/regex/javascript

- Toolbox: http://www.tripwiremagazine.com/2009/08/massive-regular-expressions-toolbox.html

- Mozilla Dev article: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp

- General Cheat Sheet: http://www.addedbytes.com/cheat-sheets/download/regular-expressions-cheat-sheet-v2.png

- GNU Grep tutorial: http://www.ibm.com/developerworks/aix/library/au-regexp/

- TAO: http://linuxreviews.org/beginner/tao_of_regular_expressions/

- List of common regex: https://gist.github.com/nerdsrescueme/1237767

## Contributing

- Report Issues
- Feel free to expand this

## Cheers ! 
