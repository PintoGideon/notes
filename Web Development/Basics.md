### This is the complete roadmap for a web developer.

### ssh

```
ssh user@host

# Example
ssh toor@fnndsc

```

# A quick note on how ssh works

1. Symmetrical Encryption

A key change alogrithm is implemented for a safer use of shared keys.

2. Asymmetrical Encryption

The client and a host posess a public and a private key. The public keys are linked with private keys in terms of functionality.
The private and the public have a one way relationship.

For example, Suppose a client and a server both send their public keys to each other. The client is going to send a message by encrypting it with the host's public key. The server can decrypt the message using it's own private key.

A message encrypted by a public key of a client can only be decrypted by the priavte key of the client.

```
cd ~/.ssh
ls

# Do you see id_rsa id_rsa.pub

ssh-keygen -C "test@gmail.com"

# Give the folder path and rename the key accordingly.
ls
id_rsa_digitalocean.pub

# You can share the public key with the host

pbcopy < ~/.ssh/id_rsa_digitalocean.pub
ssh {user}@{host}

```
