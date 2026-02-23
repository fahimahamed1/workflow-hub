# 🚀 Android GitHub Actions Workflows

This repository contains three different Android CI/CD workflows designed for different use cases:

- ✅ Simple CI build
- ✅ Automated unsigned release
- ✅ Fully automated signed production release with semantic versioning

---

# 📦 Workflow Overview

| Workflow File | Purpose | Signing | Versioning | GitHub Release | Recommended Use |
|---------------|----------|----------|------------|----------------|-----------------|
| `simple-android-build.yml` | Basic CI build | ❌ No | ❌ No | Only on tag | Development / Testing |
| `android-build-unSign-release.yaml` | Automated unsigned release | ❌ No | From build.gradle | ✅ Yes | Internal QA |
| `android-build-autoSign-release-pro.yml` | Production-grade automated release | ✅ Yes (Temp keystore) | ✅ Auto semantic | ✅ Yes | Production / Beta |

---

# 1️⃣ simple-android-build.yml

## 📌 Purpose
A lightweight CI workflow that builds Release APK and AAB files.

## 🕒 Triggers
- Push to `main`
- Tag push (`v*`)
- Manual trigger (`workflow_dispatch`)

## 🔧 What It Does
1. Checkout repository
2. Setup JDK 17
3. Setup Android SDK
4. Clean project
5. Build:
   - Release APK
   - Release AAB
6. Upload artifacts
7. If triggered by tag → publish GitHub Release (APK only)

## 🎯 Best For
- Pull request validation
- Continuous Integration builds
- Manual tagged releases

## 🚫 Limitations
- No automatic version increment
- No automatic tag creation
- No signing configuration

---

# 2️⃣ android-build-unSign-release.yaml

## 📌 Purpose
Automates building and creating a GitHub release without signing.

## 🕒 Triggers
- Push to `main`
- Manual trigger

## 🔧 What It Does
1. Builds Release APK & AAB
2. Reads:
   - `versionName`
   - `versionCode`
   from `app/build.gradle`
3. Creates version tag:
   ```
   v<versionName>.<github_run_number>
   ```
4. Pushes Git tag
5. Creates GitHub Release
6. Uploads:
   - APK
   - AAB
7. Stores build artifacts in Actions

## 🎯 Best For
- Internal distribution
- QA builds
- Projects using external signing (Google Play App Signing)

## 🚫 Limitations
- No automatic semantic version bump
- Uses version defined in build.gradle
- No signing (unsigned output)

---

# 3️⃣ android-build-autoSign-release-pro.yml (Production Grade)

## 📌 Purpose
Fully automated CI/CD pipeline with:
- Automatic semantic versioning
- Auto versionCode increment
- Temporary keystore generation
- Signed APK & AAB
- Git tag creation
- GitHub release creation
- Automatic release notes generation

## 🕒 Triggers
- Push to:
  - `main`
  - `beta`
- Manual trigger

---

## 🧠 Versioning Logic

1. Fetch latest git tag (example: `v1.2.3`)
2. Increment PATCH version automatically
3. If branch is `beta`:
   ```
   v1.2.4-beta
   ```
4. If branch is `main`:
   ```
   v1.2.4
   ```

---

## 🔐 Signing Strategy

- Generates temporary keystore inside CI
- Random passwords generated automatically
- Used only for build process
- Deleted after workflow completes

⚠️ For Google Play production, enable Play App Signing or replace with real keystore secrets.

---

## 📝 Release Notes

Automatically generated using:

```
git log previous_tag..HEAD
```

Included in GitHub Release body.

---

## 🔧 Automatically Updates

- `versionCode` → incremented by 1
- `versionName` → updated to new semantic version

---

## 🎯 Best For

- Production automation
- Beta releases
- Zero manual release process
- Teams wanting full CI/CD pipeline

---

# 🏗 Recommended Usage Strategy

### 🔹 Development Phase
Use:
```
simple-android-build.yml
```

### 🔹 Internal Testing / QA
Use:
```
android-build-unSign-release.yaml
```

### 🔹 Production / Beta Deployment
Use:
```
android-build-autoSign-release-pro.yml
```

---

# 🔐 Required Permissions

Go to:

```
Repository Settings → Actions → Workflow permissions
```

Enable:
- ✅ Read and write permissions
- ✅ Allow GitHub Actions to create and push tags

---

# 📌 Required Secrets

Currently only required:

```
GITHUB_TOKEN
```

(Automatically provided by GitHub)

If switching to real keystore signing, add:

- `KEYSTORE_BASE64`
- `KEYSTORE_PASSWORD`
- `KEY_ALIAS`
- `KEY_PASSWORD`

---

# 📊 Feature Comparison

| Feature | Simple CI | Unsigned Release | Auto Signed Pro |
|----------|-----------|------------------|------------------|
| Builds APK | ✅ | ✅ | ✅ |
| Builds AAB | ✅ | ✅ | ✅ |
| Auto Tag | ❌ | ✅ | ✅ |
| Auto Version Bump | ❌ | ❌ | ✅ |
| Signed | ❌ | ❌ | ✅ |
| Auto Release Notes | ❌ | ❌ | ✅ |
| Production Ready | ❌ | ⚠️ | ✅ |

---

# 🧩 Possible Improvements

You can extend these workflows to:

- Upload to Google Play (Play Developer API)
- Upload to Firebase App Distribution
- Add Slack/Discord notifications
- Run unit tests & lint before release
- Add code coverage reporting
- Add build caching optimizations

---

# 📌 Summary

You now have:

- 🟢 Simple CI workflow
- 🟡 Automated unsigned release workflow
- 🔵 Fully automated signed production release workflow

Choose the one that fits your development stage and deployment strategy.
