### This is the complete roadmap for a web developer.

### ssh

```
ssh user@host

# Example
ssh toor@fnndsc

```

### A quick note on how ssh works

1. Symmetrical Encryption

   A key change alogrithm is implemented for a safer use of shared keys.

2. Asymmetrical Encryption

   The client and a host posess a public and a private key. The public keys are linked with private keys in terms of functionality.
   The private and the public have a one way relationship.

   For example, Suppose a client and a server both send their public keys to each other. The client is going to send a message by encrypting it with the host's public key. The server can decrypt the message using it's own private key.

   A message encrypted by a public key of a client can only be decrypted by the priavte key of the client.

```bash
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


# Look for a .ssh file on the host.

ls
authorized_keys known_hosts

# Add your public key to authorized keys through an editor


```

### Setting up a ssh key on Github.

Github recommends the following command to generate a key-value pair

```bash
ssh-keygen -t rsa -b 4096 -C "test@example.com"
pbcopy < ~/.ssh/id_rsa_github.pub


# Add a new key through the settings in your github account.
# Add your identity

ssh-add ~/.ssh/id_rsa_github

# Useful commands to list and delete identities
ssh-add -l
ssh-add -D

```

### Performance

Internet speeds vary.

HTML, CSS and Javascript can be minified by using uglify.js.
There are mainly 4 types of images.

1. SVG
2. JPG
3. GIF
4. PNG

If you want simple icons, logos and illustrations, use SVGs.
Display different sized images for different backgrounds using media queries. The browser also optimizes the code for us by choosing what it actually needs (probably known as lazy loading)

JPEG Optimizers and TinyPNG are two good websites to reduce the size of the images.

### Reducing download frequency

1. Minimize all Text
2. Minimize images
3. Media queries
4. Minimize the number of files.

### Critical render path

We make a request to the server and the server responds with an HTML file. The browser reads the html file and the incrementally generates the tree model (DOM). It will request for the css file, which will also build a CSS object model. If the browser comes across the js file, it will update the document object model accordingly. The browser combines the CSS object model and the DOM to create a render tree.

The browser is going to figure the positioning of all the element and paint the pixels.
DOM----->CSSOM------>Render Tree (DOMContentLoaded)------->Layout------>Paint

This is the critical render path.
We can write scripts to load stylesheets as per our need. When the browser comes across the js tag, it will request the server for the js script which can manipulate both the DOM and the CSSOM before rendering the tree.

PageSpeedInsights and WebPageTest are two websites which help us gauge performance issues.

### React

The package-lock.json makes sure the dependencies numbers are locked in.
