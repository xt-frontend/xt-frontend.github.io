---
layout: post
title: "안드로이드 프로젝트 세팅"
author: kimgyutae
categories: [React]
tags: [React]
image: assets/images/logo-xt-lg.png
toc: false
---

**brew설치**

**1. /opt 디렉토리 이동**

cd /opt

**2. Homebrew 디렉토리를 만든다 (root 권한 필요)**

sudo mkdir homebrew

**3. /opt/homebrew 디렉토리의 소유권을 부여(root 권한이 필요 없도록)**

sudo chown -R $(whoami) /opt/homebrew

**4. homebrew를 다운로드하고 압축을 푼다.**

curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew

**5. homebrew/bin 디렉토리를 PATH에 추가한다. (zsh를 사용하지 않을 경우 직접 해야함)**

echo "export PATH=/opt/homebrew/bin:$PATH" >> ~/.zshrc

**6. 마지막으로 아래 명령어 실행**

/bin/bash -c "$(curl -fsSL https://gist.githubusercontent.com/nrubin29/bea5aa83e8dfa91370fe83b62dad6dfa/raw/48f48f7fef21abb308e129a80b3214c2538fc611/homebrew_m1.sh)"

——————————————————————————————————————

저장소 만들기

brew tap adoptopenjdk/openjdk

——————————————————————————————————————

Cask 설치

Brew install —cask cask

——————————————————————————————————————

jdk설치

Brew install —cask adoptopenjdk8

Brew install —cask adoptopenjdk11

——————————————————————————————————————

Grade 설치

Brew install gradle

——————————————————————————————————————

vi .zshrc

export PATH=/opt/homebrew/bin:$PATH

export PATH=/opt/homebrew/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin

export JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home

export PATH=$PATH:$JAVA_HOME/bin

export ANDROID_HOME=$HOME/Library/Android/sdk

export PATH=$PATH:$ANDROID_HOME/emulator

export PATH=$PATH:$ANDROID_HOME/tools

export PATH=$PATH:$ANDROID_HOME/tools/bin

export PATH=$PATH:$ANDROID_HOME/platform-tools

eval "$(/opt/homebrew/bin/brew shellenv)"

~

:x로 나와야 저장됨

——————————————————————————————————————

source .zshrc
