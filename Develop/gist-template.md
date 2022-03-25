# Breaking Down an Email Regular Expression (regex)

This tutorial will explain the individual components that make up a regular expression that can be used to search text for an email. A regular expression (regex) is a sequence of characters that define a search pattern. These expressions are frequently used in search algorithms, to help parse data and to validate input.

## Summary

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

The regex above consists of anchors, captured groups, bracket expressions, and quantifiers and is used to search text for patterns that match a standard email address. All of these elements must be wrapped in forward slashes because without them, regexs would be interpreted as literals by the code editor. A literal, in this context, is a series of conventional characters with obvious values. Without forward slashes, '^' would literally be interpreted as a circumflex, and 'a-z' as 'a hyphen z'. With the forward slashes, the code editor reads our input as a regex, which assigns meaning to certain literal characters/combinations of them.

Before we break down the individual parts, let's sum up, in a ridiculous amount of words, what this regex is doing. We are searching text for emails, and we'll find emails by looking for patterns that start with one or more characters a-z, 0-9, underscores, periods or hyphens, followed by an '@' symbol, followed by one or more digits, letters a-z, periods or hyphens, followed by a period '\\.', followed by 2 to 6 letters a-z or periods. If that's all you need to understand how the regex works, congratulations, you're a savant. For the rest of, let's break this down.

## Table of Contents

