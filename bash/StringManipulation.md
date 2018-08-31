# String Manipulation

## Truncate

To cut down / truncate a file end forexample use the following:

```bash
subject="Hello World!"

truncated="${subject% *}"

echo "${truncated}" #> "Hello"
```
