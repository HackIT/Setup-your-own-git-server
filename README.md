Setup your own git server
=========================

## Prerequistes

* server with git and openssh installed
* client with git and openssh installed

## server side 

first add an user and setup home directory to a git store folder.
all repos are bare object using ".git" as extension

```shell
useradd -c "git-operator" -M -d /mnt/disk/git -U git
passwd git
```

## client side

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


## re-bare a repository into objects

```shell
git clone --bare source_reponame bare_reponame.git
```

by now you can send that bare repo to your git server using scp

## License
This paper is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.

###### Enjoy,                                               HackIT.
