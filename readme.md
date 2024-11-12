# Vector Database Client

This library provides a Python client for interacting with Qdrant, a vector search database, to store, retrieve, and search high-dimensional vectors. It supports collection management, adding vectors with optional payloads, retrieving vectors by ID or filters, and searching for similar vectors based on distance metrics.

---

### Class and Method Overview

- **Qdrant**
- **Faiss** *(Work in Progress)*
- **Milvus** *(Work in Progress)*

**Initialization**:
```python
Qdrant(host: str = "localhost", port: int = 6333)
```
Initializes a connection to the Qdrant database.

#### Methods

- **try_create_collection**: Create a collection if it does not already exist.
  ```python
  try_create_collection(collection_name: str, dimension: int, distance: Distance)
  ```

- **add**: Adds vectors with optional payloads to a collection.
  ```python
  add(collection_name: str, vectors: List[List[float]], payloads: List[Dict[str, Any]] = None)
  ```

- **get**: Retrieve vectors by ID, list of IDs, or with filters.
  ```python
  get(collection_name: str, id_or_filter: Union[dict, list, str], top_k=5)
  ```

- **search**: Searches for similar vectors to the query vector.
  ```python
  search(collection_name: str, query: Union[np.ndarray, List[np.ndarray]], top_k: int = 5, filter: dict = None)
  ```

- **update**: Updates a vector by ID with a new vector and/or payload.
  ```python
  update(collection_name: str, id: str, vector: np.ndarray, payload: Dict[str, Any] = None)
  ```

- **delete**: Deletes a vector by ID.
  ```python
  delete(collection_name: str, id)
  ```

- **delete_collection**: Deletes an entire collection by name.
  ```python
  delete_collection(collection_name: str)
  ```

---

### Examples

#### Creating a Collection
```python
qdrant.try_create_collection("example_collection", dimension=128, distance=Distance.COSINE)
```

#### Adding Vectors
```python
vectors = [[0.1, 0.2, 0.3] * 128]  # Example vector
payloads = [{"name": "example1"}]
qdrant.add("example_collection", vectors, payloads)
```

#### Retrieving a Vector by ID
```python
result = qdrant.get("example_collection", "unique-vector-id")
print(result)
```

#### Searching for Similar Vectors
```python
query_vector = np.random.rand(128)
search_results = qdrant.search("example_collection", query_vector, top_k=5)
print(search_results)
```

#### Updating a Vector
```python
qdrant.update("example_collection", "unique-vector-id", vector=np.random.rand(128), payload={"updated": True})
```

#### Deleting a Vector
```python
qdrant.delete("example_collection", "unique-vector-id")
```

#### Deleting a Collection
```python
qdrant.delete_collection("example_collection")
```

### Error Handling and Logging

- **Logging**: Operations (e.g., creating a collection, adding vectors) log actions and errors to help monitor client activity.
- **Exception Handling**: Handles exceptions in methods like `delete` to manage potential errors, with logging for details.