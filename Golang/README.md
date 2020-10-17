# Overview
- [Overview](#overview)
  - [Questions](#questions)
  - [Misc](#misc)
    - [Package, Module and Repos](#package-module-and-repos)
  - [Commands](#commands)
  - [Keywords](#keywords)
  - [References](#references)

## Questions 

## Misc

Executable commands must always use package main.

Compile a module using `go install example.com/user/hello` this will create `hello` executable file in `~/go/bin`

**Environment variables**  `GOPATH`, `GOBIN` default value for `GOPATH` is `~/go/bin`

go commands are executed basis current working directory. so `go install example.com/user/hello`  === `go install .` === `go install` 

### Package, Module and Repos

**Package** directory, collection of multiple source files.

Functions, types, variables, and constants defined in one source file are visible to all other source files within the same package.

import path = module path + subdirectories

Packages in the standard library do not have a module path prefix.

package must be the first command in a go file

**Module** collection of related go packages

**Repository** one or more go modules

## Commands

`go install`

`go env -w GOBIN=/somewhere/else/bin` & `go env  -u GOBIN` to set/unset go env variables

`go get golang.org/x/tour`

## Keywords

go package, go module, go commands

go tool

fetch, build, install a go package/module/command

package

`go.mod` 

import path, to import package

## References

https://golang.org/doc/code.html