# Indexes in MongoDB and best performance practices

## Optimisation summary

### Always try to use `$elemMatch`

Query with `$elemMatch` is eta ~2x faster than simple string `array.element` notation

### Normalize when you can

Querying `users.clients` array even indexed as `user.client.name` proves to be +1000x slower querying `client.name` from normalized collection. 

Making 2 normalized was  500x - 800x faster than one query on denormalized collection with indexes.

### Test, benchmark, improve

Small performance optimization value is expotentialy proportional to the amount of data. Testing and benchmarking can improve your server performance over 1000x!!!

Note: 

## Benchmark results

Users: 1930000 documents with 73000 matches
Cliets: 2000000 documents with 1 match
Sessions: 1000000 documents with 1 match

### Finding user clients on one collection

Query:
```
db.users.find({ "clients.name": 'clients_1'} ).explain()
```
Execution times:
 - no indexes: 1730ms
 - `clients:1`: 1839ms
 - `clients.name:1`: 1816ms
 - `clients.games.name:1`: 1934ms
 - `clients:1` & `client.games:1`: 1709ms


Query:
```
db.users.find({ clients: { $elemMatch: { name: 'clients_1'} } }).explain()
```
Execution times:
- no indexes: 1027
- `clients:1`: 1040ms
- `clients.name:1`: 1031ms
- `clients.games.name:1`: 988ms
- `clients:1` & `client.games:1`: 1045ms
```

### Finding user clients on two collections

Query:
```
db.users.find({ "name": "users_99999" }).explain()
```
- no indexes: 790
- `name:1`: 2

Query:
```
db.clients.find({ "name": "clients_99999" }).explain()
```
- no indexes: 859
- `name:1`: 0


### Finding session with multi-index
Query:
```
db.sessions.find({name: "sessions_99999", index: "index_99999"}).explain()
```
- no indexes: 490
- `name:1, index: 1`: 1

### Finding user client games

Query:
```
db.users.find({ "clients.games.name": 'games_0'} ).explain()
```
Execution times:
- no indexes: 1948
- `clients:1`: 2186ms
- `clients.name:1`: 1940ms
- `clients.games.name:1`: 1962ms
- `clients:1` & `client.games:1`: 1941ms

Query:
```
db.users.find({ clients: { $elemMatch: { 
    games: {  $elemMatch: { name: 'games_0' }}
}}}).explain()
```
Execution times:
- no indexes: 1203ms
- `clients:1`: 1137ms
- `clients.name:1`: 1111ms
- `clients.games.name:1`: 1107ms
- `clients:1` & `client.games:1`: 1120ms


Query:
```
db.stats()
```
- No indexes: 
    + indexSize: 62636336,
    + fileSize: 1006632960,
- `clients:1`:
    + indexSize: 315160272,
    + fileSize: 2080374784,
- `clients.name:1`:
    + indexSize: 95667376,
    + fileSize: 2080374784,    
- `clients:1` & `client.games:1`:
    + indexSize: 348191312,
    + fileSize: 2080374784,


### Used indexing commands
#### Dropping indexes on clients
```
db.users.getIndexes();
```

#### Ensuring indexes on clients
```
db.users.ensureIndex({clients:1});
db.users.ensureIndex({"client.name":1});
db.users.ensureIndex({"client.games":1});
db.users.ensureIndex({"client.games.name":1});
db.users.ensureIndex({name:1});
db.clients.ensureIndex({name:1});
db.sessions.ensureIndex({name:1, index:1});
```
#### Dropping indexes on clients
```
db.users.dropIndex({clients:1});
db.users.dropIndex({"client.name":1});
db.users.dropIndex({"client.games":1});
db.users.dropIndex({"client.games.name":1});
db.users.dropIndex({name:1});
db.clients.dropIndex({name:1});
db.sessions.dropIndex({name:1, index:1});
```
