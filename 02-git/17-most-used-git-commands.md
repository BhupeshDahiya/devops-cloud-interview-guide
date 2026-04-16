## Question  
Explain 10 Git commands that you use on a day-to-day basis. What are they used for?

### 📝 Short Explanation  
This question evaluates your hands-on comfort with Git. It's meant to uncover whether you just follow the basics or actually use Git effectively for version control and collaboration.

## ✅ Answer  
Here are 10 Git commands I use regularly and what I use them for:

---

### 1. `git clone`
```bash
git clone https://github.com/org/repo.git
```
📦 Used to create a local copy of a remote repository when starting a new task or joining a project.

---

### 2. `git status`
```bash
git status
```
🧭 Shows the current state of the working directory — what’s staged, unstaged, and untracked.

---

### 3. `git add`
```bash
git add file.py
git add .
```
📝 Stages changes before committing. I use this before `git commit` to mark files I want to include.

---

### 4. `git commit`
```bash
git commit -m "Fix login bug"
```
💾 Saves a snapshot of the staged changes with a message describing the purpose.

---

### 5. `git push`
```bash
git push origin feature-branch
```
🚀 Uploads local commits to the remote repository, usually after a commit or merge.

---

### 6. `git pull`
```bash
git pull origin main
```
🔄 Fetches changes from the remote and merges them into the current branch — helps keep in sync with team updates.

---

### 7. `git fetch`
```bash
git fetch origin
```
📥 Downloads changes from the remote without merging — I use this when I want to review changes before integrating.

---

### 8. `git branch`
```bash
git branch
git branch feature/login
```
🌿 Lists all branches or creates a new one. Branching is the foundation of my feature workflows.

---

### 9. `git checkout`
```bash
git checkout main
git checkout -b hotfix/typo
```
🧳 Switches between branches or creates and switches in one go. I use this constantly during development.

---

### 10. `git rebase`
```bash
git rebase main
```
📚 Re-applies commits from one branch on top of another. Useful for maintaining a clean commit history before merging.

---

### 11. `git merge`
```bash
git merge main
```
📚 Merges branches or integrate changes from one branch to another.

---

### 12. `git log`
```bash
git rebase main
```
📝 To see the commit history on local machine.

---
