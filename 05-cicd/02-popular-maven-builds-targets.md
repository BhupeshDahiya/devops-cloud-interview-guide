## Question  
Talk about 5 build targets that you use on a day-to-day basis in Maven.

### 📝 Short Explanation  
Maven uses **build lifecycle phases** (also called build targets) to automate project compilation, packaging, testing, and deployment. These phases are executed using `mvn <goal>` commands.

## ✅ Answer  

Here are 5 Maven build targets I commonly use in my day-to-day workflow:

---

### 1. `mvn clean`
- **Purpose:** Deletes the `target/` directory to ensure a clean slate before a new build.
- **When I use it:** Before any fresh build to avoid conflicts from previous build artifacts.

---

### 2. `mvn compile`
- **Purpose:** Compiles the source code in the `src/main/java` directory.
- **When I use it:** To verify that there are no compilation issues before moving to packaging.

---

### 3. `mvn test`
- **Purpose:** Runs unit tests using a testing framework like JUnit or TestNG.
- **When I use it:** Regularly during development or in CI pipelines to ensure code stability.

---

### 4. `mvn package`
- **Purpose:** Packages the compiled code into a distributable format like a `.jar` or `.war`.
- **When I use it:** When the build is stable and I need to generate an artifact.

---

### 5. `mvn install`
- **Purpose:** Installs the built artifact into the **local Maven repository** (`~/.m2/repository`) so it can be used by other local projects.
- **When I use it:** When I’m developing shared libraries or modules that are reused by other internal projects.

---
## OR
Here are 5 Maven build targets I commonly use in my day-to-day workflow:

--- 
### 1. `npm install`
- Installs all dependencies listed in package.json.
- Often the first step in CI/CD pipelines to set up the environment.
---
### 2. `npm run build`
- Compiles/transpiles the source code (e.g., using Webpack, Babel, TypeScript).
- Generates production-ready files in a dist or build folder.
---
### 3. `npm test / npm run test`
- Runs unit tests, integration tests, or any test suite defined in the project.
- Essential for validating code before deploying.
---
### 4. `npm run lint`
- Runs a linter (like ESLint) to check for code quality and styling issues.
- Helps catch potential bugs and enforce team coding standards.
---
### 5. `npm publish / npm pack`
- Publishes the package to a registry (like npm or Artifactory) or creates a distributable package.
- Often the last step in the release pipeline for internal or public packages.

---

> Summary:  
> The most frequently used Maven targets in my daily work include: `clean`, `compile`, `test`, `package`, and `install`. They ensure a complete and reliable build pipeline from development to artifact distribution.

---
