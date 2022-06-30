


https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process

# Introduction

SSH, or secure shell, is a secure protocol and the most common way of safely administering remote servers. Using a number of encryption technologies, SSH provides a mechanism for establishing a cryptographically secured connection between two parties, authenticating each side to the other, and passing commands and output back and forth.

In other guides, we have discussed how to configure SSH key-based access, how to connect using SSH, and some SSH tips and tricks.

In this guide, we will be examining the underlying encryption techniques that SSH employs and the methods it uses to establish secure connections. This information can be useful for understanding the various layers of encryption and the different steps needed to form a connection and authenticate both parties.

# Symmetric Encryption, Asymmetric Encryption, and Hashes
In order to secure the transmission of information, SSH employs a number of different types of data manipulation techniques at various points in the transaction. These include forms of symmetrical encryption, asymmetrical encryption, and hashing.

## Symmetrical Encryption
The relationship of the components that encrypt and decrypt data determine whether an encryption scheme is symmetrical or asymmetrical.

Symmetrical encryption is a type of encryption where one key can be used to encrypt messages to the opposite party, and also to decrypt the messages received from the other participant. This means that anyone who holds the key can encrypt and decrypt messages to anyone else holding the key.

This type of encryption scheme is often called “shared secret” encryption, or “secret key” encryption. There is typically only a single key that is used for all operations, or a pair of keys where the relationship is easy to discover and it is trivial to derive the opposite key.

Symmetric keys are used by SSH in order to encrypt the entire connection. Contrary to what some users assume, public/private asymmetrical key pairs that can be created are only used for authentication, not the encrypting the connection. The symmetrical encryption allows even password authentication to be protected against snooping.

The client and server both contribute toward establishing this key, and the resulting secret is never known to outside parties. The secret key is created through a process known as a key exchange algorithm. This exchange results in the server and client both arriving at the same key independently by sharing certain pieces of public data and manipulating them with certain secret data. This process is explained in greater detail later on.

The symmetrical encryption key created by this procedure is session-based and constitutes the actual encryption for the data sent between server and client. Once this is established, the rest of the data must be encrypted with this shared secret. This is done prior to authenticating a client.

SSH can be configured to utilize a variety of different symmetrical cipher systems, including AES, Blowfish, 3DES, CAST128, and Arcfour. The server and client can both decide on a list of their supported ciphers, ordered by preference. The first option from the client’s list that is available on the server is used as the cipher algorithm in both directions.

On Ubuntu 14.04, both the client and the server are defaulted like this: aes128-ctr, aes192-ctr, aes256-ctr, arcfour256, arcfour128, aes128-gcm@openssh.com, aes256-gcm@openssh.com, chacha20-poly1305@openssh.com, aes128-cbc, blowfish-cbc, cast128-cbc, aes192-cbc, aes256-cbc, arcfour.

This means that if two Ubuntu 14.04 machines are connecting to each other (without overriding the default ciphers through configuration options), they will always use the aes128-ctr cipher to encrypt their connection.

## Asymmetrical Encryption
Asymmetrical encryption is different from symmetrical encryption in that to send data in a single direction, two associated keys are needed. One of these keys is known as the private key, while the other is called the public key.

The public key can be freely shared with any party. It is associated with its paired key, but the private key cannot be derived from the public key. The mathematical relationship between the public key and the private key allows the public key to encrypt messages that can only be decrypted by the private key. This is a one-way ability, meaning that the public key has no ability to decrypt the messages it writes, nor can it decrypt anything the private key may send it.

The private key should be kept entirely secret and should never be shared with another party. This is a key requirement for the public key paradigm to work. The private key is the only component capable of decrypting messages that were encrypted using the associated public key. By virtue of this fact, any entity capable decrypting these messages has demonstrated that they are in control of the private key.

SSH utilizes asymmetric encryption in a few different places. During the initial key exchange process used to set up the symmetrical encryption (used to encrypt the session), asymmetrical encryption is used. In this stage, both parties produce temporary key pairs and exchange the public key in order to produce the shared secret that will be used for symmetrical encryption.

The more well-discussed use of asymmetrical encryption with SSH comes from SSH key-based authentication. SSH key pairs can be used to authenticate a client to a server. The client creates a key pair and then uploads the public key to any remote server it wishes to access. This is placed in a file called authorized_keys within the ~/.ssh directory in the user account’s home directory on the remote server.

After the symmetrical encryption is established to secure communications between the server and client, the client must authenticate to be allowed access. The server can use the public key in this file to encrypt a challenge message to the client. If the client can prove that it was able to decrypt this message, it has demonstrated that it owns the associated private key. The server then can set up the environment for the client.

## Hashing
Another form of data manipulation that SSH takes advantage of is cryptographic hashing. Cryptographic hash functions are methods of creating a succinct “signature” or summary of a set of information. Their main distinguishing attributes are that they are never meant to be reversed, they are virtually impossible to influence predictably, and they are practically unique.

Using the same hashing function and message should produce the same hash; modifying any portion of the data should produce an entirely different hash. A user should not be able to produce the original message from a given hash, but they should be able to tell if a given message produced a given hash.

Given these properties, hashes are mainly used for data integrity purposes and to verify the authenticity of communication. The main use in SSH is with HMAC, or hash-based message authentication codes. These are used to ensure that the received message text is intact and unmodified.

As part of the symmetrical encryption negotiation outlined above, a message authentication code (MAC) algorithm is selected. The algorithm is chosen by working through the client’s list of acceptable MAC choices. The first one out of this list that the server supports will be used.

