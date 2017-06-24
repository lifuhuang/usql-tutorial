# Parameters

The `DECLARE` statement allows us to define parameters to store values for things that aren't RowSets.

We'll start with this snippet

```
@rows = 
  EXTRACT name string, 
           id int
  FROM "/data.csv"
  USING Extractors.Csv();
```

`DECLARE` can assign constant values to a name. In this case we can assign the input file to a parameter.

```
DECLARE @inputfile string = "/data.csv"

@rows = 
  EXTRACT name string, 
          id int
  FROM @inputfile
  USING Extractors.Csv();
```

## Supported Datatypes with DECLARE

**Text**

```
DECLARE @text1 string = "Hello World";
DECLARE @text2 string = @"Hello World";

DECLARE @text3 char   = 'a';
```

**DateTimes**

```
DECLARE @d1 DateTime = System.DateTime.Parse("1979/03/31");
DECLARE @d2 DateTime= DateTime.Now;
```

**Signed numerics**

```
DECLARE @a sbyte  = 0;
DECLARE @b short  = 1;

DECLARE @c int    = 2;
DECLARE @d long   = 3L;

DECLARE @e float  = 4.0f;
DECLARE @f double = 5.0;
```

**Unsigned numerics**

```
DECLARE @g byte   = 0;
DECLARE @h ushort = 1;
DECLARE @i uint   = 2;
DECLARE @j ulong  = 3L;
```

** Arrays **


```
DECLARE @array1 byte [] = new byte[] { 0, 1, 2, 3, 4 };
DECLARE @array2 string [] = new string[] { "foo", "bar", "beer" };
```

**Miscellaneous**

```
DECLARE @misc1 bool    = true;
DECLARE @misc2 Guid    = System.Guid.Parse("BEF7A4E8-F583-4804-9711-7E608215EBA6");
```


## Using Expressions

DECLARE parameters can be expressions

```
DECLARE @a string = "BEGIN" + @text1 + "END";
DECLARE @b string = string.Format("BEGIN{0}END", @text1);
DECLARE @c string = MyHelper.GetMyName();
```


## Parameter values cannot be assigned directly from a RowSet.

Values cannot be assigned from a RowSet to a DECLARE parameter.

```
// This does NOT work
DECLARE @maxval int = SELECT MAX(value) FROM data;
```

An alternative is to get a single-row RowSet with a single column and then JOIN that single-row RowSet other RowSet to get what you need.
