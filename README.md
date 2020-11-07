![logo](/res/regexMATAHACK.png)
<sup><sup>Font: Road Rage</sup></sup>

# regex-workshop-MataHacks-2020
This repo is a workshop designed for MataHacks 2020

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

![capture001](/res/capture001.PNG)


Moving on, there are several other ways to capture:
```regex
[0-9] = [\d]
```

