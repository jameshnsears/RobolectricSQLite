# NOTES

## 1. Summary

* Starting point: <https://github.com/robolectric/robolectric/issues/4209>

* Changes:
  * updated package versions
  * moved to "maven-publish" + "com.github.johnrengelman.shadow" plugins 
  * migration to GitHub Packages
  * removed superfluous sample module code

---

## 2. GitHub Packages

### 2.1. Token

* create package token

```text
https://github.com/settings/tokens
    > New personal access token
        > write:packages
        > delete:packages
```

* store GITHUB_TOKEN in local.properties

### 2.2. Publishing

```text
gradle      
    > library module 
        > build
            > build
        > publishing
            > publish (to GitHub)
            > publishToMavenLocal (to $HOME/.m2/repository/com/github/guness)
```

---

## 3. (Optional) GitHub GraphQL

* NOTE: "Public package versions are not eligible for deletion."

* https://docs.github.com/en/free-pro-team@latest/packages/manage-packages/deleting-a-package  

```text
https://docs.github.com/en/free-pro-team@latest/graphql/overview/explorer

{
  repository(owner: "jameshnsears", name: "RobolectricSQLite") {
    packages(first: 10) {
      nodes {
        versions(last: 10) {
          nodes {
            version
            id
          }
        }
      }
    }
  }
}
```

```text
curl -X POST \
-H "Accept: application/vnd.github.package-deletes-preview+json" \
-H "Authorization: bearer $GITHUB_TOKEN" \
-d '{"query":"mutation { deletePackageVersion(input:{packageVersionId:\"MDE0OlBhY2thZ2VWZXJzaW9uNTcyODc2NA==\"}) { success }}"}' \
https://api.github.com/graphql
```
