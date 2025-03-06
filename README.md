The **"Write Yourself a Git" (WYAG)** guide explains how to build a simple version of Git in Python. Here’s a **step-by-step summary** of the key concepts and commands to implement your own Git-like system:

---

## **1️⃣ Initialize a Git Repository**  
Every Git repository has a `.git` directory to store all objects and metadata.  
**Command to implement:**  
```python
def init(repo_path):
    os.makedirs(repo_path + "/.git", exist_ok=True)
    os.makedirs(repo_path + "/.git/objects", exist_ok=True)
    os.makedirs(repo_path + "/.git/refs", exist_ok=True)
    with open(repo_path + "/.git/HEAD", "w") as f:
        f.write("ref: refs/heads/master\n")
```
**Run:**  
```sh
python git.py init
```
---

## **2️⃣ Storing File Content as an Object**  
Git stores files as **blob objects** in a compressed format.  
**Command to implement:**  
```python
import hashlib, zlib

def hash_object(file_path):
    data = open(file_path, 'rb').read()
    obj = b'blob ' + str(len(data)).encode() + b'\x00' + data
    oid = hashlib.sha1(obj).hexdigest()
    
    obj_path = f".git/objects/{oid[:2]}/{oid[2:]}"
    os.makedirs(os.path.dirname(obj_path), exist_ok=True)
    
    with open(obj_path, "wb") as f:
        f.write(zlib.compress(obj))
    
    return oid
```
**Run:**  
```sh
python git.py hash-object file.txt
```
---

## **3️⃣ Retrieving Stored Objects**  
Retrieve and decompress stored objects from `.git/objects/`.  
**Command to implement:**  
```python
def cat_file(oid):
    obj_path = f".git/objects/{oid[:2]}/{oid[2:]}"
    with open(obj_path, "rb") as f:
        data = zlib.decompress(f.read())
    print(data.split(b'\x00', 1)[1].decode())
```
**Run:**  
```sh
python git.py cat-file <object-hash>
```
---

## **4️⃣ Storing Directory Structure as a Tree**  
Git tracks directories using **tree objects**, which contain references to blob objects.  
**Command to implement:**  
```python
def write_tree():
    entries = []
    for file in os.listdir("."):
        if os.path.isfile(file):
            oid = hash_object(file)
            entries.append(f"100644 {file}\x00{bytes.fromhex(oid)}")
    
    tree_data = b"tree " + str(sum(len(e) for e in entries)).encode() + b"\x00" + b"".join(entries)
    tree_oid = hashlib.sha1(tree_data).hexdigest()
    
    obj_path = f".git/objects/{tree_oid[:2]}/{tree_oid[2:]}"
    os.makedirs(os.path.dirname(obj_path), exist_ok=True)
    
    with open(obj_path, "wb") as f:
        f.write(zlib.compress(tree_data))
    
    return tree_oid
```
**Run:**  
```sh
python git.py write-tree
```
---

## **5️⃣ Creating a Commit Object**  
Commits store metadata and reference a tree object.  
**Command to implement:**  
```python
def commit_tree(tree_oid, message, parent=None):
    commit_data = f"tree {tree_oid}\n"
    if parent:
        commit_data += f"parent {parent}\n"
    commit_data += f"author User <user@example.com>\n"
    commit_data += f"committer User <user@example.com>\n\n"
    commit_data += message + "\n"
    
    commit_obj = b"commit " + str(len(commit_data)).encode() + b"\x00" + commit_data.encode()
    commit_oid = hashlib.sha1(commit_obj).hexdigest()
    
    obj_path = f".git/objects/{commit_oid[:2]}/{commit_oid[2:]}"
    os.makedirs(os.path.dirname(obj_path), exist_ok=True)
    
    with open(obj_path, "wb") as f:
        f.write(zlib.compress(commit_obj))
    
    return commit_oid
```
**Run:**  
```sh
python git.py commit-tree <tree-hash> -m "First commit"
```
---

### **Summary of Commands**
| Command | Description |
|---------|------------|
| `init` | Initialize a repository |
| `hash-object <file>` | Store a file as an object |
| `cat-file <object-hash>` | Retrieve object contents |
| `write-tree` | Save the directory structure |
| `commit-tree <tree-hash> -m "message"` | Create a commit |

This covers the **basic Git internals**. Would you like to add **branches, merges, or logs next?** 🚀
