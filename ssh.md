
references:

https://web.mit.edu/rhel-doc/5/RHEL-5-manual/Deployment_Guide-en-US/ch-openssh.html

# SECURE SHELL

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



## SSH KEYS


Keys are a crucial part of SSH. Almost everything that SSH does involves a key, and often more than one key.

Keys are used as parameters for SSH algorithms such as encryption or
authentication. SSH keys are used to bind the operation to a particular user.

There are two types of SSH keys: symmetric (**shared secret keys**) and asymmetric (**public/private key pairs**).

The private and public key are related in a mathematically clever way:

- data encrypted with one key may be decrypted only with the other member of the pair
- it is infeasible to derive the private key from the public one.


Asymmetric keys are used to establish and exchange short-duration symmetric keys.


- User key: This persistent asymmetric key is used by the SSH clients to
validate the user’s identity. A single user can have multiple keys and
“identities” on a network.

- Host key: This persistent asymmetric key is used by the SSH servers to
validate their identity, as well as the client if host-based authentication is
used. If the device runs a single SSH server process, the host key uniquely
identifies the device. Devices running multiple SSH servers can share a key or
use different host keys.

- Session key: This transient symmetric key is generated to encrypt the data sent between client and server. It is shared during the SSH connection setup to use for encrypted data streams during the session. When the session ends, the key is destroyed. There are several session keys, actually — one in each direction (server to client, and client to server) and others to check integrity of communications.





##


The SSH protocol works on top of the packet-level protocol, and proceeds in the following phases:

-	The client opens a connection to the server. (Note that an attacker may cause the connection to actually go to a different machine.)

- The server sends its public RSA host key and another public RSA key (''server key'') that changes every hour. The client compares the received host key against its own database of known host keys.
At present, SSH is not able to validate keys for hosts that it does not already know. The first time a key is received, the user is
prompted about whether or not to accept it. Once the key of an unknown host is accepted,  it is cached in a “known hosts” file for future reference.

-	The client generates a 256 bit random number using a cryptographically strong random number generator, and chooses an encryption algorithm from those supported by the server (normally IDEA or three-key 3DES). The client encrypts the random number (session key) with RSA using both the host key and the server key, and sends the encrypted key to the server.

- The purpose of the host key is to bind the connection to the desired server host (only the server can decrypt the encrypted session key). The server key is used to make decrypting recorded historic traffic impossible after the server key has been changed (usually every hour) in the event that the host key becomes compromised. The host key is normally a 1024 bit RSA key, and the server key is 768 bits. Both keys are generated using a cryptographically strong random number generator.

- The server decrypts the RSA encryptions and recovers the session key. Both parties start using the session key (until this point, all traffic has been unencrypted on the packet level). The server sends an encrypted confirmation to the client. Receipt of the confirmation tells the client that the server was able to decrypt the key, and thus holds the proper private keys.
At this point, the server machine has been authenticated, and transport-level encryption and integrity protection are in use.


- The user is authenticated to the server. This can happen in a number of ways; the dialog is driven by the client which sends requests to the server. The first request always declares the user name to log in as. The server responds to each request with either ''success'' (no further authentication is needed) or ''failure'' (further authentication is required).

Currently supported authentication methods are:

    -	Traditional password authentication. The password is transmitted over the encrypted channel, and thus cannot be seen by outsiders.
    - A combination of traditional .rhosts or hosts.equiv authentication and RSA-based host authentication. Host authentication works by the server generating a 256 bit challenge, encrypting it with the client's public host key, and sending the encrypted challenge to the client. The client decrypts the challenge, and computes MD5 [7] of the challenge and other information that binds the returned value to the particular session. The client then sends this value to the server; the server makes the corresponding computations and compares the values.
    - Pure RSA authentication. The idea is that possession of a particular private RSA key serves as authentication. The server has a list of accepted public keys. The client requests authentication by a particular key, and the server responds with a challenge similar to that in Rhosts RSA authentication.


- After authentication has been successful, a preparatory phase begins. In this phase, the client sends requests that prepare for the actual session. Such requests include allocation of a pseudo-tty, X11 forwarding, TCP/IP forwarding, etc. Adding new preparatory operations is easy.
After all other requests, the client sends a request to start the shell or to execute a given command. This message causes both sides to enter the interactive session.

- During the interactive session, both sides are allowed to send packets asynchronously. The packets may contain data, open requests for X11 connections, forwarded TCP/IP ports, or the agent, etc. Finally at some point the client usually sends an EOF message. When the user's shell or command exits, the server sends its exit status to the client, and the client acknowledges the message and closes the connection.
