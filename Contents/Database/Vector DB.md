---
created: 2024-01-12T12:20
updated: 2024-01-12T18:26
---
# Data를 벡터 형태로 저장하고 관리하기 위해 특화된 DB

### 원리

ANN(Approximate Nearest Neighbor) 알고리즘(예: Locality-Sensitive Hashing 또는 Product Quantization)과 같은 인덱싱 기술을 사용하여 검색 프로세스를 가속화

### 종류

- Vespa
- Pinecone
- Milvus
- Redis, MongoDB, Cosmos DB, Cassandra &rarr; 벡터 지원 **NoSQL 데이터베이스**
- PostgreSQL

### 장단점

장점
- 고차원 벡터 (단 하나의 숫자로 나타낼 수 없는 양) 데이터의 효율적인 저장, 검색, 유사성 검색

단점
- 이 vector DB가 차지하는 메모리가 큰 편에 속함
- embedding을 해서 indexing 하는 데에 많은 시간이 소요되고, cost가 매우 높은 편.

### Ref
[Vector DB란?](https://velog.io/@tura/vector-databases)
[Vector DB 종류](https://www.linkedin.com/pulse/choosing-vector-database-your-gen-ai-stack-abhinav-srivastava/)
[Vector DB 종류&한계점](https://hotorch.tistory.com/406)