# Linux file permissions — quick reference

Keep re-looking this up. Writing it down for real this time.

## The permission bits

```
-rwxrw-r--   1   user group  4096  Feb 17  filename
 |||||||||||
 ||||||| |--> other: r-- = read only
 |||| |--> group: rw- = read and write
 | |--> owner: rwx = read, write, execute
 |--> file type (- = file, d = directory, l = symlink)
```

`r` = 4, `w` = 2, `x` = 1. So `rwx` = 7, `rw-` = 6, `r--` = 4.

`chmod 755 file` → owner: rwx (7), group: r-x (5), other: r-x (5)  
`chmod 644 file` → owner: rw- (6), group: r-- (4), other: r-- (4)  
`chmod 600 file` → owner: rw- (6), group: --- (0), other: --- (0) — for private keys

## Common commands

```bash
chmod 755 script.sh              # make executable, readable by all
chmod -R 755 directory/          # recursive
chmod u+x,g-w file               # symbolic: add execute for user, remove write for group
chown user:group file            # change owner and group
chown -R user:group directory/   # recursive
```

## Directory permissions

Execute permission on a directory means you can `cd` into it and access files by name. Without it, `ls` may work but `ls -l` fails. This is a non-obvious permission behavior.

`chmod 700 ~/.ssh` — the SSH directory should not be readable by group or other.

## Special bits

```
SUID (4000): executable runs as file owner, not the user running it
SGID (2000): new files in directory inherit group
Sticky (1000): only file owner can delete (used on /tmp)
```

`chmod 1777 /tmp` is the sticky bit pattern.

Reference: [Linux File Permissions Explained](https://www.redhat.com/sysadmin/linux-file-permissions-explained) on the Red Hat sysadmin blog.
