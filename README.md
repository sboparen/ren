ren
===

Use your favourite text editor (as determined by the environment
variable `$EDITOR`) to rename files!

## Example Usage

Executing the following commands

```
$ touch a.txt b.txt c.txy                   
$ ren
```

will launch a text editor containing

```
# Change the file names below as desired.
# Save and quit to perform the rename.
# Do not change the number of lines (including these lines).
a.txt
b.txt
c.txy
```

and then if we fix the typo like so,

```
# Change the file names below as desired.
# Save and quit to perform the rename.
# Do not change the number of lines (including these lines).
a.txt
b.txt
c.txt
```

saving and quitting will yield

```
Rename commands saved to '/home/sboparen/.ren/fEDScU'.
Renaming 'c.txy' to 'c.txy.TEMP'.
Renaming 'c.txy.TEMP' to 'c.txt'.
Done. To undo, type:
ren -r '/home/sboparen/.ren/fEDScU'
```

Note that `ren` can also handle cyclic renames, for example
to rename `a -> b -> c -> a`.

```
$ echo a >a; echo b >b; echo c >c
$ cat a b c
a
b
c
$ ren
Rename commands saved to '/home/sboparen/.ren/HKNGl6'.
Renaming 'a' to 'a.TEMP'.
Renaming 'b' to 'b.TEMP'.
Renaming 'c' to 'c.TEMP'.
Renaming 'a.TEMP' to 'b'.
Renaming 'b.TEMP' to 'c'.
Renaming 'c.TEMP' to 'a'.
Done. To undo, type:
ren -r '/home/sboparen/.ren/HKNGl6'
$ cat a b c
c
a
b
```

# Notes

I wrote this program a long time ago, but I use it all the time.
Also, I last felt the need to modify it in 2011, so I guess it
counts as a mature program.

#### Intentional Limitations
* No attempt was made to handle file names with newlines in them.
* The renaming is done in two stages, where the first stage adds
`.TEMP` to the names of all involved files.  `ren` does simulate
the renames before executing them, but it might not come up with a
sequence of commands that works if some of your file names already
end in `.TEMP`.

My biggest regret is the way it renames files by creating a hard
link and then unlinking the original file name.  This can cause
problems, most notably:

* It doesn't work at all on filesystems which don't support hard
links.
* It essentially never works on directories, since hard links to
directories are generally disallowed.

If I had to do it again, I would probably allow the user to specify
an external command so that the actual renaming operation can be
someone else's problem, and default to `mv -n --`.