-   [Anchors](#anchors)
-   [Grouping and Capturing](#grouping-and-capturing)
-   [Bracket Expressions](#bracket-expressions)
-   [Quantifiers](#quantifiers)
-   [Character Classes](#character-classes)
-   [Conclusion](#conclusion)

## Regex Components

### Anchors

The character directly to the right of the opening forward slash, the circumflex ^, and the character directly to the left of the closing forward slash, the dollar sign $, are the starts-with and ends-with anchors for this expression. These anchors are placed in front of and after the patterns we're searching for that start and end the string, respectively. Our regex says the email address should start and end with a character/s that match the criteria inside the respective pair of parentheses. Anything wrapped inside parentheses is called a group, which we'll discuss below.

Our anchers and the groups that define the starts-with and ends-with criteria:

```
starts-with
^([a-z0-9_\.-]+)

ends-with
([a-z\.]{2,6})$

full regex
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

Anchors can be placed adjacent to exact characters to indicate the string should start with this combo of characters exactly, or adjacent to a range of possible characters enclosed in a bracket expression/group. For example:

```
^The end$
```

looks for a pattern of characters that exactly starts with 'The', has a space in-between, and ends with 'end' and is case sensitive.

Let's take a pause to note that our bracket expression has three groups; two of which are next to the anchors. There is a group to the right of the starts-with anchor, followed by an '@' symbol, followed by another group, followed by a '\\.' (we use the backslash to escape the literal and indicate we literally mean the pattern must contain a period there) followed by a third group to the left of the ends-with anchor. This pattern matches the way we see most standard email addresses: 'group@group.group'. Each group has certain criteria defined in the bracket expressions, and our regex searches for strings that match the entire regex pattern.

### Grouping and Capturing

Grouping is done with parentheses '()' to help break up complex sections of regexs when searching a string for different requirements, and the email regex has three groups that we capture. The two groups next to our anchors, and shown above, are wrapped in parentheses to indicate a group, and there's a third group in the middle that comes after the '@' character and before the '\\.':

```
middle group
([\da-z\.-]+)
```

Grouping and capturing is useful because we can extract and reference exact information from a bigger match, we can rematch a previous matched group and we can also implement substitutions on groups to manipulate data. If, for whatever reason, you only wanted to capture the first and third groups from our regex to reference that data later, then you could add a '?:' to the right of the left parenthesis to not capture the second group. While we won't go over capturing in detail, for the purposes of this tutorial it's important to notice the three groups in our regex because they make up the standard email format we are used to:

```

username@domain-name.domain

```

In our regex, we want to use more than one group because each part of a standard email address has different requirements. Because we group and capture it, we open a huge door of possibilities for data extraction and manipulation, which is beyond the scope of this tutorial.

### Bracket Expressions

Inside each of our groups are bracket expressions. Anything inside a set of square brackets '[]' represents a range of characters that define the pattern we're looking for. Starting with the left-most bracket expression:

```

[a-z0-9_\.-]+

```

The first bracket expression indicates we're searching the text for a character that meets the defined criteria inside the square brackets. It starts with the 'a-z' range, meaning we're looking for a character that can be a letter a through z, case sensitive. The '0-9' means the character could also be the characters 0 through 9. The diction here is important because a character 0-9 means the explicit characters 0-9, even though unicode has many more defined digits that aren't explicit characters; more on this later. The underscore is a literal character, and doesn't have any special meta meaning attached, so it tells the editor that the character could also be an underscore. The period however, does have a meta significance (it indicates any character except newline). In order to include a literal period in our character search criteria, we need to escape the regex with a backslash. Lastly, we have a hyphen, and like the underscore, we mean to say the character could also be a hyphen. The plus sign '+' is a quantifier and is not technically part of the bracket expression, but rather indicates that we're looking for one or more characters that meet the criteria and appear before an '@' character.

The second bracket expression differs because it includes the 'digit' criteria '\d':

```

[\da-z\.-]+

```

The '\d' is a character class that would match a digit. Unicode defines digits as more than just the 10 number keys, 0-9. This means that our second bracket notation would match characters that represent digits outside of the 10 number keys. For example, 'V.' is the unicode for 70 in indic (mirod Stackoverflow.com 5.21.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex). We'll cover character classes below. For now we only need to understand why the second, middle bracket expression is looking for one or more characters that could be a 'digit', letters a through z, periods or hyphens. The middle expression is searching for this pattern in between an '@' symbol and period '\\.'.

Our third bracket expression introduces a new quantifier {2,6}

```

[a-z\.]{2,6}

```

This quantifier specifies the number of characters we're looking for that match the bracket expression criteria. We're searching a string for 2-6 characters, and these characters could be letters a-z or periods '\\.'.

### Quantifiers

In our first bracket expression, we use the '+' quantifier to indicate we're looking for one or more of the characters that meet the criteria inside the square brackets '[]'. All quantifiers are inherently greedy (as opposed to lazy, which we won't cover because our regex doesn't have any lazy quantifiers). A greedy quantifier will look for the most occurances of characters that match the pattern. For curious minds, this means that even if there are multiple occurances of the pattern within a larger occurence of the pattern, only the larger occurence will match.

The first bracket expression represents a standard email username, and the plus sign quantifier means the following usernames would match the pattern:

```

a@gmail.com
5555555555555555555555555555555555555555@gmail.com
_@gmail.com
...@gmail.com
to-.ny@gmail.com

```

The third bracket expression is followed by the quantifier {2,6} and means we're looking for a domain that matches the given criteria and is from two to six characters long. The following top-level domains meet the bracket expression criteria given the quantifier that follows:

```

tony@gmail.com
tony@gmail.info
tony@gmail.de
tony@gmail.gov.us

```

### Character Classes

The middle bracket expression uses the digit character class represented by '\d.' A character class is a defined set of characters that could fulfill a match. Character classes can be words '\w', whitespace '\s', bracket expressions [abc], or their opposites: not a word '\W', not a digit '\D', not a whitespace '\s', and not 'abc' [^abc].

In the context of our regex that uses both [0-9] and '\d', Nicholas Knight from stackoverflow.com summarizes the difference between [0-9] and '\d' perfectly:

"For maximum safety, I'd suggest using [0-9] any time you don't specifically intend to match all unicode-defined digits.

Per perldoc perluniintro, Perl does not support using digits other than [0-9] as numbers, so I would definitely use [0-9] if the following are both true:

You want to use the result as a number (such as performing mathematical operations on it or storing it somewhere that only accepts proper numbers (e.g. an INT column in a database)).

It is possible non-digits [^0-9] would be present in the data in such a way that the regular expression could match them. (Note that this one should always be considered true for untrusted/hostile input.)

If either of these are false, there will only rarely be reason to specifically not use \d (and you'll probably be able to tell when that is the case), and if you're trying to match all unicode-defined digits, you'll definitely want to use \d." (5.20.09 https://stackoverflow.com/questions/890686/should-i-use-d-or-0-9-to-match-digits-in-a-perl-regex)

## Conclusion

```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

In conclucsion, the regex above can help search text for emails, and we'll find emails by looking for patterns that start with one or more characters a-z, 0-9, underscores, periods or hyphens, followed by an '@' symbol, followed by one or more digits, letters a-z, periods or hyphens, followed by a period '\\.', followed by 2 to 6 letters a-z or periods.

## Author

My name is Tony Linz and I'm a Web Development student at the University of Wisconsin-Madison who enjoys programming in javascript and python. While I was born and raised in the Milwaukee, WI metro area, I spent the last decade soaking in the mountains and ocean in Seattle, WA. I intend to explore the professional tech world that Seattle has to offer after school.

Link to Tony's profile: https://github.com/alinz07

### **Credits**

-   https://www.gavilan.edu/csis/languages/literals.html#:~:text=Literals%20or%20constants%20are%20the,explicit%20constants%20or%20manifest%20constants.

-   https://stackoverflow.com/questions/3512471/what-is-a-non-capturing-group-in-regular-expressions

-   https://coding-boot-camp.github.io/full-stack/computer-science/regex-tutorial

-   https://regexr.com/

-   https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285
