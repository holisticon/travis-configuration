# travis-configuration

Configuration for travis-ci.org builds that support publishing successful snapshot builds to the sonatype snapshot repo.

**Note:** this only works for maven projects that use the Sonatype OSS maven parent.

**inspirations:**

Check out these links for more background information.

* [Deploying Maven snapshot artifacts to Sonatype](https://groups.google.com/forum/#!topic/travis-ci/86sOKUoFEy0)
* [blog.xeiam.com](http://blog.xeiam.com/2013/05/configure-travis-ci-to-deploy-snapshots.html)

Instead of providing an extra branch per repo, we provide one repo/branch for the whole organisation.

## .travis.yml

In your github project, use a .travis.yml like this one:

```yaml
# this is a java project
language: java
jdk:
- oraclejdk7
# checkout the this project to get the settings.xml before build
before_install: "git clone https://github.com/holisticon/travis-configuration.git target/travis"
# build, package and deploy snapshot to sonatype repo
script: "mvn -q clean deploy --settings target/travis/settings.xml"
# whitelist: only use master
branches:
  only:
  - master
# encrypted credentials for sonatype
env:
  global:
  - secure: <YOUR_ENCRYPTED_SONATYPE_USERNAME_HERE>
  - secure: <YOUR_ENCRYPTED_SONATYPE_PASSWORD_HERE>
```

You can get your projects encrypted credentials strings by using the [travis command line client](http://docs.travis-ci.com/user/build-configuration/#Secure-environment-variables):

```bash
cd <YOUR_LOCAL_REPO_CLONE>
travis login
travis encrypt SONATYPE_USERNAME=<YOUR_SONATYPE_USER>
travis encrypt SONATYPE_PASSWORD=<YOUR_SONATYPE_PASSWORD>
```
