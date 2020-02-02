# Test *SSH* connection to **GitHub**

This repository is used to test the **SSH** connection to GitHub without using the *ssh-agent* as recommended in GitHub [Docs](https://help.github.com/en/github/authenticating-to-github). Some of the reasons are exposed [here](http://rabexc.org/posts/pitfalls-of-ssh-agents) and they're serious enough to be worth finding a way to overcome these possible security holes.

For connecting GitHub I was using a dedicated generated ssh key named `rubincced25519` which used *ed25519* digital signature scheme and the key was correctly registered in GitHub.


In many cases I have solved the complex connectivity problems through ssh using the ssh client configuration file: `~/.ssh/config`.

A single host section in this file looks like this:

    Host firezila
        HostName firezila.example.com
        User chefman
        ServerAliveInterval 60
        ServerAliveCountMax 10
        IdentityFile ~/.ssh/chefmaned25519

So I added a new __Host__ section in my `~/.ssh/config` file:

    Host github.com
         HostName github.com
         User git
         ServerAliveInterval 60
         ServerAliveCountMax 10
         IdentityFile ~/.ssh/rubincced25519

After running the connectivity test comand

    $ ssh -T git@github.com
    git@github.com: Permission denied (publickey).

I ended up with the same error message that indicates the connection is still not working as expected even if I pointed directly to the correct key file.

After retesting with more verbose output it seemed that the ssh client still tried to connect using standard rsa key.

Finally the working host section looks like this:

    Host github.com
    #     HostName github.com
    #     User git
    #     ServerAliveInterval 60
    #     ServerAliveCountMax 10
        IdentityFile ~/.ssh/rubincced25519
        IdentitiesOnly yes


I intentionally left the commented lines to illustrate that they were of no importance in this case.