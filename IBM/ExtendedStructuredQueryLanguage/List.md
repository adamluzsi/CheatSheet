# eSQL List

## create new element

```sql
DECLARE NewElement REFERENCE TO OutputRoot.XMLNSC.path.to.new.targetNamespace:element;

CREATE LASTCHILD OF OutputRoot.XMLNSC.path.to.new AS NewElement
	TYPE XMLNSC.Folder
	NAMESPACE targetNamespace -- namespace if it's required, this is here only for the sake of example
	NAME 'element'; -- same as the end of the "OutputRoot.XMLNSC.path.to.new.element"

SET NewElement.Key = 'Value';
```

## Index accessing

```sql
DECLARE cursor REFERENCE TO ReferencePath.Element[1]; -- returns the list of "Element" first value (index 0)
```

## Iterate List

```sql
DECLARE cursor REFERENCE TO ReferencePath.Element[1];

WHILE LASTMOVE(cursor) DO -- iterator.next() => boolean

	DECLARE ChildElement REFERENCE TO CALCULATE_AGE(cursor.ChildElement); -- retruns the ChildElement Reference

	-- do something useful with the cursor or with it's child elements

	MOVE cursor NEXTSIBLING REPEAT TYPE NAME; -- set cursor to the next element with the same name that the cursor has.
	                                          -- in this case, this is "Element"
END WHILE;
```

