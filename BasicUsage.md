# Basic usage #

First we have to map the Solr document to a class (Solr supports only one document type per instance at the moment). Let's use a subset of the default schema that comes with the Solr distribution:

```
public class TestDocument : ISolrDocument {
    private ICollection<string> cat;
    private ICollection<string> features;
    private string id;
    private bool inStock;
    private string manu;
    private string name;
    private int popularity;
    private double price;
    private string sku;

    [SolrField("cat")]
    public ICollection<string> Cat {
        get { return cat; }
        set { cat = value; }
    }

    [SolrField("features")]
    public ICollection<string> Features {
        get { return features; }
        set { features = value; }
    }

    [SolrUniqueKey]
    [SolrField("id")]
    public string Id {
        get { return id; }
        set { id = value; }
    }

    [SolrField("inStock")]
    public bool InStock {
        get { return inStock; }
        set { inStock = value; }
    }

    [SolrField("manu")]
    public string Manu {
        get { return manu; }
        set { manu = value; }
    }

    [SolrField("name")]
    public string Name {
        get { return name; }
        set { name = value; }
    }

    [SolrField("popularity")]
    public int Popularity {
        get { return popularity; }
        set { popularity = value; }
    }

    [SolrField("price")]
    public double Price {
        get { return price; }
        set { price = value; }
    }

    [SolrField("sku")]
    public string Sku {
        get { return sku; }
        set { sku = value; }
    } 
}
```

It's just a POCO with a marker interface (ISolrDocument) and some attributes: SolrField maps the attribute to a Solr field and SolrUniqueKey (optional) maps an attribute to a Solr unique key field. Let's add a document (make sure you have a running Solr instance first):

```
[Test]
public void AddOne() {
    ISolrOperations<TestDocument> solr = new SolrServer<TestDocument>("http://localhost:8983/solr");
    TestDocument doc = new TestDocument();
    doc.Id = "123456";
    doc.Name = "some name";
    doc.Cat = new string[] {"cat1", "cat2"};
    solr.Add(doc);
    solr.Commit();
}
```

Let's see if the document is there:

```
[Test]
public void QueryAll() {
    ISolrOperations<TestDocument> solr = new SolrServer<TestDocument>("http://localhost:8983/solr");
    ISolrQueryResults<TestDocument> r = solr.Query("*:*");
    Assert.AreEqual("123456", r[0].Id);
}
```

For more examples, see the [tests](http://solrnet.googlecode.com/svn/trunk/SolrNet.Tests/SolrOperationsTests.cs).