---
icon: file-lines
---

# Contributing Guide

Thanks for your interest in contributing! Follow these quick steps to open a pull request. We would encourage both technical and non-technical contributions. Technical contributions may include improvisations and addition of new details for the Circles SDK while non-technical contributions can be creating detailed user oriented guides on various Circles features. All non-technical guides should be created under the **User Guides** folder.

### &#x20;Prerequisites

\- Node.js (v18+ recommended)

\- Git + GitHub account

\- Markdown editor or IDE (e.g., VS Code)

### Steps to Contribute

#### &#x20;1. Fork & Clone

```
git clone https://github.com/your-username/circles-docs.git
cd circles-docs
git remote add upstream https://github.com/aboutcircles/circles-docs.git
```

#### 2. Create a New Branch

```
git checkout -b your-contribution-topic
```

#### 3. Install Dependencies & Start Dev Server

```
npm install
npm run dev
```

Preview: http://localhost:3000

#### 4. Make Your Changes

* Edit or create .md files inside the docs/ folder.
* Follow the tone and structure of existing content.

#### 5. Commit & Push

```
git add .
git commit -m "docs: your concise message"
git push origin your-contribution-topic
```

#### 6. Open a Pull Request

* Visit your fork on GitHub.
* Click “Compare & pull request”.
* Base repo: aboutcircles/circles-docs, branch: main.

After that, your contribution will be reviewed shortly. Thank you for helping improve Circles documentation!
