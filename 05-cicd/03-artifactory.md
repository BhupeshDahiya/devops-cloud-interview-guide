## Question  
Which artifact repository do you use for builds?

### 📝 Short Explanation  
An artifact repository is a storage system where you can **publish**, **store**, and **retrieve** build artifacts like `.jar`, `.war`, Docker images, etc. It’s crucial in modern CI/CD pipelines for dependency management and versioning.

## ✅ Answer  
### JFROG
Oh, we use JFrog Artifactory for our builds. It handles all our artifacts—Maven, npm, Docker images—you name it. We push build outputs there and pull dependencies from it in our pipelines. It’s really reliable for versioning, supports lots of repo types, and integrates nicely with Jenkins and other CI/CD tools. The caching and proxying for remote repos also speeds things up, so our builds are consistent and faster.
### NEXUS (Better for maven only usecases)
For our builds, we use Nexus Repository. It manages all our artifacts—like Maven, npm, and Docker images—in a central place. We push build outputs to Nexus and pull dependencies from it in our pipelines. It helps with version control, dependency management, and keeps builds consistent across environments.

---

### 📘 Why JFrog Artifactory?

- **Supports multiple package formats** (Maven, npm, Docker, PyPI, Helm, etc.)
- **Integration with Jenkins & GitHub Actions** for publishing artifacts during CI/CD.
- **Proxying public repositories** like Maven Central or Docker Hub to improve speed and reliability.
- **Access control and security** via RBAC for different teams and projects.
- **Retention policies** to clean up outdated builds automatically.

### 📘 Why Sonatype Nexus ?

- **Supports multiple artifact formats** (Maven, npm, Docker, etc.) in a single repository.
- **Centralized storage for all build artifacts** easy versioning and dependency management.
- **Integrates seamlessly with Jenkins & GitHub Actions** for publishing artifacts during CI/CD.
- **Access control and security features** for safe artifact management.
- **Reliable and widely adopted** has good community support.

---

### 🔄 Typical Workflow

1. Developer commits code to GitHub.
2. Jenkins triggers a build and packages the application using Maven.
3. The artifact is pushed to Artifactory using `mvn deploy`.
4. Later stages (like deployment) pull the artifact from Artifactory.

---

### 🧠 Alternatives I've worked with:
- **Sonatype Nexus Repository** – Lightweight and easy to set up for Maven-only use cases.
- **AWS CodeArtifact** – Great for AWS-native environments.
- **GitHub Packages** – Useful for open-source projects or tight GitHub integrations.

---

> Summary:  
> My go-to artifact repository is **JFrog Artifactory** due to its versatility, wide ecosystem support, and strong integration with CI/CD pipelines.

---
