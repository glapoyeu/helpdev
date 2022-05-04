# Syntax guide  
Here’s an overview of Markdown syntax that you can use anywhere on GitHub.com or in your own text files.

## Headers
```
# This is an <h1> tag
## This is an <h2> tag
###### This is an <h6> tag 
```
## Emphasis
```
*This text will be italic*
_This will also be italic_

**This text will be bold**
__This will also be bold__

_You **can** combine them_  
```

## Lists
### Unordered
```
* Item 1
* Item 2
  * Item 2a
  * Item 2b
```
### Ordered
```
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b 
```

## Images
```
![GitHub Logo](/images/logo.png)
Format: ![Alt Text](url)
```
## Links
```
http://github.com - automatic!
[GitHub](http://github.com)
```

## Blockquotes
```
As Kanye West said:

> We're living the future so
> the present is our past. 
```

## Inline code
```
I think you should use an
`<addr>` element here instead.
```

# GitHub Flavored Markdown  
GitHub.com uses its own version of the Markdown syntax that provides an additional set of useful features, many of which  make it easier to work with content on GitHub.com.  
Note that some features of GitHub Flavored Markdown are only available in the descriptions and comments of Issues and  Pull Requests. These include @mentions as well as references to SHA-1 hashes, Issues, and Pull Requests. Task Lists are  also available in Gist comments and in Gist Markdown files.


## Syntax highlighting
Here’s an example of how you can use syntax highlighting with GitHub Flavored Markdown:  
```
javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}  
```
You can also simply indent your code by four spaces:
```
function fancyAlert(arg) {
      if(arg) {
        $.facebox({div:'#foo'})
      }
    }
```
Here’s an example of Python code without syntax highlighting:
```
def foo():
    if not bar:
        return True
```

## Task Lists
```
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item 
```
If you include a task list in the first comment of an Issue, you will get a handy progress indicator in your issue list. It also works in Pull Requests!

## Tables
You can create tables by assembling a list of words and dividing them with hyphens - (for the first row), and then  separating each column with a pipe |:
```
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
```

## Editor Online
- [markdownlivepreview](https://markdownlivepreview.com/)

# More

- [Tables Generator](https://www.tablesgenerator.com/)