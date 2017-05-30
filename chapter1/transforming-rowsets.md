# Transforming rowsets

We are going to start with the first script you encountered change it over-and-over to discover the basics of how U-SQL can be used to transform data.

Here is the starting script

```
@searchlog = 
    EXTRACT UserId          int, 
            Start           DateTime, 
            Region          string, 
            Query           string, 
            Duration        int, 
            Urls            string, 
            ClickedUrls     string
    FROM "/SearchLog.tsv"
    USING Extractors.Tsv();

OUTPUT @searchlog 
    TO "/SearchLog_output.tsv"
    USING Outputters.Tsv();
```

## 


