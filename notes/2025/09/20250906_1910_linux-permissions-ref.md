---
title: Linux file permissions — the bits I always re-look up
slug: linux-permissions-ref
tags: [linux, sysadmin, reference]
---

# Linux file permissions — the bits I always re-look up

**The permission model**

Each file has three permission sets: owner (u), group (g), other (o). Each set has three bits: read (r=4), write (w=2), execute (x=1).

`chmod 755 file` means:
- Owner: 7 = 4+2+1 = rwx
- Group: 5 = 4+0+1 = r-x
- Other: 5 = 4+0+1 = r-x

**Symbolic notation**

```bash
chmod u+x file       # add execute for owner
chmod go-w file      # remove write from group and other
chmod a+r file       # add read for all (a = ugo)
chmod u=rw,go=r file # set exact permissions
```

**setuid, setgid, sticky bit**

These are the special bits (the leading digit in 4-digit chmod):

`chmod 4755` — setuid (4): executable runs with the owner's permissions, not the caller's. Used by `sudo`, `passwd`.

`chmod 2755` — setgid (2): for directories, new files inherit the group. Useful for shared project directories.

`chmod 1777` — sticky bit (1): only the file owner or root can delete/rename files in a directory. Used on `/tmp`.

**Default permissions and umask**

`umask` controls what's subtracted from the default permissions (666 for files, 777 for directories). A umask of `022` means files get 644 (666 minus 022) and directories get 755.

**Finding files with specific permissions**

```bash
find /path -perm -4000  # setuid files (security audit)
find /path -perm /o+w   # world-writable files
```

**ACLs for more fine-grained control**

When owner/group/other isn't enough: `setfacl -m u:alice:rw file` to give specific users access.

Linux permissions guide: https://www.linuxfoundation.org/blog/classic-sysadmin-understanding-linux-file-permissions
