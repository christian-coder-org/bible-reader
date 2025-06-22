# ssh-git-local-repos-setup

How to setup a local repo with its own .git command line ssh key.

#### First create a new SSH key pair in `~/.git`.

Use the same password for the GitHub login in the password sheet.

```sh
ssh-keygen -t ed25519 -C "christian-coder@outlook.com"

# Output
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/warren/.ssh/id_cco): /Users/warren/.ssh/id_cco
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/<home>/.ssh/id_cco
Your public key has been saved in /Users/<home>/.ssh/id_cco.pub
The key fingerprint is:
SHA256:nUDYbW1J/V2b19EdbdMYFhYw8k7wM+F3vvD02CLD4dU christian-coder@outlook.com
The key's randomart image is:
+--[ED25519 256]--+
|       o..oo=+***|
|      ... o*+=ooB|
|        .. .B .oX|
|         o + + *=|
|        S o o...E|
|           o o+oo|
|            = ooo|
|             o . |
|                 |
+----[SHA256]-----+
```

#### Add the new SSH key to the `ssh` agent.

```sh
ssh-add -k ~/.ssh/id_cco
```

#### Add the `sshCommand` to the local `.git/config` file to point to the SSH key.

```sh
git config --local core.sshCommand "ssh -i ~/.ssh/id_cco"
```

#### Add `user.name` to the local `.git/config` file.

```sh
git config --local user.name "Christian Coder"
```

#### Add `user.email` to the local `.git/config` file to point to the `no-reply` email address.

```sh
git config --local user.email "203032652+christian-coder-org@users.noreply.github.com"
```

## Changing account using lcoal `ssh` agent

Changing between accounts.

### Push with a local account (christian-coder-org)

```
ssh-add -D
ssh-add ~/.ssh/id_cco
git push
```

### Push with my main account (wkande)

```
ssh-add -D
ssh-add ~/.ssh/id_rsa
git push
```

## Add ssh key to GitHub

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?tool=webui
