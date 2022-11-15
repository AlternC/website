A travis account could build nightly, to use this feature :-1: 

* Retrieve 077CC9F2.gpg from any admin
* Verify .gitignore and exclude gpg file
* Create empty travis file ```touch .travis.yml```
```
language : bash
env:
    global:
      - HUB_PROTOCOL=https
      #GITHUB_TOKEN
      - secure: "[GITHUB_TOKEN env]"
branches:
  except:
  - nightly
git:
  quiet: true
before_install:
    - curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
    - echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
    - sudo apt-get -qq update
    - sudo apt-get install -y debhelper devscripts fakeroot build-essential po-debconf gh
    - openssl aes-256-cbc -K $encrypted_2155243bff9e_key -iv $encrypted_2155243bff9e_iv -in 077CC9F2.gpg.enc -out 077CC9F2.gpg -d
    - gpg --import 077CC9F2.gpg
install:
    - debuild '-k9D1DC8DD077CC9F2'
deploy:
    - provider: releases
      skip_cleanup: true
      file_glob: true
      file: "../*.deb"
      api_key:
          secure: "[GITHUB_TOKEN value]"
      on:
          tags: true
    - provider: releases
      skip_cleanup: true
      file_glob: true
      file: "../*.deb"
      tag_name: "nightly"
      prerelease: true
      name: Automatic nightly build by Travis on $(date +'%F %T %Z').
      target_commitish: $TRAVIS_COMMIT
      api_key:
          secure: "[GITHUB_TOKEN value]"
      on:
          branch: master
before_deploy:
    - >
      if [[ $TRAVIS_BRANCH == "master" ]]; then
        gh release delete nightly -R ${TRAVIS_REPO_SLUG} -y
        git tag -f nightly || true &&
        git remote add gh https://${GITHUB_TOKEN}@github.com/${TRAVIS_REPO_SLUG}.git &&
        git push --delete -f gh nightly || true &&
        git push -f gh nightly || true
      fi
```
* Login to travis as AlterncBot ```travis login --github-token ghp_... --pro```
* Encrypt file ```travis encrypt-file 077CC9F2.gpg --add --pro```
* Encrypt GITHUB_TOKEN env ```travis encrypt GITHUB_TOKEN=ghp_...```
* Encrypt GITHUB_TOKEN value ```travis encrypt ghp_...```

