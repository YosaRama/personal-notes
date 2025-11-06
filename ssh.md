---
description: Explain about what is SSH in general and how it's works
---

# SSH

## Problem

When we have a computer on the server, and then we want to access that computer from our local machine. how can we do that?

## Approach

To connect our local machine to the open world/open network we need some security network protocols one of them is SSH.

SSH (Secure Shell/Secure Socket Shell) is the one of the network protocols to connect between 2 computer in a non-secure network / open network. SSH by default listen to standard Transmission Control Protocol (TCP) port 22.

How SSH works? SSH will create public key pair 1 for local machine to remote machine and the other one is for remote machine to local machine. that's make both of them can be connected in secure way.

SSH will save on `known_hosts` and can be configure on `.ssh/config`, on the first time it will be opening-up prompt to allowed the access to the unknown server hosts. after it accepting it will be directly go into the server machine with terminal simulate.

## Implementation

Before connect each computer we need to generate SSH key/pair (public and private). there are a lot of ssh-keygen algorithm (ex. RSA, ED5519)

```
ssh-keygen -t rsa -b 2048 -f ~/.ssh/key_name
```

There are such a command to connect to the remote machine/server. here is the example

```
ssh <username>@<ServerIP>
```

we can also specify it on .ssh/config

```
Host type-name-easy-to-connect
    HostName <Server IP here>
    User <User Name>
    IdentityFile <SSH File>
```
