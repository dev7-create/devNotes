# Dev Notes 🛠️

Notes I’m taking as a new Frappe/ERPNext dev — sharing in case this helps other new devs too.

---

## Prerequisites (Read This First)

### 1. Access You Need

* ✅ **GitHub org / repo access** (apps you’ll work on)
* ✅ **Staging site access** (to download DB backup & check app versions)

---

### 2. Git Basics You *Must* Know

These come up **daily** in Frappe dev.

#### `git log`

Shows commit history.

```bash
git log --oneline --decorate --graph
```

Useful to see:

* commit hashes
* tags
* branches

---

#### `git tag`

Tags usually represent releases.

```bash
git tag
git tag -l "v15*"
```

---

#### `git fetch --tags`

Fetches all tags from remote.

```bash
git fetch --tags
```

Very important when checking out **release tags**.

---

#### `git checkout`

Switch branch or tag.

```bash
git checkout version-15
git checkout v15.96.1
```

⚠️ Checking out a **tag** puts you in *detached HEAD* mode (normal for debugging).

---

### 3. Git Authentication (Very Important)

#### Classic Token (HTTPS)

* Used when cloning via HTTPS
* Generate from GitHub → Developer Settings → Personal Access Tokens
* Used as **password** when Git asks

Example:

```bash
git clone https://github.com/org/repo.git
```

---

#### SSH Key (Recommended ✅)

Best for long-term dev work.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```

* Add public key to GitHub → SSH Keys

Clone using:

```bash
git clone git@github.com:org/repo.git
```

---

## Bench Setup (Correct & Clean Flow)

### 1. Initialize Bench

```bash
bench init --frappe-branch version-15 name-bench
```

💡 **Good practice**: postfix bench name with `-bench`

---

### 2. Create Local Site

```bash
cd name-bench
bench new-site site-name-local
```

💡 Always postfix local sites with `-local`

---

### 3. Select the Site

```bash
bench use site-name-local
```

---

### 4. Get Info from Staging Site

On **staging site**:

* Go to **Help → About**
* Note:

  * Frappe version
  * ERPNext version
  * Custom apps list

---

### 5. Install Required Apps

Clone apps based on staging info:

```bash
bench get-app app_name --branch version-15
```

Then install on site (not required if using bench restore):

```bash
bench --site site-name-local install-app app_name
```

---

### 6. Setup System Requirements

```bash
bench setup requirements
```

Installs:

* node packages
* python deps

---

### 7. Restore Database Backup

#### a) Download Backup from Staging

On staging site:

* Download **Database Backup**
* If async → check **RQ Job** status

#### b) Restore Locally

```bash
bench --site site-name-local restore path/to/db.sql.gz
```

⚠️ This **overwrites** local DB — expected.

---

### 8. Set Admin Password

```bash
bench --site site-name-local set-admin-password
```

---

### 9. Migrate Site

```bash
bench migrate
```

Applies:

* patches
* schema updates
* app migrations

---

### 10. Start Bench 🚀

```bash
bench start
```

Open browser:

```
http://localhost:8000
```

---

## Common Mistakes (Fixed from Original Notes)

❌ Running `bench migrate` **before restore**
✅ Always restore DB **first**, then migrate

❌ Forgetting to install apps before restore
✅ Apps must exist before restoring DB

❌ Not matching app branches with staging
✅ Always check **Help → About**

---

## TL;DR Quick Checklist

* [ ] Git access (token preferred)
* [ ] Staging access
* [ ] `bench init`
* [ ] `bench new-site`
* [ ] `bench get-app`
* [ ] `bench setup requirements`
* [ ] `bench restore`
* [ ] `bench migrate`
* [ ] `bench start`

---

✨ And voilà — your bench is ready for development.

Happy hacking 💻🔥
