Setup your own git server
=========================

## Prerequistes

* server with http git and openssh installed
* client with git and openssh installed

## server side 


#### SSH (authenticated, secure read/write access, single user)

first add an user and setup home directory to a git store folder.
all repos are bare object using ".git" as extension


```shell
useradd -c "git-operator" -M -d /mnt/disk/git -U git
passwd git
```

/mnt/disk/git is the home directory git user and it is the place from your bare repos


#### HTTP (public eventually hidden read only access, single user)

first setup an HTTP(s)? server pointing to /mnt/disk/git
without directory listing just an index.html... for an hidden store


#### HTTP (public eventually hidden read only access, multi user)

root dir from the web server should be the parent directory from all git users
again without directory listing just an index.html... for an hidden store.

specific user setup is required. Not to let your users doing anything else than working on their repos.

```shell
useradd -c "git-user" -M -d /mnt/disk/git -s /usr/bin/git-shell -U <username>
passwd <username>
```

#### friendly frontend via HTTP(s)?

You need write both a backend and frontend...

* **backend** to handle git repos information

The backend might be an automate... scanning stuff periodically and on demand,

Then storing goods into database(s)?... 

* **frontend** to present data with consistencies

The frontend might just fetch datas with stored queries and then present things...


All done, you can access your gitserver via a webbrowser or **standalone** one...

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

## License
This paper is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.

###### Enjoy,                                               HackIT.

###### BTC: bc1qsclcm8jjld2lfqqurarwej3xuxhxxxptmnn3mn

