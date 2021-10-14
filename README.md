# Template for gretl packages
This repo includes template files for setting up a new gretl project which, optionally, can be pushed to your github-repo.

# Example
Replace package name place holder ```kmeans``` by

```bash
PKG_ID="MY_PKG"
sed -i s/kmeans/$PKG_ID/g *.*
```

# Push existing local repo to github
```bash
git remote add origin git@github.com:atecon/<REPONAME>.git
git branch -M main
git push -u origin main
```

## Changelog
