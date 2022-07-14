# About ElasticSearch

## Elasticsearch to RDBMS
```
Database = index;
Table = Type;
Row = Document;
Column = Field;
Index = Analyze;
Primary key = _id;
Schema = Mapping;
Physical partition = Shard;
Logical partition = Route;
Relational = Parent/Child, Nested;
SQL = Query DSL
```

## Cluster 정보 확인
```
curl -XGET http://localhost:9200/_cluster/health?pretty=true
```

## Read
```
# 모든 인덱스 조회
curl -XGET 'localhost:9200/_cat/indices?v'

# index의 모든 document 조회
curl -XGET 'localhost:9200/customer/_search?pretty'

# index -> type의 모든 document 조회
curl -XGET 'localhost:9200/customer/test/_search?pretty'

# _id를 이용해 조회
curl -XGET 'localhost:9200/customer/test/1?pretty'

# eqaul 조건 넣기
curl -XGET 'localhost:9200/customer/_search?pretty' -H 'Content-Type: application/json' -d '{"query":{"match":{"name":"dog"}}}'

# LIKE 조건 넣기
curl -XGET 'localhost:9200/customer/_search?pretty' -H 'Content-Type: application/json' -d '{"query":{"query_string":{"query":"*name*"}}}}'
```

## Write Update Delete
```
put /{indexName}/{typeName}/{_id}?pretty
{
	"name": "document"
}
# customer index 추가 -> DB 추가
curl -XPUT 'localhost:9200/customer?pretty'

# Index가 없어도 자동으로 추가해버림 + _id를 지정하지 않는다면 임의의 문자열로 넣음
curl -XPOST 'localhost:9200/customer/test?pretty' -H 'Content-Type: application/json' -d '{"name": "nametest"}'

# _id 추가하는 경우
curl -XPOST 'localhost:9200/customer/test/1?pretty' -H 'Content-Type: application/json' -d '{"name": "nametest2"}'
```