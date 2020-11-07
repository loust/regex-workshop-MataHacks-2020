![logo](/res/regexMATAHACK.png)
<sup><sup>Font: Road Rage</sup></sup>

# regex-workshop-MataHacks-2020
This repo is a workshop designed for MataHacks 2020

# Content
1. [Requirements](#requirements)
2. [Regex basics: Syntax](#regex-basics-syntax)
    * [Single and Multiple capturing groups](#single-and-multiple-capturing-groups)
    * [Non-capturing groups](#non-capturing-groups)
    * [Word phrasing](#word-phrasing)
    * [Sentences with changing data](#Sentences-with-changing-data)
    * [References](#references)
    * [Quantifiers](#quantifiers)
3. [Advanced Syntax](#Advanced-Syntax)
    * [Conditional Statements](#conditional-statements)
    * [Lookahead and Lookbehind](#lookahead-and-lookbehind)
    * [Flags](#flags)
    * [Some Examples](#some-examples)
4. [Resources](#Resources)

# Requirements
Feel free to do this in Python or your favorite language, or just use https://regex101.com
The content of the challenges are in each folder in the repo.

# Regex basics: Syntax
## Single and Multiple capturing groups

Regex stands for ReGular EXpression. It is desinged to give you the freedom to capture text from a paragraph or any kind of input via a pattern or specific keywords.

Before that, we will need to understand what gets "captured" and what does not get captures. If you specify your pattern without capturing, it will not return anything to you, but it will see (match) it.

Using the parentheses, you can capture a pattern. Let's take this paragraph from the movie On The Waterfront

```
You don't understand!  I coulda had class. I coulda been a contender. I could've beensomebody, instead of a bum, which is what I am.
```

Now, if we use the regex match to do the following: `( ... )`

It will be able to do a __Full match__ on "I am" and it will __Group__ "I am" by putting it in __Group 1__

You can have several parentheses and make several groups, but in general, usually one is enough for simple applications.

## Non-capturing groups
Non-capturing groups are used as anonymous placeholders or to separate rules from each other. They are usually placed inside of the main capture group.

The syntax for this is `(?: ... )`

Let's take the quote from earlier and try to find words that have apostrophes

```regex
((?:[\w]+)\'(?:[\w]+))
```

Note, this is a bit excessive, since there's two non-capturing groups and new content not yet discussed.

In transition, let's dive into matching letters, numbers and special characters.

## Word phrasing
From the regex search above:
```regex
(
 (?:
  [\w]+
 )
 \'
 (?:
  [\w]+
 )
)
```
We can see that `\w` is used. This is basically doing the same thing as `[a-zA-Z]` it is a shortcut used for more modern frameworks that utilize regex. For example, in Python, this will work. If you do this in Bash, it will fail. In bash, you will need to stick to doing `[a-zA-Z]` unless there is a way around it.

What this does:

`\w` matches all a word character. Remember, just ONE character.
Now, putting this in brackets: `[\w]` will allow any kind of word match. For instance, notice how we left one quote escaped outside of a bracket?
This means that it will capture a __static__ single quote `\'`.

Remember the `+` from earlier? This is used to __continue__ the word finding. Just using `[\w]` will only find one single word character. If we add a plus sign, it will "continue" the capture until a stop is stated. In our case, the single quote `\'` acts as a stop:

```regex
[\w]+\'
```
This will capture every word character, including the single quote and STOP. This is why, we add another `[\w]+` after the single quote

## Sentences with changing data
Let's say you have sentences that have changing words in them, but the pattern of the sentence stays the same. For example, CSV data the only the company name changes, or has similar names. Let's say the data looks like this:
```
20201001, google, 21, 51, true
20201001, yahoo, 23, 25, true
20201001, yandex, 22, 34, true
```

If you only want to show rows/sentences with google and yahoo, you'd do something like this:
```regex
((?:[\d]+,\s)(?:google|yahoo),\s[\d]+,\s[\d]+,\s[\w]+)
```
A really un-optimized, but it gathers only the rows that have google and yahoo in them.

![capture001](/res/capture001.PNG)


Moving on, there are several other ways to capture, for example digits
## References
These are basic word tokens.
```
[a-zA-Z] = [\w]
Anything that is NOT a word character = [\W]
[0-9] = [\d]
Non digits = [\D]
whitespaces = [\s]
Non-whitespaces = [\S]
```

Use the Quick References area on the bottom right of https://regex101.com/

## Quantifiers
This method makes it easy to count as many characters with limits. This is useful when you know what are the min and max values of specific patters in a word. For example, in domain names, we can only have 2 minimum top level domain, such as `.co.jp` and `.com` as a 3 character domain. Though, if you look around, there are longer top leve ldomains, such as `.info` In this case, let's just say it's 5. Domain names however, are 63 maximum and 1 character minimum. So, creating a regex that gets domain names:

![domains](/res/domains.PNG)

Notice how there are limits set with quote separation in the quantifier. You can ignore this if there is NO min or max values. If the value is specifically 5 characters, you can only do the following:
```regex
(
 [\w]{5}
)
```

This will capture 5 characters ONLY


# Advanced Syntax
## Conditional statements

Think of conditional statements as if else statements in regular programming, but in this case, it's a bit different. It either captures the left side or the right. The left one is captured if the statement is true. If not, it captures the right.

Here is a good example of this working correclty:
https://regex101.com/r/dS1dG4/1

The way this is supposed to work is that you have a conditional at
```regex
(?=<pattern>)<truepatter>|<falsepattern>
```
So, in the sense of, if it succeeds, it will do the true pattern, if it fails, it will do the false patterh. But this is easily countered via software. You can do this in your software to minimize debugging, but in the end, if you wish to do it via regex, crafting the query shouldn't be that bad if you have specific type of data that you want to grab.

This example was found from this thread: https://stackoverflow.com/questions/39222950/regular-expression-with-if-condition

## Lookahead and Lookbehind
Lookaheads are similar to regular conditional statements. Lookbehinds are different. They check the previous character and start matching your pattern after the character or pattern you stated exists.

There are also negative lookaheads and negative lookbehinds.

Regular lookbehinds are as follows: `(?<=pattern)pattern` Negatives lookbehinds are `(?<!pattern)pattern`


## Flags
Before moving to advanced Regex, let's talk about flags. One of the flags you'd use a lot is the Global flag (g) check out the options in https://regex101.com/ in your bar, there's a drop down menu where you can select what other flags to select. This is also available in Python as well, for example. You don't really need to use some other flag randomly out of nowhere, because usually, the global flag works just fine in a lot of cases. Read more about them here: http://xregexp.com/flags/

## Some examples
Here are some Regex queries that I have created:

This one finds IP addresses:
```regex
((?:(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9])\.){3}(?:25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9]?[0-9]))
```

This one finds email addresses:
```regex
\b([\w\.\_\-\d]+\S+\@\S+[\w\.\-\d]+\S+\.\S+[a-zA-Z]{1,7})\b
```

These ones finds hashes:
```regex
Generic: \b[A-Fa-f0-9]{32}\b|\b[A-Fa-f0-9]{40}\b|\b[A-Fa-f0-9]{64}\b
MD5: \b([A-Fa-f0-9]{32})\b
SHA1: \b[A-Fa-f0-9]{40}\b
SHA256: \b[A-Fa-f0-9]{64}\b
```

This one grabs URLs:
```regex
\b((?<!\w\/)[\w\d\-\.]{0,61}\S\.\S[a-zA-Z]{1,7})\b
```

This one grabs URLs:
```regex
\b((?:(?:\S+(?:(?:\:\/{2})|(?:\.[\w]+[\/].+?)))(?:[\w\d\.\[\]\_\/\-\?\&\;\=\:]+)))\b
```

This one grabs Domains. Ignoring the trailing file, and ignoring the protocol.
```regex
\b((?<!\w\/)[\w\d\-\.]{0,61}\S\.\S[a-zA-Z]{1,7})\b
```

# Resources
* https://www.regular-expressions.info/
* https://regex101.com/
* https://www.rexegg.com/
