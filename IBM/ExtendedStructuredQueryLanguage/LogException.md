# Log Exceptions

to access the last exception from a recursively wrapped IIB exception list,
use the following snippet.

```sql

	CREATE FUNCTION Main() RETURNS BOOLEAN BEGIN
		LOG EVENT SEVERITY 2 VALUES(getLastExceptionDetail(InputExceptionList));

		RETURN TRUE;
	END;

	CREATE PROCEDURE getLastExceptionDetail(IN InputTree REFERENCE) RETURNS CHAR
	/****************************************************************************
	* A procedure that will get the details of the last exception from a message
	* IN InputTree: The incoming exception list
	* IN messageNumber: The last message numberr.
	* IN messageText: The last message text.
	*****************************************************************************/
	BEGIN

		DECLARE messageText CHAR;
		-- Create a reference to the first child of the exception list
		declare ptrException reference TO InputTree.*[1];
		-- keep looping while the moves to the child of exception list work
		WHILE LASTMOVE(ptrException) DO
			-- store the current values for the error number and text
			IF ptrException.Number IS NOT NULL THEN
				SET messageText = ptrException.Text;
			END IF;
			-- now move to the last child which should be the next exceptionlist
			MOVE ptrException LASTCHILD;
		END WHILE;

		RETURN messageText;

	END;

```
