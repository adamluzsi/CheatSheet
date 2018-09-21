# User Defined Properties

If a value is configured in the "User defined properties tab" for a given message flow, than you can access that value from eSQL as follows:

For example if you have a string value with the following name: *HELL_WORLD_VALUE*

```sql
CREATE COMPUTE MODULE MessageFlowName_MyComputeNode

	DECLARE HELL_WORLD_VALUE EXTERNAL CHARACTER;

	CREATE FUNCTION Main() RETURNS BOOLEAN BEGIN
        RETURN TRUE;
	END;

END MODULE;
```
