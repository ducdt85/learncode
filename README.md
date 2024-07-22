# INSTALL DART
```
brew tap dart-lang/dart
brew install dart
dart --version
```
# INSTALL Cocoapod

# INSTALL FLUTTER (https://docs.flutter.dev/get-started/install/macos/mobile-ios)
```
wget https://storage.googleapis.com/flutter_infra_release/releases/stable/macos/flutter_macos_arm64_3.22.3-stable.zip
mkdir ~/work && mv flutter_macos_arm64_3.22.3-stable.zip && cd ~/work
unzip flutter_macos_arm64_3.22.3-stable.zip
vim ~/.bash_profile -> export PATH="$PATH":"$HOME/work/flutter/bin" 
source ~/.bash_profile
flutter doctor -v
```
# CREATE FLUTTER PROJECT and SETUP IT USE WITH MELOS
`mkdir monorepo/{packages,apps} -p`
## Step 1: Create a New Flutter Project
`cd monorepo/apps && flutter create melos_app`
## Step 2: Set Up Melos
### *Initial Melos
```
cd melos_app && vim melos.yaml
name: melos_app
packages:
  - packages/**
```  
### Create the Flutter Sample Package with the name melos_dependency.
`cd monorepo/packages && flutter create -t package melos_dependency`
### *Get version package melos_dependency
```
cat monorepo/packages/melos_dependency/pubspec.yaml 
name: melos_dependency
description: "A new Flutter package project."
version: 0.0.1
```
### *Declare dependencies package to flutter app 
```
vim monorepo/apps/melos_app/pubsec.yaml
dependencies:
  flutter:
    sdk: flutter
  melos_dependency: ^0.0.1
```
## Step 3: Install Melos for flutter
`flutter pub global activate melos`
### *Ensure Global Binaries Are in Your Path. Add Flutter global binaries directory to your PATH
`
vim ~/.bash_profile -> export PATH="$PATH":"$HOME/.pub-cache/bin"
source ~/.bash_profile
`
### *Verify Melos Installation
`melos --version`

## Step 3: INSTALL MELOS for Dart
### *Active melos globally
`dart pub global activate melos`
### *Ensure Global Binaries Are in Your Path. Add Dart’s global binaries directory to your PATH
```
vim ~/.bash_profile -> export PATH="$PATH":"$HOME/.pub-cache/bin"
source ~/.bash_profile
```
### *Verify Melos Installation
```melos --version```

## Step 4: SETUP A WORKSPACE
### *Melos is designed to work with a workspace. A workspace is a directory which contains all the packages that are going to be developed together. Its root directory must contain a melos.yaml and a pubspec.yaml file.
### *The following is the recommended workspace directory structure
```
my_project
├── apps
│   ├── apps_1
│   └── apps_2
├── packages
│   ├── package_1
│   └── package_2
├── melos.yaml
├── pubspec.yaml
└── README.md
```
### *Install Melos in the workspace
```
vim monorepo/pubspec.yaml
name: melos_presentation_workspace

environment:
  sdk: '>=3.0.0 <4.0.0'

dev_dependencies:
  melos: ^3.0.0
```
### *Configure the workspace  
```
vim monorepo/melos.yaml
name: MelosPresentation

packages:
  - apps/**
  - packages/**

command:
  verion:
    # Only allow versioning to happen on main brach
    branch: main
    # Generates a link to a prefilled GitHub release creation page
    releaseUrl: true

scripts:
  format:
    run: melos exec dart format . --fix
    description: Run `dart format` for all packages
```
### *BOOTSTRAPPING the Workspace (Bootstrap command look for pubspec.yaml then they try to connect all those packages together (locally connected) = which installs dependencies for all packages and sets up symlinks)
```melos bootstrap```

## Step 5: Usage of Melos




