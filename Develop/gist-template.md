# Breaking Down an Email Regular Expression (regex)

This tutorial will detail the individual components that make up a regular expression that matches an email. A regular expression (regex) is a sequence of characters and meta-characters that define a specific search pattern. These expressions are frequently used in search algorithms, to help parse data and to validate input.

## Summary

/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/

The regex above is made up of anchors, captured groups, bracket expressions, and quantifiers. All of these elements must be wrapped in forward slashes because they are considered literals. A literal is a conventional character with an obvious value. 1 would be interpreted as a 1, and a-z as a hyphen z. The slashes help the code editor read our input as a regex.

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

## Table of Contents

-   [Anchors](#anchors)
-   [Grouping and Capturing](#grouping-and-capturing)
-   [Bracket Expressions](#bracket-expressions)
-   [Quantifiers](#quantifiers)
-   [Character Classes](#character-classes)

## Regex Components

### Anchors

The first character to the right of the opening forward slash, the circumflex ^, and the first character to left of the closing forward slash, the dollar sign $, are the anchors for this expression. These characters indicate the start and end of the string, respectively. Anchors can be used with exact string characters and or with a range of possibilities in a bracket expression. For example:

^The end$

looks for a string that exactly matches strings that start with 'The' and end with 'end', case sensitive. Our email regex uses a range in a bracket expression. Note: this bracket expressions is inside the group captured in the first set of parentheses on the left:

[a-z0-9_\.-]+

When we are searching text for an email, we want a string that starts with characters that meet the regex criteria inside of the brackets, with the plus sign quantifier attached, and a string that ends with characters that meet the regex criteria inside of the brackets in the group captured in the right-most parentheses:

[a-z\.]{2,6}

### Grouping and Capturing

Grouping is done with parentheses () to break up sections of one or more strings to look for different requirements, and the email regex has three groups that we capture. The two that mark the beginning and the end of the string, as well as a third one in the middle that comes after the '@' character and before the '\.':

([\da-z\.-]+)

Grouping and capturing is useful because we can extract and reference exact information from a bigger match, we can rematch a previous matched group and we can also implement substitutions on groups to replace data. If, for whatever reason, you only wanted to capture the first and third groups from our regex to reference that data later, then you could use a '?:' to not capture the second group. While we won't go over capturing in detail, it's important to notice the three groups from our regex, because they make up the standard email format we are used to:

username@domain-name.domain

Our first group is the username criteria, followed by an '@' character, followed by our second group criteria for the domain name, followed by a '\.' (we used the backslash to escape the literal and indicate we literally mean we want a period), followed by our third group that makes up the domain criteria. We want to use more than one group here because each of the parts of a standard email address have different requirements.

### Bracket Expressions

Inside each of our groups are bracket expressions. Anything inside a set of square brackets '[]' represents a range of characters that we want to match. Starting from left to right:

[a-z0-9_\.-]+

This bracket expression starts with the a-z range, meaning we're looking for a character from a-z, case sensitive. The 0-9 means the character could also be the characters 0 through 9. The diction here is important because a character is specifically the keys 0-9, while unicode has many more defined digits that aren't exact numbers, that could be accessed using '\d'. More on this later. The underscore is a literal character, and doesn't have any special meta meaning attached, so it tells the editor that the character could also be an underscore. The period however, does have a meta significance; it indicates any character except newline. In order to include a period in our character criteria, we need to escape the regex with a backslash. Lastly, we have a hyphen, and like the underscore, we mean to say the character could also be a hyphen. The plus sign '+' is a quantifier, and not part of the bracet expression. It means we're looking for one or more of the characters in the bracket expression. More on this later.

The second bracket expression differs because it includes the digit criteria '\d':

[\da-z\.-]+

The '\d' is a character class that would match a single character that is a digit. Because unicode defines digits as more than just the 10 number keys, 0-9, this means that our second bracket notation would accept digit representations outside of the 10 number keys. For example, 'V.' is the unicode for 70 in indic (mirod Stackoverflow.com 5.21.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex).

Our third brack expression introduces a new quantifier {2,6}

[a-z\.]{2,6}

This quantifier specifies the number of characters we're looking for that match the bracket expression criteria.

### Quantifiers

In our first bracket expression, we use the '+' quantifier to indicate we're looking for one or more of the characters that meet the criteria in the bracket expression. Since the first bracket expression is a username, the plus sign quntifier means the following would be valid usernames in a standard email:

a@gmail.com
5@gmail.com
_@gmail.com
...@gmail.com
to-ny@gmail.com

The third brack expression is followed by the quantifier {2,6} to mean the domain needs to be between two and six characters (that meet the bracket expression criteria) long. The following top level domains meet the bracket expression criteria given the quantifier the follows:

tony@gmail.com
tony@gmail.info
tony@gmail.de

### Character Classes

The middle bracket expression include the digit character class represented by '\d.' A character class is a defined set of characters that could fulfill a match. Character classes can be words '\w', whitespace '\s, bracket expressions [abc], or their opposites not a word '\W', not a digit '\D', not a whitespace '\s', and not 'abc' [^abc].

For our regex, Nicholas Knight from stackoverflow.com summarizes the difference between [0-9] and '\d' perfectly:

"For maximum safety, I'd suggest using [0-9] any time you don't specifically intend to match all unicode-defined digits.

Per perldoc perluniintro, Perl does not support using digits other than [0-9] as numbers, so I would definitely use [0-9] if the following are both true:

You want to use the result as a number (such as performing mathematical operations on it or storing it somewhere that only accepts proper numbers (e.g. an INT column in a database)).

It is possible non-digits [^0-9] would be present in the data in such a way that the regular expression could match them. (Note that this one should always be considered true for untrusted/hostile input.)

If either of these are false, there will only rarely be reason to specifically not use \d (and you'll probably be able to tell when that is the case), and if you're trying to match all unicode-defined digits, you'll definitely want to use \d." (5.20.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex)

## Author

Tony Linz is a Web Development student at the University of Wisconsin-Madison who enjoys programming in javascript and python. While he was born and raised in the Milwaukee, WI metro area, he has spent the last decade in Seattle, WA and intends to explore the professional tech world that Seattle has to offer after school.

Link to Tony's profile: https://github.com/alinz07
