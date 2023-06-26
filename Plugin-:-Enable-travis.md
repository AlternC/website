A travis account could build nightly, to use this feature : 

* Retrieve 6D5E5753F12109663BABEED53087CD3324A99FBC.gpg from any AlternC admin (gpg key decicated to AlternCBot account)
* Verify .gitignore and exclude gpg file
* Create empty travis file ```touch .travis.yml```
```
language : bash
env:
    global:
      - HUB_PROTOCOL=https
      #GITHUB_TOKEN
      - secure: "[GITHUB_TOKEN=GITHUB_TOKEN env]"
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
    - openssl aes-256-cbc -K $encrypted_fc16b55399d6_key -iv $encrypted_fc16b55399d6_iv -in 6D5E5753F12109663BABEED53087CD3324A99FBC.gpg.enc -out ../gpg-debuild/6D5E5753F12109663BABEED53087CD3324A99FBC.gpg -d
    - gpg --import 6D5E5753F12109663BABEED53087CD3324A99FBC.gpg
install:
    - debuild '-k3087CD3324A99FBC'
deploy:
    - provider: releases
      skip_cleanup: true
      file_glob: true
      file: "../*.deb"
      api_key:
          secure: "[GITHUB_TOKEN]"
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
          secure: "[GITHUB_TOKEN]"
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
* Update debian/rules file : 
```
#!/usr/bin/make -f
# Sample debian/rules that uses debhelper.
# This file is public domain software, originally written by Joey Hess.
#
# This version is for a multibinary package. It also allows you to build any
# of the binary packages independantly, via binary-<package> targets.

# Uncomment this to turn on verbose mode.
export DH_VERBOSE=1

#Define version packaging
#Exclude nighly version
VERSION=$(shell git tag -l --points-at HEAD |grep -v 'nightly')
ifeq ($(strip $(VERSION)),)
	VERSION=$(shell git tag | sort -V |grep -v nightly|tail -1)
	ifeq ($(strip $(VERSION)),)
		export VERSION="0.0.0"
	endif
	VERSION:=$(VERSION)+$(shell date +'%y%m%d%H%M%S')
	IS_NIGHTLY=1
endif

clean:
	dh clean
	rm -f debian/files
	(git rev-parse --git-dir > /dev/null 2>&1 && git checkout debian/changelog) || true
ifeq ($(strip $(IS_NIGHTLY)),1)
	(git rev-parse --git-dir > /dev/null 2>&1 && dch -m -b -v $(VERSION) autobuild) || true
endif
%:
	dh $@
```
* Login to travis as AlterncBot ```travis login --github-token ghp_... --pro```
* Encrypt file ```travis encrypt-file 6D5E5753F12109663BABEED53087CD3324A99FBC.gpg --pro```
* Encrypt GITHUB_TOKEN env ```travis encrypt GITHUB_TOKEN=ghp_...```
* Encrypt GITHUB_TOKEN value ```travis encrypt ghp_...```

