# SynthetikDB – An Embedded Vector-Enhanced NoSQL Database for AI Agents


**SynthetikDB** is an intelligent, embeddable NoSQL document database with native vector similarity search. It's designed for modern .NET applications that require fast, local, and semantically-aware data access without spinning up servers or managing infrastructure.

✨ **Built for intelligent agents. Optimized for in-process vector search. Designed for the future of cognitive software.**

---

## 🧬 Origins

SynthetikDB began as an extension to [LiteDB](https://github.com/mbdavid/LiteDB), an embedded NoSQL document store for .NET. I needed a semantic memory layer for my AI framework ([SynthetikDelusion](https://github.com/hurley451/synthetikdelusion))—something that could search over vector embeddings, locally, at speed.

Existing solutions like Pinecone or Weaviate were server-based. SQLite extensions were promising, but didn’t fit well with C#'s type system or LINQ model. So I built what I needed—vector intelligence for a document store, fully in-process.

SynthetikDB retains all the power of LiteDB, while introducing intelligent features for vector-based retrieval, memory modeling, and embedded AI agents.

>This project preserves and celebrates the excellence of the original [LiteDB](https://github.com/mbdavid/LiteDB) by Mauricio David. All changes comply with the MIT License and retain original attribution.

---

## 🚀 What Makes SynthetikDB Different

### 🧠 Vector Support for Semantic Memory
- **BsonVector Type** – First-class support for `float[]` embeddings stored in BSON
- **VECTOR_SIM()** – Native cosine similarity operator usable in expressions and queries
- **.WhereNear() / .TopK()** – LINQ-integrated methods for nearest-neighbor and top-k search
- Smart auto-casting between arrays and vectors for ergonomic use

### ⚡ Fully In-Process Execution
- No servers, RPC, or infrastructure
- Ideal for desktop apps, edge devices, LLM toolchains, and game engines

### 🧪 Benchmark-Grade Performance
- Vector search in microseconds, not milliseconds
- Outperforms remote vector DBs like Pinecone for local workloads
- Tight integration with C# type system, LINQ expressions, and LiteDB’s expression compiler

---

## ✨ Key Features

- 🧮 **BsonVector**: Native vector embedding support (`float[]`)
- 📏 **Cosine Similarity**: Efficient similarity filtering and ranking via `VECTOR_SIM()`
- 🔍 **Top-K & Distance-Based Queries**:
  - `.WhereNear(field, target, maxDistance)`
  - `.TopKNear(field, target, k)`
- 🧪 **Fluent and SQL Support**: Both LINQ-like and SQL-style queries supported
- ⚡ **In-Process Performance**: Sub-millisecond filtering on thousands of records
- 🔒 Fully compatible with secured + encrypted LiteDB files
- ✅ **Works anywhere LiteDB works** – no server, no extra dependencies

---

## 🧠 Use Cases

- Local semantic search for small/medium knowledgebases
- In-process Retrieval-Augmented Generation (RAG)
- Cognitive memory modeling in AI agents
- Lightweight alternatives to vector databases like Pinecone or Qdrant

---

## Installation & Examples

To install, simply add the nuget package:
```bash
dotnet add package SynthetikDB
```

Or clone the repo and reference the project directly.


### Quick Examples

```csharp
using var db = new SynthetikDatabase("mem.db");
var col = db.GetCollection("vectors");

// Insert some vectorized documents
col.Insert(new BsonDocument {
    ["_id"] = 1,
    ["Embedding"] = new BsonVector(new float[] { 1.0f, 0.0f })
});

// Query for nearby embeddings
var queryEmbedding = new float[] { 1.0f, 0.0f };
var results = col.Query()
    .WhereNear(doc => doc["Embedding"], queryEmbedding, maxDistance: 0.3)
    .ToList();
```


### Vector Search with POCO Class

You can use vector similarity search with your own classes, enabling strongly typed access to documents with embedded float arrays.

```csharp
using LiteDB;

// Your POCO model
public class MyDocument
{
    [BsonId]
    public int Id { get; set; }

    public float[] Embedding { get; set; }
}

// Open a LiteDB instance (in-memory or file-backed)
using var db = new SynthetikDatabase("MyData.db");

// Get typed collection
var col = db.GetCollection<MyDocument>("mydocs");

// Insert some example vectorized documents
col.Insert(new MyDocument { Id = 1, Embedding = new float[] { 1.0f, 0.0f } });
col.Insert(new MyDocument { Id = 2, Embedding = new float[] { 0.0f, 1.0f } });
col.Insert(new MyDocument { Id = 3, Embedding = new float[] { 1.0f, 1.0f } });

// Query for documents near the target vector
var target = new float[] { 1.0f, 0.0f };

var results = col
    .Query()
    .WhereNear(x => x.Embedding, target, maxDistance: 0.5)
    .ToList();

// or TopK
var topResults = col
    .Query()
    .TopKNear(x => x.Embedding, target, 2)
    .ToList();
```

### Or in SQL:

```sql
SELECT *
FROM vectors
WHERE VECTOR_SIM($.Embedding, [1.0, 0.0]) < 0.3
```

---

## 📊 Performance

Benchmarks show microsecond-level response times for Top-K and filtered nearest vector queries:

| Dataset Size | Method           | Mean Time (μs) |
|--------------|------------------|----------------|
| 100          | `WhereNear`      | ~270 μs        |
| 1000         | `TopKNearLimit`  | ~7 ms          |
| 10,000       | `TopKNearLimit`  | ~84 ms         |

> See full benchmark results [here](docs/performance)

---

## 🧩 Integration Notes

- No background indexing or ANN required
- Works with `float[]`, `BsonVector`, or JSON `[1.0, 2.0, 3.0]`
- Embeddings can be stored in any document field
- Compatible with LINQ and SQL

---

## 🧬 License & Attribution

This software is licensed under the [MIT License](https://opensource.org/licenses/MIT). Significant portions of the underlying engine are based on LiteDB and we honor and acknowledge the foundational work of [Maurício David (@mbdavid)](https://github.com/mbdavid).

---

🔍 Getting Help

Open an issue for feature requests or improvements. 

---

## 🔍 See Also

- [SynthetikDelusion Cognitive Agent Framework](https://github.com/hurley451/synthetikdelusion)
