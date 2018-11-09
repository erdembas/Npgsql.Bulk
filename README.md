# Npgsql.Bulk
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Ferdembas%2FNpgsql.Bulk.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Ferdembas%2FNpgsql.Bulk?ref=badge_shield)

Helper for performing COPY (bulk insert and update) operation easily, using Entity Framework + Npgsql. 

.Net 4.5, Standard 1.5 and Standard 2.0 are supported.

```c#
var uploader = new NpgsqlBulkUploader(context);
var data = GetALotOfData();

// To create a lot of objects
uploader.Insert(data);

// To update a lot of objects
uploader.Update(data);
```

For .Net 4.5 BulkSelect operation is implemented which emulates join in-memory table to DB table. Here is the example:

```c#
// We need DbContext first
var context = GetContext();                 

// Obtained a collection of 50k transactions
var transactions = GetListOfTransction();   

// Need to get prices from DB for all transactions, do it with 1 SQL call
var prices = context.Prices.BulkSelect(
    x => new { x.Ticker, x.TradedOn },                      // need to specify key for JOIN
    transactions.Select(x => new { x.Ticker, x.TradedOn })  // obtain key data from local objects
);

// Done, prices variable now contains List<Price> which matches the JOIN DB table to in-memory collection

```

    
Available on NuGet: 
```
Install-Package Npgsql.Bulk
```

More info at: http://tsherlock.tech/2017/10/11/solving-some-problems-with-bulk-operations-in-npgsql/


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Ferdembas%2FNpgsql.Bulk.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Ferdembas%2FNpgsql.Bulk?ref=badge_large)