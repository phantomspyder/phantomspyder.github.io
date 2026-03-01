---
title: "Git Objects: The Engine of Version Control"
date: 2025-12-24
---
Git’s power comes from how it stores data – through objects. Understanding these core objects is vital to understanding how version control works. Let’s examine the primary types: Blobs, Trees, Commits, and Tags.

**Step 1: Creating an Empty Repository**

Initially, we have a completely empty Git repository.

```bash
mkdir my_repo
cd my_repo
git init
tree -a
Output:
.
└── .git
    ├── config
    ├── description
    ├── HEAD
    ├── hooks
    │   ├── applypatch-msg.sample
    │   ├── commit-msg.sample
    │   ├── fsmonitor-watchman.sample
    │   ├── post-update.sample
    │   ├── pre-applypatch.sample
    │   ├── pre-commit.sample
    │   ├── pre-merge-commit.sample
    │   ├── pre-push.sample
    │   ├── pre-rebase.sample
    │   ├── pre-receive.sample
    │   ├── prepare-commit-msg.sample
    │   ├── push-to-checkout.sample
    │   └── update.sample
    ├── info
    │   └── exclude
    ├── objects
    │   ├── info
    │   └── pack
    └── refs
        ├── heads
        └── tags
```

**Step 2: Creating a Blob (File)**

A blob is a raw data snapshot of a file. Let's add a simple `README.md` file.

```bash
echo Hello World! > README.md
git add README.md
tree -a
Output:
.
├── .git
│   ├── config
│   ├── description
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── fsmonitor-watchman.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── pre-merge-commit.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── push-to-checkout.sample
│   │   └── update.sample
│   ├── index
│   ├── info
│   │   └── exclude
│   ├── objects
│   │   ├── 98
│   │   │   └── 0a0d5f19a64b4b30a87d4206aade58726b60e3
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       └── tags
└── README.md
```

Notice the new `README.md` file and the `objects` directory. The `objects` directory contains all of Git’s internal objects, identified by their SHA-1 hashes.

**Step 3: Creating a Tree**

Now, let's create a directory structure.

```bash
mkdir docs
echo Hello Again >  docs/README.md
git add docs
tree -a
Output:
.
├── .git
│   ├── config
│   ├── description
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── fsmonitor-watchman.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── pre-merge-commit.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── push-to-checkout.sample
│   │   └── update.sample
│   ├── index
│   ├── info
│   │   └── exclude
│   ├── objects
│   │   ├── 90
│   │   │   └── 9789960b67d38f5e7fa0bb51238079cf041c6a
│   │   ├── 98
│   │   │   └── 0a0d5f19a64b4b30a87d4206aade58726b60e3
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       └── tags
├── docs
│   └── README.md
└── README.md
```

The `tree` command clearly visualizes the directory structure, including the SHA-1 hashes associated with each file and directory.

**Step 4: Committing the Changes**

Let's commit these changes.

```bash
git commit -m "My first commit"
tree -a
Output:
. (directory)
├── .git
│   ├── COMMIT_EDITMSG
│   ├── config
│   ├── description
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── fsmonitor-watchman.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── pre-merge-commit.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── push-to-checkout.sample
│   │   └── update.sample
│   ├── index
│   ├── info
│   │   └── exclude
│   ├── logs
│   │   ├── HEAD
│   │   └── refs
│   │       └── heads
│   │           └── main
│   ├── objects
│   │   ├── 4c
│   │   │   └── 400e691ca50b13358da4e665f0757b2549dbc8
│   │   ├── 90
│   │   │   └── 9789960b67d38f5e7fa0bb51238079cf041c6a
│   │   ├── 98
│   │   │   └── 0a0d5f19a64b4b30a87d4206aade58726b60e3
│   │   ├── bc
│   │   │   └── f0e1093dc6cdc6d1096da4c030177a33eab172
│   │   ├── c2
│   │   │   └── 64a8d463de75588458f6830d2d15fe61ff725b
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       │   └── main
│       └── tags
├── docs
│   └── README.md
└── README.md
```

**Step 5: Creating a Tag**

Finally, let's create a tag to mark a specific point in history.

```bash
git tag -a v1.0 -m "Initial version"
tree -a
Output:
.
├── .git
│   ├── COMMIT_EDITMSG
│   ├── config
│   ├── description
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── fsmonitor-watchman.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── pre-merge-commit.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── push-to-checkout.sample
│   │   └── update.sample
│   ├── index
│   ├── info
│   │   └── exclude
│   ├── logs
│   │   ├── HEAD
│   │   └── refs
│   │       └── heads
│   │           └── main
│   ├── objects
│   │   ├── 4c
│   │   │   └── 400e691ca50b13358da4e665f0757b2549dbc8
│   │   ├── 6f
│   │   │   └── 04d16bee2231b71d94d649e73bd1da89ae0ece
│   │   ├── 90
│   │   │   └── 9789960b67d38f5e7fa0bb51238079cf041c6a
│   │   ├── 98
│   │   │   └── 0a0d5f19a64b4b30a87d4206aade58726b60e3
│   │   ├── bc
│   │   │   └── f0e1093dc6cdc6d1096da4c030177a33eab172
│   │   ├── c2
│   │   │   └── 64a8d463de75588458f6830d2d15fe61ff725b
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       │   └── main
│       └── tags
│           └── v1.0
├── docs
│   └── README.md
└── README.md
```

You can see the `tags` directory containing the `v1.0` tag, linked to the commit.  The tag also has a SHA-1 hash.

```bash
git show v1.0
tag v1.0
Tagger: Test <Test>
Date:   Fri Dec 26 19:20:54 2025 +1300

Initial version

commit c264a8d463de75588458f6830d2d15fe61ff725b (HEAD -> main, tag: v1.0)
Author: Test <Test>
Date:   Fri Dec 26 19:12:02 2025 +1300

    My first commit

```
But how why did it pick this commit? It is because this is where the HEAD was pointing to.

**Conclusion**

This example demonstrates how Git uses objects, represented by SHA-1 hashes, to track changes and maintain a history of your project. By starting with a simple repository and incrementally adding files and directories, you can gain a deeper understanding of Git's internal workings.