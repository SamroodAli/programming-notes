# Terminal less commands

| Command  | Instruction  |
|---|---|
| <Ctrl+f>,space  | move forward to forward |
| <Ctrl+b>  | move backward  |
|/keyword | To search forward|
|?keyword| To search backward|
|n| next match in search |
|Shift n| previous match in search|
|g| To go the first line|
|G| To to the last line|

There are two types of commands

1. Executable files on disk -> Documentation is using `man`
```bash
help cd
```
2. Shell built in. -> Documentation is using `help`

```bash
man ls
```

Use the `type` command to know the type of the command

```bash
type cd
```

## Note: Both command's help doc is available with --help flag.
`--help is available on both shell builtins and executables`

## Man -k flag
To search in all man pages
```bash
man -k something
man -k 'copy files'
```

This is the same as the `apropos` command.
```bash
apropos uname
```

## System information using uname
```bash
uname -a
```
