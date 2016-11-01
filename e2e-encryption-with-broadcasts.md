# End to End Encryption with Broadcasts

## Context

**Alice** would like to broadcast messages without concerning herself with the specific people that might be listening at this moment, as long as she has granted each of them access (She wants to emit destination-less broadcast messages).

**Bob** is one of those people she has given explicit access to, and he is presently subscribed to her broadcast messages.

**Eve** has not whitelisted by Alice, but is able to intercept the message in some way, and potentially has the ability to see Alice's device.

## Setup

1. **Bob** generates a public/private key pair and makes the public key world accessible somewhere. In this case, we'll say he stores it as the `publicKey` property on his device. He then stores the private key locally somewhere.

2. **Alice** generates an AES key and stores it locally somewhere. She then uses **Bob's** public key to encrypt the AES key and stores it on the `keys.bobs-uuid.key` property on her device. She also adds **Bob** to her `discover.view` whitelist.

3. **Bob** subscribes to **Alice's** `broadcast.sent` messages (and his own `broadcast.received` messages)

## Execution

1. **Alice** stringifies her message and encrypts it using her AES key.

2. **Alice** emits the message as a broadcast message (`broadcast.sent`) with the encrypted portion under the key `encrypted`.

3. **Bob** receives the message with the encrypted `encrypted` value.

4. **Bob** retrieves **Alice's** device and uses his private key to decrypt the AES key on her `keys.bobs-uuid.key` property.

5. **Bob** uses the AES key to decrypt the broadcast message.

6. **Eve** cannot decrypt the message, since she does not have access to an unencrypted version of **Alice's** AES key.
