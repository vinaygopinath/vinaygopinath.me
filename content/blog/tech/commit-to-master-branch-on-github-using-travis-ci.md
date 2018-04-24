---
title: "Commit to Master Branch on GitHub Using Travis CI"
description: "Automate your Travis CI build process to commit to the master branch of your repo on Git/GitHub"
date: 2018-04-24T11:57:08+02:00
blog: ["tech"]
categories: ["tech"]
tags: ["Travis CI", "git", "Nomad Couple"]
---

I'm trying to turn my pet project to find visa requirements for couples, [Nomad Couple](https://vinaygopinath.github.io/NomadCouple), into a full-fledged progressive web app (PWA) that has regularly updated visa requirements data (from Wikipedia) through a Travis CI cron job.

As part of that, I've set up the [wiki scraper repo](https://github.com/vinaygopinath/visa-req-wiki-scraper) to be rebuilt every month and any changes in the visa requirements for citizens of different countries (available in `dist/output`) to be automatically committed back to the `master` branch.

### Attempt #1 - Travis' Github Pages deployment

I noticed that Travis CI offers automated [deployment to GitHub Pages](https://docs.travis-ci.com/user/deployment/pages/) via configuration in `.travis.yml`. By default, it commits code to the `gh-pages` branch, but the configuration has a `target_branch` property to customize it. [This](https://github.com/vinaygopinath/visa-req-wiki-scraper/blob/698b7617bbc4ba2382efe57f263c428a5a685c87/.travis.yml) is the configuration I tried

```yaml
language: node_js
node_js:
  - "lts/*"

# ...
# Unrelated configuration for node.js/electron project
# ...

script:
  - npm run scrape
deploy:
  provider: pages
  skip-cleanup: true
  target-branch: master # Commit to master instead of gh-pages
  github-token: $GH_TOKEN
  keep-history: true # By default, Travis uses push --force and wipes out commit history
  verbose: true
  on:
    branch: master
```

The `GH_TOKEN` mentioned in the config refers to a token generated (with `public_repo` permission) on the [GitHub personal access tokens page](https://github.com/settings/tokens) that I've saved on Travis CI as an [environment variable](https://docs.travis-ci.com/user/environment-variables#Defining-Variables-in-Repository-Settings).

While this was super easy to set up, it has a few issues.

### Drawbacks

1. This approach is dependent on Travis CI's GitHub Pages support. You're out of luck if you're using a different Git hosting provider or your own Git server.
2. Travis listens for commits on the `master` branch and triggers a build. Since the commit at build time is pushed back to `master`, this triggers an infinite build loop<sup>[[1]](https://github.com/travis-ci/travis-ci/issues/9329)</sup>!
3. It's [not possible](https://github.com/travis-ci/travis-ci/issues/9287) (as of Apr 2018) to customize the commit message. This rules out adding ["[skip ci]"](https://docs.travis-ci.com/user/customizing-the-build#Skipping-a-build) to the commit message to avoid the infinite loop.

### Attempt #2 - Good ol' shell script

Travis supports `after_success`, a hook that is called when the build succeeds. I replaced the `deploy` section in `.travis.yml` above with:

```yaml
after_success:
- sh .travis-push.sh
```

travis-push.sh
```bash
#!/bin/sh
# Credit: https://gist.github.com/willprice/e07efd73fb7f13f917ea

setup_git() {
  git config --global user.email "travis@travis-ci.org"
  git config --global user.name "Travis CI"
}

commit_country_json_files() {
  git checkout master
  # Current month and year, e.g: Apr 2018
  dateAndMonth=`date "+%b %Y"`
  # Stage the modified files in dist/output
  git add -f dist/output/*.json
  # Create a new commit with a custom build message
  # with "[skip ci]" to avoid a build loop
  # and Travis build number for reference
  git commit -m "Travis update: $dateAndMonth (Build $TRAVIS_BUILD_NUMBER)" -m "[skip ci]"
}

upload_files() {
  # Remove existing "origin"
  git remote rm origin
  # Add new "origin" with access token in the git URL for authentication
  git remote add origin https://vinaygopinath:${GH_TOKEN}@github.com/vinaygopinath/visa-req-wiki-scraper.git > /dev/null 2>&1
  git push origin master --quiet
}

setup_git

commit_country_json_files

# Attempt to commit to git only if "git commit" succeeded
if [ $? -eq 0 ]; then
  echo "A new commit with changed country JSON files exists. Uploading to GitHub"
  upload_files
else
  echo "No changes in country JSON files. Nothing to do"
fi
```

TODO: If you're using Travis on public GitHub repositories, your build log is publicly visible. If there are any Git related errors, it is possible that the origin URL (with your GitHub personal access token with access to ALL your public repositories) may be logged, which is a huge security risk. It is strongly recommended to redirect the output of all git commands to `/dev/null` (e.g, `git push origin master --quiet > /dev/null 2>&1`) once you've verified that the script works for your repo.

References:
* Will Price's [GitHub Gist](https://gist.github.com/willprice/e07efd73fb7f13f917ea)
* Justin Ellis' [post](https://jellis18.github.io/post/2017-12-03-continuous-integration-hugo/)