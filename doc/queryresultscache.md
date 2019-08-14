<#ifdef MICROSOFT># Query Results Cache (Experimental)

Kusto can be instructed to store query results in a designated cache.

For cache purposes, two queries will be considered identical if:

1. They share the same UTF8 representation and

2. They share the same database and

3. They share the same [client request properties](../api/netfx/request-properties.md), excluding the following properties:
   - [ClientRequestId](../api/netfx/request-properties.md#the-clientrequestid-x-ms-client-request-id-named-property)
   - [Application](../api/netfx/request-properties.md#the-application-x-ms-app-named-property)
   - [User](../api/netfx/request-properties.md#the-user-x-ms-user-named-property)

The cache can be utilized by including the `query_results_cache_max_age` option:

<!-- csl: https://demo12.westus.kusto.windows.net/GitHub -->

```
set query_results_cache_max_age = time(5m);
GithubEvent
| where CreatedAt > ago(180d)
| summarize arg_max(CreatedAt, Type) by Id
```

The option value specifies the cached result max age, measured from the query execution start time.
If a cached result which satisfies the time constraint could not be found, the query will be executed and the
query results will be cached if:

1. The query execution wasn't cancelled.

2. The query execution completed without raising any runtime failures (e.g. because the cluster was low on memory).

The query results cache is currently disabled by default and a [support ticket](https://aka.ms/kustosupport) should be opened to enable it.
<#endif>