# FileSets

We've seen that you can explicitly list all the files in the EXTRACT statement. In some cases, there might be a large number of files, so you may not want to list all the files manually every time.

**FileSets** make it easy to define a pattern to identify a set of files to read.

In the simplest case let's get all the files in a folder.

```
@rs =
    EXTRACT 
        user   string,
        id     string,
    FROM 
        "/input/{*}"
    USING Extractors.Csv();
```

### Specifying a list of files that have an extension

This is very simple modification syntax. The example uses will extract all the files that end with ".csv".

```
@rs =
    EXTRACT 
        user   string,
        id     string,
    FROM 
        "/input/{*}.csv"
    USING Extractors.Csv();
```

### Getting filenames as a column in the RowSet

Because we are reading rows from multiple files. it is convenient to for the rows to have some information about the filename it came from. We can adjust the query slightly to make this possible.

```
@rs =
    EXTRACT 
        user       string,
        id         string,
        __filename string
    FROM 
        "/input/{__filename}"
    USING Extractors.Csv();
```

You are probably wondering about the `__` in the column `__filename`. It isn't necessary at all, however it is useful as a way of marking that this information came from the process of extracting the file, not from the data in the file itself.

To emphasize that the naming of `__filename` and the use of the `__` prefix was completely arbitrary below is the same script with a different name (`foo`).

```
@rs =
    EXTRACT 
        user string,
        id   string,
        foo  string
    FROM 
        "/input/{foo}"
    USING Extractors.Csv();
```


### Getting parts of a filename as a column in the RowSet

Instead of the full filename, we can also get part of the filename. The sample below shows how to get just the number part.

```
@rs =
    EXTRACT 
        user       string,
        id         string,
        __filenum  int
    FROM 
        "/input/data{__filenum}.csv"
    USING Extractors.Csv();
```


### Notes

* The schemas for all the files in the FileSet must match the schema specified in the extract.
* The more files there are in the FileSet the longer the compilation time will take.




