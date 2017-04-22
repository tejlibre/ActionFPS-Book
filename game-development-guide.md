# Game Development Guide

## Download latest release

Go here: https://github.com/ActionFPS/ActionFPS-Game/releases

Releases are built automatically for Windows (using AppVeyor) and Linux (using Travis). For Mac automated builds, we need a [CircleCI subscription](https://circleci.com/pricing/#build-os-x).

An admin of the project can [create a release here](https://github.com/ActionFPS/ActionFPS-Game/releases).

## Test server

We're running an always up-to-date server (synchronized with the branch that is being test):

```
/connect woop.ac 7654
```

## Run from source

This is for development and testing. First, [clone the repository](https://help.github.com/articles/cloning-a-repository/).

### Windows
1. Install Windows Visual C++ Studio Express 2010 
2. Open `source/vcpp/cube.vcxproj` and build "Release"
3. Launch `actionfps_release.bat`.

### Linux

```
$ cd source/src
$ make install
$ cd ../..
$ ./actionfps.sh
```

### Mac

1. Install XCode
2. Dependencies with [brew](https://brew.sh/): `brew install openssl jq`
3. Compile: `cd source/xcode && make && cd ..`
4. Artifact is built in: `source/xcode/build/Release/actionfps.dmg`.

Also see auto-build: [circle.yml](https://github.com/ActionFPS/ActionFPS-Game/blob/master/circle.yml).

## Contribute

**Help us with the first release: https://github.com/ActionFPS/ActionFPS-Game/milestone/2**

## Release process

```
$ git checkout master
$ git commit --allow-empty -m "Release 'version_1234'"
$ git tag 'version_1234'
$ git push origin HEAD --tags 
```

Wait a while (for build to complete),
go to https://github.com/ActionFPS/ActionFPS-Game/releases
and select the release you're interested in. 

Then you can 'publish' the release into a full one.

Alternatively, [create a release here](https://github.com/ActionFPS/ActionFPS-Game/releases).
