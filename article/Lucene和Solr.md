# Lucene 和 Solr 指的是什么

## 📌 [Lucene](ca://s?q=Lucene是什么)
- **Lucene** 是 Apache 提供的一个 **高性能全文搜索引擎库**。  
- 它是一个 **Java 编写的开源工具包**，提供索引和搜索功能，但本身不是完整的搜索引擎。  
- 特点：  
  - 提供倒排索引结构，适合处理大规模文本数据。  
  - 支持全文检索、模糊查询、通配符查询等。  
  - 高度可扩展，常作为其他搜索系统的底层核心。  

## 📌 [Solr](ca://s?q=Solr是什么)
- **Solr** 是基于 Lucene 构建的 **企业级搜索服务器**。  
- 提供 REST 风格的 API，支持分布式搜索和索引。  
- 特点：  
  - 内置 **HTTP 接口**，易于与各种应用集成。  
  - 支持 **分布式索引与查询**，适合大规模数据场景。  
  - 提供丰富功能，如高亮显示、分面搜索、排序、缓存等。  
  - 常用于电商网站、日志分析、企业搜索等场景。  

## 🔎 核心区别

| 特性 | **[Lucene](ca://s?q=Lucene特点)** | **[Solr](ca://s?q=Solr特点)** |
|------|--------------------------------|--------------------------------|
| **定位** | 库（Library），需要开发者集成 | 搜索服务器（Server），开箱即用 |
| **接口** | Java API | REST/HTTP API |
| **功能** | 提供索引与搜索的底层能力 | 提供完整的搜索平台功能 |
| **扩展性** | 高度灵活，但需自行开发 | 内置分布式支持，功能丰富 |
| **适用场景** | 作为搜索引擎的核心组件 | 企业级搜索解决方案 |

## 📊 使用示例

### Lucene 创建索引
```java
IndexWriter writer = new IndexWriter(directory, config);
Document doc = new Document();
doc.add(new TextField("title", "Lucene 入门", Field.Store.YES));
```
### Solr 查询接口
```http
http://localhost:8983/solr/core1/select?q=title:Lucene
```
返回 JSON 格式的搜索结果。

## 📊 总结
Lucene → 搜索引擎的底层库，提供索引和搜索能力。

Solr → 基于 Lucene 的搜索服务器，提供完整的企业级搜索解决方案。

关系可以理解为：Lucene 是内核，Solr 是包装好的产品。


