# How to create checks

## Simple check expression

To define a simple check expression, the only required argument is `command`.
But we usually need to pass a few arguments to be tested. The most important are `arg_values` and `arg_column`.
Which values ​​are tested against the column under test or which other column is tested against the column under test, respectively.

```py title="getting_started.py" linenums="29" hl_lines="3-4"
--8<-- "notebooks/getting_started.py:29:33"
```
In human-readable text: `check if a given column is greater than or equal to 0`.

However, you can also define custom `name`, `error_level` and `error_msg` that will be further parsed by ErrorCollector.

```
name: <string or empty>
error_level: <empty or one of: 'warning', 'error', 'critical'>
error_msg: <string or empty>
command: <string or a function*>
subject: <empty or a list with column names*>
arg_values: <empty or a list of args>
arg_columns: <empty or a list of column names>

```

### command

It can be one of:

```
'is_equal_to',
'is_equal_to_or_both_missing',
'is_greater_than_or_equal_to',
'is_greater_than',
'is_less_than_or_equal_to',
'is_less_than',
'is_not_equal_to',
'is_not_equal_to_and_not_both_missing',
'is_unique',
'is_duplicated',
'is_in',
'is_null',
'is_not_null'
```

or a user-defined function.

### Tailor-made check functions

Users can also define their own verification functions. The only requirement is to follow the same signature pattern below.

`data` always receives an object that has a `.lazyframe` (a polar `LazyFrame`) and `.key`, which is the name of the column to be validated.

Finally, it must return a polar `LazyFrame` with a binary column.

```py title="getting_started.py" linenums="4" hl_lines="1"
--8<-- "notebooks/checks.py:4:9"
```

Let's use the above function to perform the same check we did before for the `age` column. We'll also use other fields to understand how they modify the report output.


```py title="getting_started.py" linenums="9" hl_lines="12-16 50-52"
--8<-- "notebooks/checks.py:9"

#{
#  "error_reports": [
#    {
#      "name": "Age must be not null, grater than or equal to 0 and less than 150",
#      "errors": [
#        {
#          "type": "SchemaErrorReason.SERIES_CONTAINS_NULLS",
#          "message": "non-nullable column 'age' contains null values",
#          "level": "error",
#          "title": "Not_Nullable",
#          "traceback": null
#        },
#        {
#          "type": "SchemaErrorReason.DATAFRAME_CHECK",
#          "message": "Column 'age' failed validator number 0: <Check warning: Age must be between 0 (inclusive) and 150 (exclusive)> failure case examples: [{'age': -5}, {'age': 150}]",
#          "level": "warning",
#          "title": "Tailor-made function check: is_between",
#          "traceback": null
#        }
#      ],
#      "total_errors": 2,
#      "id": "1351fbab-6dbd-424c-a349-74d601518d5c"
#    }
#  ],
#  "exceptions": []
#}
```
Instead of creating two separate checks, we implemented our own function as a single test. Notice that instead of **3** errors being collected, we now only have **2**, meaning that even though the same validations are performed, the report output is different.


## Complex check expression

## DF level expressions
