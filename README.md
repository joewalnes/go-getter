go-getter: Like "go get", but different
=======================================

A very small shell script that fetches Go packages pinned to your preferred
version.

For creating consistent and repeatable builds.


5 second tutorial
-----------------

To install specific git revisions of Go packages into your `GOPATH`:
````
$ cat Gofile
github.com/lib/pq                      8910d1c3a4bda5c97c50bc38543953f1f1e1f8bb         git:ssh
github.com/julienschmidt/httprouter    b59a38004596b696aca7aa2adccfa68760864d86         git
github.com/hashicorp/golang-lru        d85392d6bc30546d352f52f2632814cde4201d44         git:http
bitbucket.org/someuser/somerepo        12-dfjkjddfddf                                   hg:ssh

$ go-getter Gofile
````
The third field is the VCS:TRANSPORT. The default VCS is git and the default TRANSPORT is https. Any of the 2 missing fields will default to them.

Uhh that's it.

Background
----------

If you program [Go](https://golang.org/) you are no doubt familiar with its
package manager `go get`.

Like everything about Go, it's fantastically opinionated: you depend on the
HEAD version of a package and it's the package author's responsibility to
ensure HEAD *always* contains a good version and maintains backwards
compatibility.

In a perfect world, that's great. *Except it's not always the case.* Libraries
evolve, behaviors change, APIs evolve, regressions come and go, performance
fluctuates, bad things sometimes appear in HEAD, even if only for a short period
of time.

`go-getter` is an alternative to `go get` that has a stronger opinion: **Given
an identical source tree, building a project tomorrow should give you the same
result as building it today, and yesterday, and in 2 years time.**

Because it sucks when you need to cut an emergency release and the code no
longer compiles due to something out of your control.


Installation
------------

Grab [go-getter](https://raw.githubusercontent.com/joewalnes/go-getter/master/go-getter). It's a very small shell script. Don't forget to `chmod +x`.


Usage
-----

Create a file to declare your package dependencies and git has versions.
I call my file `Gofile` but you can call it anything.

````
# List packages and git hashes of versions you want

github.com/lib/pq                      8910d1c3a4bda5c97c50bc38543953f1f1e1f8bb         git:ssh
github.com/julienschmidt/httprouter    b59a38004596b696aca7aa2adccfa68760864d86         git
github.com/hashicorp/golang-lru        d85392d6bc30546d352f52f2632814cde4201d44         git:http
bitbucket.org/someuser/somerepo        12-dfjkjddfddf                                   hg:ssh
````

Then run:
````bash
$ go-getter Gofile
````

This will ensure that your packages are installed in the correct place in your
`GOPATH`. You can run it repeatedly and it will ensure packages are always at
your desired version.


FAQ
---

#### Why should I agree with your opinion over that of the Go team?

We both have opinions. They're both right. I can't choose for you.

#### You know there are loads of these out there already, right?

Yep, I tried a few. Some of them were written in Go, which lead to a
bootstrapping issue (you had to go get them, but which version were you
getting?). Others were too complicated or confusing for me. I just want
to download dependencies.

#### Are you crazy? This is just a lame 26 line shell script - I could write that

Me too. That's why I did.

#### How does this handle transitive dependencies (ie. deps of deps)?

It doesn't. By design. If you depend libraries that depend on other libraries,
you must explicitly list them, that way you can state the versions of those
libraries too.

#### How do I find the current version of a hash for the package I want to use?

View the latest commit on GitHub. You'll see it there.

#### Do you accept pull requests?

Probably not. Your best bet is to fork it and publicize your better version.

#### Is this available in brew, apt, yum, etc?

No. It's a teeny tiny shell script. Just check it in to your project and run
it from there.

#### I read somewhere that a better way to do this is 'vendoring'?

That basically means taking a snapshot of the code and including it in your
own repository. Yes, it's even better as you are isolated from remote
repositories being deleted. But it's also annoying because you have to check
all the code into your own repo. This is a pragmatic middle ground that gives
you most of the advantages of vendoring, without repository bloat.

#### How do I use this if I'm a library author?

You don't. It's for top-level (main) programs only.

#### What if you have multiple top level programs that require different package versions?

Use different `GOPATH`s. I typically have one GOPATH per main project.

Architectural diagram
---------------------

I love gophers too.

![Gophers ready!](http://i.imgur.com/MmNPB.gif "Gophers ready!")
