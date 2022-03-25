# Breaking Down an Email Regular Expression (regex)

This tutorial will detail the individual components that make up a regular expression that can be used to search text for an email. A regular expression (regex) is a sequence of characters and meta-characters that define a specific search pattern. These expressions are frequently used in search algorithms, to help parse data and to validate input.

## Summary

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

The regex above consists of anchors, captured groups, bracket expressions, and quantifiers and is used to search text for patterns that match a standard email address. All of these elements must be wrapped in forward slashes because without them, regexs would be interpreted as literals by the code editor. A literal, in this context, is a series of conventional characters with obvious values. Without forward slashes, '^' would literally be interpreted as a circumflex, and 'a-z' as 'a hyphen z'. With the forward slashes, the code editor reads our input as a regex, which assigns meaning to certain literal characters/combinations of them.

Before we break down the individual parts, let's sum up, in a ridiculous amount of words, what this regex is doing. We are searching text for emails and we'll find emails by looking for patterns that start with one or more characters a-z, 0-9, and \_ . or -, followed by an '@', followed by one or more digits, letters a-z, . or hyphen, followed by a period '\.', followed by 2 to 6 letters a-z or periods '\.'. If that's all you need, congratulations, you're a savant. For the rest of, let's break this down.

## Table of Contents

-   [Anchors](#anchors)
-   [Grouping and Capturing](#grouping-and-capturing)
-   [Bracket Expressions](#bracket-expressions)
-   [Quantifiers](#quantifiers)
-   [Character Classes](#character-classes)

## Regex Components

### Anchors

The character directly to the right of the opening forward slash, the circumflex ^, and the character directly to the left of the closing forward slash, the dollar sign $, are the starts-with and ends-with anchors for this expression. These anchors indicate the pattern we're searching for that start and end the string, respectively. So if we're looking for an email address, we're saying the email address should start and end with a set of characters that match the criteria inside the respective parentheses.

Anchors can indicate we're searching the string for a pattern that starts with the exact characters defined in the criteria or with a range of possible characters enclosed in a bracket expression. For example:

```
^The end$
```

looks for a pattern of characters that exactly starts with 'The' and ends with 'end', case sensitive and with nothing in between.

Our email regex has both a starts-with and ends-with anchor next to grouped bracket expressions. The bracket expressions offer ranges of characters and character options that define the starts-with and ends-with criteria 'a-z' '0-9', etc. More on bracket expressions to come. Note: the bracket expressions are enclosed in parentheses '()', which indicate a group, one contains a '+' quantifier and the other contains a {2,6} quantifier; we'll cover groups and quantifiers later.

```
full regex
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/

bracket expression by the starts with anchor (enclosed in a group () and with a quantifier +)
^([a-z0-9_\.-]+)

bracket expression by the end with anchor (enclosed in a group () and with a quantifier {2,6})
([a-z\.]{2,6})$
```

At this point, it's good to note there is a group to the right of the starts-with anchor, followed by an '@' symbol, followed by another group, followed by a '\.' (we used the backslash to escape the literal and indicate we literally mean we want a period) followed by a third group to the left of the ends-with anchor. In terms of an email that we're used to seeing, think of it as 'group@group.group'. Each group has certain criteria defined in the bracket expressions. Our regex searches for strings that match the entire pattern.

### Grouping and Capturing

Grouping is done with parentheses '()' to help break up complex sections of regexs when searching a string for different requirements, and the email regex has three groups that we capture. The two bracket expressions discussed directly above and wrapped in parentheses are groups, as well as a third one in the middle that comes after the '@' character and before the '\.':

```
middle group
([\da-z\.-]+)
```

Grouping and capturing is useful because we can extract and reference exact information from a bigger match, we can rematch a previous matched group and we can also implement substitutions on groups to replace data. If, for whatever reason, you only wanted to capture the first and third groups from our regex to reference that data later, then you could use a '?:' to not capture the second group. While we won't go over capturing in detail, for the purposes of this tutorial it's important to notice the three groups in our regex because they make up the standard email format we are used to:

```

username@domain-name.domain

```

In our regex, we want to use more than one group because each part of a standard email address has different requirements. Because we group and capture it, we open a huge door of possibilities for data extraction and manipulation, which is beyond the scope of this tutorial.

### Bracket Expressions

Inside each of our groups are bracket expressions. Anything inside a set of square brackets '[]' represents a range of characters that define the pattern we're looking for. Starting with the left-most bracket expression:

```

[a-z0-9_\.-]+

```

The first bracket expression indicates we're searching the string for a character that meets the defined criteria inside the square brackets. It starts with the 'a-z' range, meaning we're looking for a character that can be a letter a through z, case sensitive. The '0-9' means the character could also be the characters 0 through 9. The diction here is important because a character 0-9 means specifically the explicit characters 0-9, even though unicode has many more defined digits that aren't integer characters; more on this later. The underscore is a literal character, and doesn't have any special meta meaning attached, so it tells the editor that the character could also be an underscore. The period however, does have a meta significance (it indicates any character except newline). In order to include a literal period in our character search criteria, we need to escape the regex with a backslash. Lastly, we have a hyphen, and like the underscore, we mean to say the character could also be a hyphen. The plus sign '+' is a quantifier and is not technically part of the bracket expression, but rather indicates that we're looking for one or more characters that meet the criteria and appear before an '@' character.

The second bracket expression differs because it includes the 'digit' criteria '\d':

```

[\da-z\.-]+

```

The '\d' is a character class that would match a digit. Unicode defines digits as more than just the 10 number keys, 0-9. This means that our second bracket notation would match characters that represent digits outside of the 10 number keys. For example, 'V.' is the unicode for 70 in indic (mirod Stackoverflow.com 5.21.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex). We'll cover character classes to come, but just know that the second, middle bracket expression, that is also grouped in parentheses, is looking for one or more characters that could be a 'digit', letter a through z, or a period or hyphen. The middle expression is searching for this pattern in between an '@' symbol and period '\.'.

Our third bracket expression introduces a new quantifier {2,6}

```

[a-z\.]{2,6}

```

This quantifier specifies the number of characters we're looking for that match the bracket expression criteria. We're searching a string for 2-6 characters, and these characters could be the letter a-z or a period '\,'.

### Quantifiers

In our first bracket expression, we use the '+' quantifier to indicate we're looking for one or more of the characters that meet the criteria in the bracket expression. All quantifiers are inherently greedy (as opposed to lazy, which we won't cover because our regex doesn't have any lazy quantifiers). A greedy quantifier will look for the most occurances of characters that match the pattern. The first bracket expression represents a standard email username, and the greedy, plus sign quantifier means the following would be valid usernames in an email:

```

a@gmail.com
5555555555555555555555555555555555555555@gmail.com
_@gmail.com
...@gmail.com
to-ny@gmail.com

```

The third bracket expression is followed by the quantifier {2,6} and means we're looking for a domain that matches the given criteria and is from two to six characters long. The following top-level domains meet the bracket expression criteria given the quantifier that follows:

```

tony@gmail.com
tony@gmail.info
tony@gmail.de

```

### Character Classes

The middle bracket expression uses the digit character class represented by '\d.' A character class is a defined set of characters that could fulfill a match. Character classes can be words '\w', whitespace '\s', bracket expressions [abc], or their opposites: not a word '\W', not a digit '\D', not a whitespace '\s', and not 'abc' [^abc].

In the context of our regex that uses both [0-9] and '\d', Nicholas Knight from stackoverflow.com summarizes the difference between [0-9] and '\d' perfectly:

"For maximum safety, I'd suggest using [0-9] any time you don't specifically intend to match all unicode-defined digits.

Per perldoc perluniintro, Perl does not support using digits other than [0-9] as numbers, so I would definitely use [0-9] if the following are both true:

You want to use the result as a number (such as performing mathematical operations on it or storing it somewhere that only accepts proper numbers (e.g. an INT column in a database)).

It is possible non-digits [^0-9] would be present in the data in such a way that the regular expression could match them. (Note that this one should always be considered true for untrusted/hostile input.)

If either of these are false, there will only rarely be reason to specifically not use \d (and you'll probably be able to tell when that is the case), and if you're trying to match all unicode-defined digits, you'll definitely want to use \d." (5.20.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex)

## Author

Tony Linz is a Web Development student at the University of Wisconsin-Madison who enjoys programming in javascript and python. While he was born and raised in the Milwaukee, WI metro area, he spent the last decade in Seattle, WA and intends to explore the professional tech world that Seattle has to offer after school.

Link to Tony's profile: https://github.com/alinz07
