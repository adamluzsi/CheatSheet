# Create

## empty element by path

this expression will create a the complex elements
and namespaces and everything than set last element value to NULL

```sql
SET consumer.cembra:MinimumAmountForLiving VALUE = NULL;
```

## Create a new last/first Child to something

```sql
DECLARE NewElement REFERENCE TO OutputRoot.XMLNSC.path.to.new.targetNamespace:element;

CREATE LASTCHILD OF OutputRoot.XMLNSC.path.to.new AS NewElement
	TYPE XMLNSC.Folder
	NAMESPACE targetNamespace -- namespace if it's required, this is here only for the sake of example
	NAME 'element'; -- same as the end of the "OutputRoot.XMLNSC.path.to.new.element"

SET NewElement.Key = 'Value';
```

