# Set up keys for SSH {#ssh-keys}

*instructions via RStudio and the shell to go here*

In the meantime, here are good instructions:

  * How to do via RStudio? see the end of the section on initial set up:
    - <http://r-pkgs.had.co.nz/git.html#git-init>

## SSH Keys
### About SSH Keys

SSH keys provide a more secure way of logging into a virtual private server with SSH than using a password alone. While a password can eventually be cracked with a brute force attack, SSH keys are nearly impossible to decipher by brute force alone. Generating a key pair provides you with two long string of characters: a public and a private key. You can place the public key on any server, and then unlock it by connecting to it with a client that already has the private key. When the two match up, the system unlocks without the need for a password. You can increase security even more by protecting the private key with a passphrase.

1.  Create the RSA Key Pair
The first step is to create the key pair on your computer:
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
2.  Store the Keys and Passphrase
Once you have entered the Gen Key command, you will get a few more questions:
```
Enter file in which to save the key (/home/demo/.ssh/id_rsa):
```
You can press enter here, saving the file to the user home (in this case, my example user is called demo).
```
Enter passphrase (empty for no passphrase):
```
It's up to you whether you want to use a passphrase. Entering a passphrase does have its benefits: the security of a key, no matter how encrypted, still depends on the fact that it is not visible to anyone else. Should a passphrase-protected private key fall into an unauthorized users possession, they will be unable to log in to its associated accounts until they figure out the passphrase, buying the hacked user some extra time. The only downside, of course, to having a passphrase, is then having to type it in each time you use the Key Pair.

The entire key generation process looks like this:
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+

```
The public key is now located in ```/home/demo/.ssh/id_rsa.pub```
The private key (identification) is now located in ```/home/demo/.ssh/id_rsa```

3. Add the SSH keys to the SSH agent. First ensure the agent is enabled:
```
$ eval "$(ssh-agent -s)"
Agent pid 59566
```
Then add the key to the agent:
```
$ ssh-add ~/.ssh/id_rsa
```

4. Now copy this public key onto your clipboard.

### Mac
```
pbcopy < ~/.ssh/id_rsa.pub
```

### Linux
```
sudo apt-get install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub

```

### Windows
```
$ clip < ~/.ssh/id_rsa.pub
```

5. Go to your profile on [GitHub](GitHub.com)
-   In the top right corner of the page, click your profile photo, then click Settings.

-   In the user settings sidebar, click SSH and GPG keys.

-   Click New SSH key.

-   In the "Title" field, add a descriptive label for the new key. For   example, if you're using a personal Mac, you might call this key "Personal MacBook Air".

-   Paste your key into the "Key" field.

-   Click Add SSH key.

-   Confirm the action by entering your GitHub password
