# 🚀 Android GitHub Actions Workflows

This repository includes **four Android CI/CD workflows** designed for different stages of development — from simple CI builds to fully automated production releases.

Choose the one that fits your workflow.

---

# 📦 Workflow Overview

| Workflow File | Purpose | Signing | Versioning | Release | Best For |
|---------------|---------|---------|------------|---------|----------|
| `simple-android-build.yml` | Basic CI build | ❌ | ❌ | On tag only | Development |
| `android-build-unSign-release.yaml` | Unsigned auto release | ❌ | From `build.gradle` | ✅ | QA / Internal |
| `android-build-autoSign-release-pro.yml` | Full production pipeline | ✅ | Auto semantic | ✅ | Production |
| `android-auto-release.yml` | Commit-controlled release | ❌ | From `build.gradle` | ✅ | Controlled production |

---

# 1️⃣ simple-android-build.yml

### 📌 What it does
- Builds Release APK & AAB  
- Uploads build artifacts  
- Creates release only when pushing a tag  

### 🕒 Triggers
- Push to `main`
- Tag push (`v*`)
- Manual trigger

### 🎯 Best for
- Pull request validation  
- CI builds  
- Manual tagged releases  

### 🚫 Limitations
- No version bump  
- No automatic tagging  
- No signing  

---

# 2️⃣ android-build-unSign-release.yaml

### 📌 What it does
- Builds Release APK & AAB  
- Reads `versionName` & `versionCode` from `build.gradle`  
- Creates tag:  
  ```
  v<versionName>.<github_run_number>
  ```
- Publishes GitHub Release  
- Uploads APK & AAB  

### 🕒 Triggers
- Push to `main`
- Manual trigger  

### 🎯 Best for
- QA builds  
- Internal distribution  
- Projects using Google Play App Signing  

### 🚫 Limitations
- No semantic version bump  
- Unsigned output  

---

# 3️⃣ android-build-autoSign-release-pro.yml (Production)

### 📌 What it does
- Automatically increments semantic version  
- Increments `versionCode`  
- Generates temporary keystore  
- Builds signed APK & AAB  
- Creates tag & GitHub Release  
- Generates release notes from commits  

### 🕒 Triggers
- Push to `main`
- Push to `beta`
- Manual trigger  

### 🧠 Versioning Logic
- Reads latest tag (e.g., `v1.2.3`)  
- Auto increments patch → `v1.2.4`  
- On `beta` branch → `v1.2.4-beta`

### 🎯 Best for
- Full production automation  
- Beta pipelines  
- Teams wanting zero manual release steps  

---

# 4️⃣ android-auto-release.yml (Commit-Based Release)

### 📌 What it does
- Builds Release APK & AAB  
- Reads version from `build.gradle`  
- Creates tag using:
  ```
  v<versionName>.<github_run_number>
  ```
- Publishes GitHub Release  
- Uploads APK & AAB  

### 🕒 Triggers
- Push to `main`
- Manual trigger  

To trigger intentionally, include:

```
[release]
```

Example:
```
git commit -m "Prepare production build [release]"
```

### 🎯 Best for
- Controlled production releases  
- Teams who want intentional release commits  
- Simple, stable automation  

### 🚫 Limitations
- No semantic version bump  
- Unsigned build  

---

# 🔐 Required Permissions

Go to:

```
Repository Settings → Actions → Workflow permissions
```

Enable:
- ✅ Read and write permissions  
- ✅ Allow Actions to create and push tags  

---

# 🔑 Required Secrets

Default:
```
GITHUB_TOKEN
```
(Provided automatically by GitHub)

For real signing (optional):
- `KEYSTORE_BASE64`
- `KEYSTORE_PASSWORD`
- `KEY_ALIAS`
- `KEY_PASSWORD`

---

# 📊 Feature Comparison

| Feature | Simple CI | Unsigned | Auto Signed Pro | Commit-Based |
|----------|-----------|----------|-----------------|--------------|
| Builds APK | ✅ | ✅ | ✅ | ✅ |
| Builds AAB | ✅ | ✅ | ✅ | ✅ |
| Auto Tag | ❌ | ✅ | ✅ | ✅ |
| Auto Version Bump | ❌ | ❌ | ✅ | ❌ |
| Signed | ❌ | ❌ | ✅ | ❌ |
| Auto Release Notes | ❌ | ❌ | ✅ | ❌ |
| Production Ready | ❌ | ⚠️ | ✅ | ✅ |

---

# 📌 Final Summary

You now have:

- 🟢 Simple CI build  
- 🟡 Automated unsigned release  
- 🔵 Fully automated signed production pipeline  
- 🟣 Controlled commit-based release  

Start simple during development, move to unsigned for QA, and use the Pro workflow for full production automation.
