# reStructuredText
## Inline markup
* Emphasis (italic) text: \*emphasis for this phrase\* .
* Extra emphasis (bold) text: \*\*extra emphasis for this phrase\*\* .
* For lists without numbers, a simple dash with spaces after it:
- item 1
- item 2
The space after the dash is required for reStructuredText to recognize the list.
* For lists with numbers, the number followed by a period and a space:
1. item 1
2. item 2
* For numbered lists, the period after the number is required.
* Interpreted text: These are domain specific. Within Python documentation, the default role is code which means that surround text with back ticks will convert your code to use code tags. For example, `if spam and eggs:` . Different roles can be set through either a role prefix or suffix depending on your preference. For example, :math:`E=mc^2` to show mathematical equations.
* Inline literals: This is formatted with a mono-space font, which makes it ideal for inline code. Just add two back ticks to \`\`add some code\`\` .
* References: These can be created through a trailing underscore. They can point to headers, links, labels, and more. The next section will cover more about these, but the basic syntax is simply reference_ or enclosed in back ticks when the reference contains spaces, \`some reference link\`_ .
* To escape the preceding characters, the backslash can be used. So if you wish to have an asterisk with emphasis, it's possible to use *\** , quite similar to escaping in Python strings.

## Headers
Headers are useful for three aspect:

1. It formatted according to its level;
2. Sphinx can generate Table Of Contents (TOC) tree from headers;
3. Headers are automatically function as labels, can create links towards to them.
