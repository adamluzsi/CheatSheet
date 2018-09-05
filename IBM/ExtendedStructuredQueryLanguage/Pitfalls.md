# Pitfalls

## IS NOT NULL

when you want to check an element is equal to null, so in other means it's there or not, don't use IS NOT NULL.
It will return true if the element is there but the field value is an empty string.
This is bothersome for complex types where you only use it as a structure and not as a element with field value.

use Exists instead with empty selector.

```sql

IF EXISTS(reference.NameSpace:ElementName[]) THEN
   -- ...
END IF;
```
