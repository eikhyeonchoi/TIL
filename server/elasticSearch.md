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
GET /{indexName}/{typeName}/_search?pretty
{"query": {"wildcard": {"field_name": "*부자*"}}}

# bool 조건으로 여러 조건 걸기
조건문인 불 조합으로 적용해서 최종 검색 결과를 찾아낼 수 있습니다
1. must - 반드시 매칭되는 조건 score에 영향을 준다
2. filter - must와 동일하지만 score 영향을 주지 않음
3. should -  bool 쿼리가 query context에 있고 must 또는 filter 절이 있다면, should 쿼리와 일치하는 결과가 없더라도 매치가 된다. bool 쿼리가 filter context 안에 있거나, must 또는 filter 중에 하나라도 있는 경우에만 매칭된다. minimum_should_match 이 값을 지정해서 컨트롤할 수 있다.
4. must_not - 이 쿼리와 매칭되지 않아야 한다

예시
{
"query": {
    "bool": {
        "should": [
            {
                "wildcard": {
                    "adress": "*Seoul*"
                }
            },
            {
                "match": {
                    "adress": "Seoul"
                }
            }
        ],
        "must": {
            "match": {
                "age": 22
            }
        },
        "filter": {
            "range": {
                "time": {
                    "gte": "2020-12-10 00:00:00",
                    "lte": "2020-12-11 23:59:59",
                    "format": "yyyy-MM-dd HH:mm:ss"
                }
            }
        }
    }
}
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

# 파일로 추가
$ curl -X POST 'localhost:9200/_bulk?pretty' -H 'Content-Type: application/x-ndjson' --data-binary @query.json
파일 예시
{"index":{"_index":"indexName","_type":"typeName","_id":"pk"}}
{"field_name1": "name", "field_name2": "name2" ... }
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
