# File Permission Bits

| ls -l | N | Allow to                 |
|-------|---|--------------------------|
| rwx   | 7 | Read, write and execute  |
| rw-   | 6 | Read, write              |
| r-x   | 5 | Read, and execute        |
| r--   | 4 | Read,                    |
| -wx   | 3 | Write and execute        |
| -w-   | 2 | Write                    |
| --x   | 1 | Execute                  |
| ---   | 0 | no permissions           |

| Permission | Octal| Field |
|------------|------|-------|
| rwx------  | 0700 | User  |
| ---rwx---  | 0070 | Group |
| ------rwx  | 0007 | Other |