
# SECURE shell

SSH is not really a Unix shell, such as the Bourne shell or BASH or other Unix interfaces. It’s really a protocol that runs, like most things, over IPv4 or IPv6. Yet the use of the word “shell” in SSH is a good one because there is a lot more to SSH than just remote access. Perhaps the term “secure suite” would have been better, but SSH is what it is.


## SSH ARCHITECTURE


Many SSH components interact to allow secure client-server exchanges


![](https://raw.githubusercontent.com/justinjiajia/img/master/personalwiki/ssh_architecture.PNG)


- Client: The program run on the client (user) device, often *ssh*, but also *scp*, *sftp*, and so on.

- Session: The client/server connection, which can be interactive or batch. The session begins after successful authentication to the server and ends when the connection terminates.

- Key generator: A program (usually *ssh-keygen*) that generates persistent
keys.

- Known hosts: A database of host keys. This is the major authentication
mechanism in SSH.

- Agent: A caching program for user keys to spare users the need to repeat
passphrases. The agent is only a convenience and does not disclose the keys.
The usual agent is *ssh-agent*, and *ssh-add* loads and unloads the key cache.

- Signer: This program signs the host-based authentication packets used instead
of password authentication.

- Random seed: Random data used by SSH components to initialize the
pseudorandom number generators (PRNG) used in SSH.

- Configuration files: Settings to determine the behavior of SSH clients and servers.
