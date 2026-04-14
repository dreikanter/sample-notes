# Linux file permissions reference

The part I always have to mentally decode.

## Reading permissions

```
-rwxr-xr--  1  user  group  4096  Oct 11  file.txt
```

Field 1: `-` (file) or `d` (directory) or `l` (symlink)
Fields 2-4: owner permissions (r/w/x)
Fields 5-7: group permissions
Fields 8-10: other permissions

```
r = read    (4)
w = write   (2)
x = execute (1)
```

## Common permission combinations

| Octal | Symbolic | Meaning |
|-------|----------|---------|
| 644   | rw-r--r-- | typical file |
| 755   | rwxr-xr-x | executable / directory |
| 700   | rwx------ | private executable |
| 600   | rw------- | private file (e.g., SSH key) |
| 777   | rwxrwxrwx | avoid unless necessary |

## chmod

```bash
chmod 644 file.txt         # numeric
chmod u+x script.sh        # symbolic: add execute for owner
chmod go-w sensitive.txt   # symbolic: remove write for group and other
chmod -R 755 directory/    # recursive
```

## chown and chgrp

```bash
chown user:group file.txt
chown -R www-data:www-data /var/www/
```

## Special bits

- **Setuid (4000):** Execute as file owner: `chmod u+s binary`
- **Setgid (2000):** On directories, new files inherit group: `chmod g+s dir/`
- **Sticky bit (1000):** Users can only delete their own files in shared dirs: `chmod +t /tmp`

## umask

Default permissions for new files are `666 - umask` for files, `777 - umask` for directories. Typical umask is `022`, yielding `644` and `755` respectively.

Reference: [GNU coreutils chmod documentation](https://www.gnu.org/software/coreutils/manual/)

