# 🚀 MiniGit - A Lightweight Git Implementation

# 📝 Introduction
MiniGit is a streamlined version control system inspired by Git, written in Python. It serves as an educational tool for developers interested in understanding the fundamental mechanisms of Git, including repository initialization, object storage, tree creation, and commit management. MiniGit offers a simplified yet functional implementation to enhance learning and experimentation.

## 🔥 Key Features
- ✅ **Initialize a new Git repository** (`init`)
- ✅ **Store and retrieve file objects** (`hash-object`, `cat-file`)
- ✅ **Create and manage directory trees** (`write-tree`)
- ✅ **Commit and track changes** (`commit-tree`)
- ✅ **Lightweight, minimal, and easy to understand**
- ✅ **Ideal for learning version control system internals**

## 🛠 Installation
Ensure you have Python installed on your system before proceeding. Then, clone the repository and navigate into the project directory:

```sh
git clone <repo-url>
cd MiniGit
```

## 🚀 Usage Guide
MiniGit includes several essential version control commands. Below is a step-by-step guide to using them effectively:

### 🎯 Initialize a Repository
Start by setting up a new MiniGit repository:
```sh
python git.py init
```
This command creates a `.git` directory that tracks your project’s files and changes.

### 📌 Store a File as an Object
Save a file in the repository and generate its unique SHA-1 hash:
```sh
python git.py hash-object <filename>
```
This compresses and stores the file in the `.git/objects` directory.

### 🔍 Retrieve Stored Content
Display the contents of a previously stored object:
```sh
python git.py cat-file <object-hash>
```
This allows verification of stored files within the repository.

### 📂 Save the Directory Structure as a Tree
Capture the current working directory as a tree object:
```sh
python git.py write-tree
```
This structures stored file objects into a hierarchical format, preserving relationships between files.

### 📝 Commit Changes
Create a new commit with a descriptive message:
```sh
python git.py commit-tree <tree-hash> -m "Your commit message"
```
This command associates the current tree state with a commit object, tracking modifications over time.

## 🤝 Contributing
We welcome contributions to improve MiniGit! You can contribute by:
- 🐛 Reporting issues
- 💡 Suggesting new features
- 🔥 Submitting pull requests

## 📜 License
MiniGit is released under the **MIT License**, making it freely accessible for modification and distribution.

---

Built with ❤️ for developers passionate about version control and learning Git internals. 🚀

