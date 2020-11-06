Setup your own git server
=========================

## Prerequistes

* server with git and openssh installed
* client with git and openssh installed

## server side 


#### SSH read write (authenticated and secure~ access)
first add an user and setup home directory to a git store folder.
all repos are bare object using ".git" as extension

```shell
useradd -c "git-operator" -M -d /mnt/disk/git -U git
passwd git
```

#### HTTP read only (public but eventualy hidden access)

first setup an HTTP(s)? server pointing to /mnt/disk/git
without directory listing but an index.html... for an hidden store

```shell
git clone http://mygit.server.ez/reponame
```
now you can clone repo from HTTP(s)?

#### friendly frontend via HTTP(s)?

You need write both a backend and frontend...

* **backend** to handle git repos information
* **frontend** to present data consistently

and then access your gitserver via a webbrowser or **standalone** one...

## client side


#### Create bare repo and install to your store

we will use git to create a bare repo and then we send it to git server using scp

```shell
git init --bare reponame.git
scp -r ./reponame.git git@mygit.server.ez:~/
```

by now we can directly work with this repo


```shell
git clone git@mygit.server.ez:~/reponame
cd reponame
echo "First commit" >> README.md
git add .
git commit -m "Aye Karamba!"
git push
```

notice ".git" is optional when cloning...


#### re-bare a repository into objects

```shell
git clone --bare source_reponame bare_reponame.git
```

by now you can send that bare repo to your git server using scp

## License
This paper is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.

###### Enjoy,                                               HackIT.
