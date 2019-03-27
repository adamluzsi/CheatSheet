# SQL

## Prepare statement

in prepare statements query you have to use ${number} instead of question marks "?",
because else it result in an syntax error.

example with lib/pg
```golang

const query = `
INSERT INTO "table_name" (column_name)
VALUES ($1);
`

func prep(db *sql.DB, tb testing.TB) {

	statement, err := db.Prepare(query)
	require.Nil(tb, err)
	defer func() { require.Nil(tb, statement.Close()) }()

	if _, err := statement.Exec("some value here"); err != nil {
		tb.Fatal(err.Error())
	}

}
```
