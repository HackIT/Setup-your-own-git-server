Setup your own git server
=========================

## Prerequistes

* server with http git and openssh installed
* client with git and openssh installed

We will assume we are using GNU/Linux®.

This document is targeted to intermediate hackerz.

I assume that readers knows a little about networking and services.


## server side 


### SSH (authenticated, secure read/write access, single user)

First add an user and setup home directory to a git store folder.
all repos are bare object using ".git" as extension


```shell
useradd -c "git-operator" -M -d /mnt/disk/git -U git
passwd git
```

/mnt/disk/git is the home directory from **git** user and it is the place from your bare repos.



### GIT PROTOCOL  (non authenticated, unsecure read/(possibly write), public access)


Notice the --enable=receive-pack which allow push requests.


Notice also that the server is public... anybody can pull/push from this access...


Git protocol doesn't handle authentication neither a secure layer.


Server:

```
git daemon --verbose --export-all --base-path=/mnt/disk/git --user=git --enable=receive-pack
```


Client:
```
git clone git://server.name/reponame
```


#### HTTP (public eventually hidden read only access, single user)

First setup an HTTP(s)? server pointing to /mnt/disk/git
without directory listing just an index.html... for an hidden store

[git-scm.com/git-http-backend...](https://git-scm.com/docs/git-http-backend)

#### HTTP (public eventually hidden read only access, multi user)

root dir from the web server should be the parent directory from all git users : /mnt/disk
again without directory listing just an index.html... for an hidden store.

specific user setup is required. Not to let your users doing anything else than working on their repos.


```shell
useradd -c "git-user" -M -d /mnt/disk/git -s /usr/bin/git-shell -U <username>
passwd <username>
```

[git-scm.com/git-http-backend...](https://git-scm.com/docs/git-http-backend)

[snackoverflow...](https://stackoverflow.com/questions/6414227/how-to-serve-git-through-http-via-nginx-with-user-password)



#### friendly frontend via HTTP(s)?

##### Homemade app

You need write both a backend and frontend...

* **backend** to handle git repos information

The backend might be an automate... scanning stuff periodically and on demand,

Then storing goods into database(s)?... 

* **frontend** to present data with consistencies

The frontend might just fetch datas with stored queries and then present things...

All done, you can access your gitserver via a webbrowser or **standalone** one...

##### sourcehut

[sourcehut.org website](https://sourcehut.org/)

[sourcehut.org source code](https://sr.ht/~sircmpwn/sourcehut)

Discovered that one few days ago... Seem very well done with lot useful features.

## client side

#### HTTP client access

##### single user, read only

```shell
git clone http://mygit.server.ez/reponame
```

##### multi user, read only

```shell
git clone http://mygit.server.ez/user/reponame
```


#### SSH client access

##### single user

This is single user mode... using /bin/bash as shell.
we will use git to create a bare repo from server side.


```shell
ssh git@mygit.server.ez git init --bare reponame.git
```

by now we can directly work with this repo, using:


```shell
git clone git@mygit.server.ez:~/reponame
cd reponame
echo "First commit" >> README.md
git add .
git commit -m "Aye Karamba!"
git push
```

##### multi user

This is multi user mode... using /usr/bin/git-shell as shell.
To create a bare repo you'll need to request server; usually via HTTP...
frontend/backend required...


```shell
git clone <user>@mygit.server.ez:~/reponame
cd reponame
echo "First commit" >> README.md
git add .
git commit -m "Aye Karamba!"
git push
```

notice ".git" is optional when cloning...


## Hints

#### re-bare a repository into objects


```shell
git clone --bare source_reponame bare_reponame.git
```

by now you can send that bare repo to your git server using scp(single user mode)

#### local git

The concept is simple, you keep your bare repos (reponame.git) somewhere on your local file system...

Then you clone them to work on source code and then push back to your local bare repo...


```shell
mkdir -p /mnt/disk/my_source_code_repository
git init --bare /mnt/disk/my_source_code_repository/example.git

git clone /mnt/disk/my_source_code_repository/example
cd example
echo "I've Modified something :)" >> README.md
git add .
git commit -m "Aye Karamba!"
git push
```

#### git any dir

You can keep trace from everything.

```shell
cd /etc/init.d
git --init
git add .
git commit -m "First commit."
```

### git isn't stupid ;)

## License
This paper is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.

###### Enjoy,                                               HackIT.

###### BTC: bc1q5jw0dsgc4x96l0um6vqexp5kq7wthlfvz944uc

