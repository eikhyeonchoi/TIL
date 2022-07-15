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
GET /_cluster/health?pretty=true
```

## Read
```
# 모든 인덱스 조회
GET /_cat/indices?v

# index의 모든 document 조회
GET /{indexName}/_search?pretty

# index -> type의 모든 document 조회
GET /{indexName}/{typeName}/_search?pretty

# 출력할 필드 선택하기
GET /_search
{
  "fields" : ["user", "created_at"],
  "query" : {
    "term" : {"title" : "민주노총"}
  }
}

# _id를 이용해 조회
GET /{indexName}/{typeName}/1?pretty

# eqaul 조건 넣기
GET /{indexName}/_search?pretty
{"query":{"match":{"name":"dog"}}}

# LIKE 조건 넣기
GET /{indexName}/_search?pretty
{"query":{"query_string":{"query":"*name*"}}}}
```

## Write
```
PUT /{indexName}/{typeName}/{_id}?pretty
{
	"name": "document"
}

# customer index 추가 -> DB 추가
PUT /{indexName}?pretty

# Index가 없어도 자동으로 추가해버림 + _id를 지정하지 않는다면 임의의 문자열로 넣음
POST /{indexName}/{typeName}?pretty
{"name": "nametest"}

# _id 추가하는 경우
POST /{indexName}/{typeName}/1?pretty
{"name": "nametest2"}
```

# Delete
```
# index 삭제
DELETE /{indexName}

# query로 삭제
POST /{indexName}/_delete_by_query
{
  "query": {
    "match": {
      "user.id": "elkbee"
    }
  }
}
```

# Bulk API
```
# 줄바꿈 2번 해줘야 에러가 나지않음
POST _bulk
{"index":{"_index":"indexName", "_type":"typeName" "_id":"1"}}
{"field1":"value one", "field2":"value two" ... }


```
