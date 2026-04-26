## App Crashes with "Permission Denied" in Container but works fine on localhost

### Question  
Your application runs fine locally, but when containerized and run with Docker, it crashes with a "Permission Denied" error. What could be the issue and how would you fix it?

### Short explanation of the question  
This scenario is testing your knowledge of how file permissions, user IDs, and execution rights differ inside a Docker container versus the host system.

### Answer  
The likely cause is that the app or script lacks proper execution permissions in the container or is being run as a non-root user without access. I’d check file permissions and adjust the Dockerfile to ensure the user has necessary rights.

### Detailed explanation of the answer for readers’ understanding

This issue often stems from:

---

### 🔒 1. **Permissions not preserved during COPY**

When using `COPY` in Dockerfile, default permissions might not be sufficient.

For example, a shell script might be copied into the container without execute permission:

```Dockerfile
COPY start.sh /app/start.sh
```

If you run it like this:

```Dockerfile
CMD ["/app/start.sh"]
```

It will throw:

```
Permission denied
```

#### ✅ Fix

Set executable permission inside Dockerfile:

```Dockerfile
RUN chmod +x /app/start.sh
```

---

### 👤 2. **Running as non-root user inside the container**

If your Dockerfile switches to a non-root user for security:

```Dockerfile
USER appuser
```

That user may not have access to directories/files unless proper ownership and permissions are set.

#### ✅ Fix

Ensure files are owned or accessible by that user:

```Dockerfile
RUN chown -R appuser:appuser /app
```

---

### 🧪 3. **Volume mounts with restricted permissions**

Sometimes the container mounts a host directory or file that’s owned by root on host, but not accessible to container's user.

```bash
docker run -v $(pwd)/data:/data myapp
```

If `/data` contains a file not readable by container user, you’ll get "Permission Denied."

#### ✅ Fix

Adjust host file permissions:

```bash
chmod -R 755 data/
```

Or avoid mounting sensitive paths.

---

### 🔍 Debugging steps

- Use `ls -l` inside container to inspect permissions:
  ```bash
  docker exec -it <container> ls -l /app
  ```

- Run container with interactive shell:
  ```bash
  docker run -it myapp /bin/sh
  ```

---

### 🧠 Real-world Insight

> “In one CI build, a Python script crashed with `Permission Denied`. Turned out we copied it from host, but forgot `chmod +x` in the Dockerfile. Fixed it by updating the Dockerfile with `RUN chmod +x script.py`.”

---

### Key takeaway

> "Always check file permissions and user context in your Dockerfile. A script may run fine on your machine but fail inside a container due to missing execute permissions or user access."
---

## Bonus

### **Why do you not run apps inside container as root but always create a appuser (non-root user) ?**

Running apps as root inside a container is a bad practice, especially in production. The main reason is security, not functionality.
By default, containers run as root, but it’s recommended to use a non-root user like appuser for security reasons.
If the application is compromised, running as root could allow an attacker to gain elevated privileges on the container and potentially the host.
Using a non-root user follows the principle of least privilege, limiting what the application can access or modify.
It also helps meet security best practices and compliance requirements in production environments.

### When is root OK?
- Installing packages (during build)
- Temporary debugging

But not at runtime

> Running containers as root increases security risk; using a non-root user enforces least privilege and reduces the impact of potential compromises.
