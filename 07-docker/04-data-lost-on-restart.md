## Data lost when container stops and restarts, How will you fix it?

### Question  
Your application container loses data when it stops and restarts. How do you fix this?

### Short explanation of the question  
This question checks your understanding of Docker storage and persistence. Containers are ephemeral by default, and any data written to the container filesystem is lost on removal unless volumes are used.

### Answer  
I would mount a Docker volume or bind mount to persist the data outside the container. This ensures that data is stored on the host and survives container restarts or recreation.

### Detailed explanation of the answer for readers’ understanding

By default, Docker containers store data in the writable layer of the container. When a container is stopped or removed, any data stored in this layer is lost. To persist data, Docker provides two main mechanisms:

---

### 🗂️ 1. **Docker Volumes**

Docker-managed directories stored in `/var/lib/docker/volumes/`.

```bash
docker volume create mydata
docker run -v mydata:/app/data myapp
```

This way, data written to `/app/data` will persist even if the container stops or is deleted.

---

### 🧷 2. **Bind Mounts**

Mount a host directory directly into the container:

```bash
docker run -v /host/data:/app/data myapp
```

Changes made in `/app/data` inside the container reflect in `/host/data` on the host.

---

### 🧪 Real-world Example

Suppose your container is running a database like SQLite, storing its `.db` file inside the container:

```Dockerfile
CMD ["sqlite3", "/app/db/mydb.sqlite"]
```

Every time the container stops, the database is lost.  
**Fix:** Mount a volume:

```bash
docker run -v sqlite_data:/app/db my-sqlite-app
```

Now, the DB file will persist across container restarts.

---

### 🔍 Verify Persistence

- Stop the container:
  ```bash
  docker stop myapp
  docker rm myapp
  ```

- Start a new container with the same volume:
  ```bash
  docker run -v mydata:/app/data myapp
  ```

- Data will still be there!

---

> In Docker, volumes are managed by Docker and are preferred for production because they are portable and isolated from the host. Bind mounts directly map a host directory into the container and are mainly used in development for live file changes. The key difference is that volumes are Docker-managed, while bind mounts depend on the host filesystem.

### 💡 Real-world pattern
- Dev → bind mount (edit code live)
- Prod → volume (stable + safe storage)

### Key takeaway

> "Containers are ephemeral by design. To persist data, always use Docker volumes or bind mounts — especially for databases, logs, or uploaded files."
