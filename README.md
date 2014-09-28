watch.js
========

This is a node.js script that watches local files and keeps them updated on a remote server. It requires [**node.js**](http://www.nodejs.org), **rsync**, and a server that allows password-less login via **SSH keys**.

```bash
node watch.js source destination
```
>        -i,    --identity=ARG        Specify id_rsa file location (default is `~/.ssh/id_rsa`).
>        -h,    --help                Display this help.
>        -v,    --version             Display version number.


### Example Output:

On every file change, it'll perform an **rsync**:

```bash
> watch.js ../a3/ rsnara@linux.student.university.ca:/home/rsnara/cs240/a3
SUCCESS: rsync -avz -e "ssh -i ~/.ssh/id_rsa" ../a3/ rsnara@linux.student.cs.university.ca:/home/rsnara/cs240/a3
SUCCESS: rsync -avz -e "ssh -i ~/.ssh/id_rsa" ../a3/ rsnara@linux.student.cs.university.ca:/home/rsnara/cs240/a3
SUCCESS: rsync -avz -e "ssh -i ~/.ssh/id_rsa" ../a3/ rsnara@linux.student.cs.university.ca:/home/rsnara/cs240/a3
```

<br>
Installation
============

Grab [**node.js**](http://nodejs.org) and install [**Cygwin**](https://www.cygwin.com/) with **rsync** and **ssh-keygen**.


Once you have all three things installed, clone the repository and install the npm modules:
```bash
> git clone https://github.com/ramanpreetnara/watch.js.git path/to/repo
> cd path/to/repo && npm install
```

> **NOTE:** You can also add ```path/to/repo``` into your PATH environment variable to make the script easier to execute. 


<br>
### Server Configuration:

> **NOTE:** To use this script, your server must allow password-less login via **SSH Keys**. If that's already set up, feel free skip this section.

We'll generate two files: ```id_rsa``` and ```id_rsa.pub``` using **ssh-keygen**. This script requires the generated keys to not be password protected. **Be careful**.

```bash
# follow the instructions
> ssh-keygen
```

Now, append the contents of newly generated ```id_rsa.pub``` to the file ```~/.ssh/authorized_keys``` on your server.
```BASH
# copy over the file (fill in the details here)
> scp ~/.ssh/id_rsa.pub username@server:/temporary/location/id_rsa.pub

# SSH into the server
> ssh username@server

# append to ~/.ssh/authorized_keys
> echo '' >> ~/.ssh/authorized_keys
> cat /temporary/location/id_rsa.pub >> ~/.ssh/authorized_keys

```

To test to see if the key works, simply run ```watch.js source destination```. 

You can also generate a private key using PuTTYgen and try to log in via PuTTY (using the private key). If the key doesn't work, feel free to post an issue (after doing some digging around on google, that is).

<br>
Special thanks:
===============

This script builds on the following two node projects:

> [**getopt**](https://github.com/jiangmiao/node-getopt) - a command line parser that makes the script easier to run <br>
> [**node-rsync**](https://github.com/mattijs/node-rsync) - building and executing rsync commands with Node.js.

Notices:
========
This script relies on [**fs.watch**](http://nodejs.org/api/fs.html#fs_fs_watch_filename_options_listener), which is currently unstable.
