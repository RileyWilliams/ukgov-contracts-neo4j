# ukgov-contracts-neo4j
Exploration of UK Gov Open Contracts data using Neo4j


# Setup

```
LOAD CSV WITH HEADERS FROM 'https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/930598/FINAL_Aug_2020_Over__25k_Spend_Data_to_be_Published_.csv'  AS row
WITH row WHERE row.Supplier IS NOT NULL AND row.Entity is not null AND row.`PO Line Description ` is not null

MERGE (s:Supplier {name: row.Supplier, type: row.`Supplier Type`})

MERGE (e:Expense {name: row.`Expense Type`})

MERGE (en:Entity {name: row.Entity})

MERGE (i:Item {name: row.`PO Line Description `, amount:row.Amount})-[:EXPENSE]->(e)

MERGE (en)-[r:HAS_PURCHASED]->(i)<-[:SUPPLIED]-(s) SET r.date = row.Date
```
