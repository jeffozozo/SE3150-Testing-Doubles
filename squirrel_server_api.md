# Squirrel Server – HTTP API Guide
Squirrel server is a simple, REST based HTTP server that manages squirrels. It is written
in python, uses BaseHTTPRequestHandler, HTTPServer and SQLite.

It illustrates basic HTTP request handling.


This is a short guide to the endpoints exposed by the **Squirrel Server**.  
Default address: **http://127.0.0.1:8080** (edit squirrel_server.py if you want a different port)

> Note: The handler class is `SquirrelServerHandler`; data storage is via `SquirrelDB` (SQLite-backed).  
> The server exposes a REST-style API for managing squirrels.

To start the squirrel server, simply run python squirrel_server.py

---
## Resource
- **squirrels** – collection of squirrel records.

A **squirrel** is a JSON object with the following fields:
```json
{
  "id": 1,
  "name": "Fluffy",
  "size": "large"
}
```
`id` is assigned by the server/DB. Clients provide `name` and `size`.

---

## Endpoints

### List
**GET /squirrels**  
Returns an array of squirrel objects.

```bash
curl -s http://127.0.0.1:8080/squirrels
```

### Retrieve
**GET /squirrels/{id}**  
Returns a single squirrel by id, or **404** if not found.

```bash
curl -s http://127.0.0.1:8080/squirrels/1
```

### Create
**POST /squirrels**  
`Content-Type: application/json`  
Body includes `name` and `size` (no `id`). Returns the created object (or confirmation).

```bash
curl -s -X POST http://127.0.0.1:8080/squirrels   -H "Content-Type: application/json"   -d '{"name":"Fluffy","size":"large"}'
```

### Replace (full update)
**PUT /squirrels/{id}**  
`Content-Type: application/json`  
Body includes `id`, `name`, and `size`. Returns the updated object, or **404** if the id is missing.

```bash
curl -s -X PUT http://127.0.0.1:8080/squirrels/1   -H "Content-Type: application/json"   -d '{"id":1,"name":"Fluffy","size":"small"}'
```

### Delete
**DELETE /squirrels/{id}**  
Deletes the squirrel. Returns 200 on success or **404** if not found.

```bash
curl -s -X DELETE http://127.0.0.1:8080/squirrels/1
```

---

## Status Codes
- **200 OK** – Success.
- **201 Created** – On successful `POST`.
- **400 Bad Request** – Malformed JSON/body.
- **404 Not Found** – Unknown path or missing id.
- **405 Method Not Allowed** – Unsupported method on a resource.
- **500 Internal Server Error** – Unexpected errors.

---

## Notes
- All bodies are **JSON**. Use `Content-Type: application/json` for `POST`/`PUT`.
- Server start (from code):
  ```bash
  python3 squirrel_server.py
  # prints: squirrel_server running at 127.0.0.1:8080
  ```

