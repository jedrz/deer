In this file I'll try to explain some design decisions and how `deer` works.

VARIABLES
=========

**DEER_DIRNAME**

The full path to the current directory, without the trailing slash.

**DEER_BASENAME**

The name of the currently selected file or directory.

Due to the special case of the root directory (`/`), simply using
`$DEER_DIRNAME/$DEER_BASENAME` to get the full path to the selected
directory is not sufficient as it would result in a double slash.
Currently I use `${DEER_DIRNAME%/}/$DEER_BASENAME` to eliminate one
slash in that corner case. It complicates the code in a few places but
it's more maintainable then keeping the slash in `DEER_BASENAME` and
concatenating dirname with basename without the additional slash in
between (it was the case up until recently).

**DEER_FILTER**

An associative array mapping the dirnames to the filter to be applied
there. It's modified in the function `deer-enter`. If the directory
does not exist in that map, it's value should be assumed to be `*`.
The prefered notation is quite ugly but it works:
`${DEER_FILTER[$DEER_DIRNAME]:-'*'}`

**OLD_LBUFFER and OLD_RBUFFER**

The line buffer stored to restore the `LBUFFER` and `RBUFFER` contents
later (in the `deer-restore` function).

**PREDISPLAY**

The built-in variable used to display the preview of the left side of
the buffer (`OLD_LBUFFER`). I tried to do the same with `POSTDISPLAY`
and the right side of the buffer but I was unable to apply the
coloring to it (using `region_highlight`).