Each message that is sent after the encryption is negotiated must contain a MAC so that the other party can verify the packet integrity. The MAC is calculated from the symmetrical shared secret, the packet sequence number of the message, and the actual message content.

The MAC itself is sent outside of the symmetrically encrypted area as the final part of the packet. Researchers generally recommend this method of encrypting the data first, and then calculating the MAC.

How Does SSH Work?
You probably already have a basic understanding of how SSH works. The SSH protocol employs a client-server model to authenticate two parties and encrypt the data between them.

The server component listens on a designated port for connections. It is responsible for negotiating the secure connection, authenticating the connecting party, and spawning the correct environment if the credentials are accepted.

The client is responsible for beginning the initial TCP handshake with the server, negotiating the secure connection, verifying that the server’s identity matches previously recorded information, and providing credentials to authenticate.

An SSH session is established in two separate stages. The first is to agree upon and establish encryption to protect future communication. The second stage is to authenticate the user and discover whether access to the server should be granted.

Negotiating Encryption for the Session
When a TCP connection is made by a client, the server responds with the protocol versions it supports. If the client can match one of the acceptable protocol versions, the connection continues. The server also provides its public host key, which the client can use to check whether this was the intended host.

At this point, both parties negotiate a session key using a version of something called the Diffie-Hellman algorithm. This algorithm (and its variants) make it possible for each party to combine their own private data with public data from the other system to arrive at an identical secret session key.

The session key will be used to encrypt the entire session. The public and private key pairs used for this part of the procedure are completely separate from the SSH keys used to authenticate a client to the server.

The basis of this procedure for classic Diffie-Hellman is:

Both parties agree on a large prime number, which will serve as a seed value.
Both parties agree on an encryption generator (typically AES), which will be used to manipulate the values in a predefined way.
Independently, each party comes up with another prime number which is kept secret from the other party. This number is used as the private key for this interaction (different than the private SSH key used for authentication).
The generated private key, the encryption generator, and the shared prime number are used to generate a public key that is derived from the private key, but which can be shared with the other party.
Both participants then exchange their generated public keys.
The receiving entity uses their own private key, the other party’s public key, and the original shared prime number to compute a shared secret key. Although this is independently computed by each party, using opposite private and public keys, it will result in the same shared secret key.
The shared secret is then used to encrypt all communication that follows.
The shared secret encryption that is used for the rest of the connection is called binary packet protocol. The above process allows each party to equally participate in generating the shared secret, which does not allow one end to control the secret. It also accomplishes the task of generating an identical shared secret without ever having to send that information over insecure channels.

The generated secret is a symmetric key, meaning that the same key used to encrypt a message can be used to decrypt it on the other side. The purpose of this is to wrap all further communication in an encrypted tunnel that cannot be deciphered by outsiders.

After the session encryption is established, the user authentication stage begins.

Authenticating the User’s Access to the Server
The next stage involves authenticating the user and deciding access. There are a few different methods that can be used for authentication, based on what the server accepts.

The simplest is probably password authentication, in which the server simply prompts the client for the password of the account they are attempting to login with. The password is sent through the negotiated encryption, so it is secure from outside parties.

Even though the password will be encrypted, this method is not generally recommended due to the limitations on the complexity of the password. Automated scripts can break passwords of normal lengths very easily compared to other authentication methods.

The most popular and recommended alternative is the use of SSH key pairs. SSH key pairs are asymmetric keys, meaning that the two associated keys serve different functions.

The public key is used to encrypt data that can only be decrypted with the private key. The public key can be freely shared, because, although it can encrypt for the private key, there is no method of deriving the private key from the public key.

Authentication using SSH key pairs begins after the symmetric encryption has been established as described in the last section. The procedure happens like this:

The client begins by sending an ID for the key pair it would like to authenticate with to the server.
The server check’s the authorized_keys file of the account that the client is attempting to log into for the key ID.
If a public key with matching ID is found in the file, the server generates a random number and uses the public key to encrypt the number.
The server sends the client this encrypted message.
If the client actually has the associated private key, it will be able to decrypt the message using that key, revealing the original number.
The client combines the decrypted number with the shared session key that is being used to encrypt the communication, and calculates the MD5 hash of this value.
The client then sends this MD5 hash back to the server as an answer to the encrypted number message.
The server uses the same shared session key and the original number that it sent to the client to calculate the MD5 value on its own. It compares its own calculation to the one that the client sent back. If these two values match, it proves that the client was in possession of the private key and the client is authenticated.
As you can see, the asymmetry of the keys allows the server to encrypt messages to the client using the public key. The client can then prove that it holds the private key by decrypting the message correctly. The two types of encryption that are used (symmetric shared secret, and asymmetric public-private keys) are each able to leverage their specific strengths in this model.

Conclusion
Learning about the connection negotiation steps and the layers of encryption at work in SSH can help you better understand what is happening when you login to a remote server. Hopefully, you now have a better idea of relationship between various components and algorithms, and understand how all of these pieces fit together.


references:

https://web.mit.edu/rhel-doc/5/RHEL-5-manual/Deployment_Guide-en-US/ch-openssh.html

# SECURE SHELL

SSH is not really a Unix shell, such as the Bourne shell or BASH or other Unix interfaces. It's really a protocol that runs, like most things, over IPv4 or IPv6. Yet the use of the word “shell” in SSH is a good one because there is a lot more to SSH than just remote access. Perhaps the term “secure suite” would have been better, but SSH is what it is.




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
