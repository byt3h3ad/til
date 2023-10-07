# Indenting Code

Vim provides `==` and `=` as tools to aide in keeping your code formatted and tidy. Double equal can be used to quickly properly indent the current line. Similarly, `10==` will indent 10 lines, inclusive of the one you’re one.

There are a few options to achieve the same result:

- `4==` will indent the current line, and the next three.
- `=ap` will indent **A**round the current **P**aragraph. Having no empty lines, this method qualifies as a paragraph.
- `=%` will indent to the the end of the method. Note this implies the use of the *matchit* plugin. But `%` works with common curly brace blocks, parens, etc out of the box.
- `V3j=` will do a visual select on the current line and the three below and apply indenting to them.
- Lastly, the nuclear option `gg=G` can be used to indent an entire file.
