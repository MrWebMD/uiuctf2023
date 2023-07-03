# Three Time Pad Writeup 

**Author: W3BFederation**

In the three-time-pad challenge there are three cipher texts denoted by `c1`, `c2`, `c3` and one plaintext message denoted by `p`.

Using the cyberchef "To Hex" functionality on the three files given to us, we can represent the data in hex.

```
c1: 14f5f95b4a252948a8aef177d6c92d82e3016362bd7463f41f40a00ad9e0ccad911b959ef8dfad5f1cc4481ecb64

c2: 06e2f65a4c256d0ba8ada164cecd329cae436069f83476e91757e91bd4a4cce2c60a8f9aac8cb14210d55253cd787c0f6a

c3: 03f9ea574c267249b2b1ef5d91cd3c99904a3f75873871e94157df0fcbb5d1eab94f9386

p2: printed on flammable material so that spies could
    7072696e746564206f6e20666c616d6d61626c65206d6174657269616c20736f207468617420737069657320636f756c64
```

In order to calcuate the cipher text of an xor operation, a key must used.

```
p (plaintext) xor k (key) = c (cipher text)
```

An xor operation can also be used to calculate the k (key) using the plaintext and cipher text.

```
p (plaintext) xor c (cipher text) = k (key)
```

All three cipher text above `c1`, `c2`, `c3` all used the same key for xor.

We have the cipher text for `c2` and the plaintext of `c2` which is `p2`. 

This means we can calculate `k` and decrypt all of the other cipher texts.

## c2 xor p2 = k

```
c2: 06e2f65a4c256d0ba8ada164cecd329cae436069f83476e91757e91bd4a4cce2c60a8f9aac8cb14210d55253cd787c0f6a

xor 

p2: 7072696e746564206f6e20666c616d6d61626c65206d6174657269616c20736f207468617420737069657320636f756c64

= 

k: 76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e

```

Here is the [Cyber chef formula](https://cyberchef.org/#recipe=From_Hex('Auto')XOR(%7B'option':'Hex','string':'7072696e746564206f6e20666c616d6d61626c65206d6174657269616c20736f207468617420737069657320636f756c64'%7D,'Standard',false)To_Hex('None',0)&input=MDZlMmY2NWE0YzI1NmQwYmE4YWRhMTY0Y2VjZDMyOWNhZTQzNjA2OWY4MzQ3NmU5MTc1N2U5MWJkNGE0Y2NlMmM2MGE4ZjlhYWM4Y2IxNDIxMGQ1NTI1M2NkNzg3YzBmNmE)

Now that k is calculate all of the other ciphers can be decrypted using xor. 

```
c1 xor k = p1

14f5f95b4a252948a8aef177d6c92d82e3016362bd7463f41f40a00ad9e0ccad911b959ef8dfad5f1cc4481ecb64

xor 

76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e

= before computers, one-time pads were sometimes
```

[Cyber chef formula](https://cyberchef.org/#recipe=From_Hex('Auto')XOR(%7B'option':'Hex','string':'76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e'%7D,'Standard',false)&input=MTRmNWY5NWI0YTI1Mjk0OGE4YWVmMTc3ZDZjOTJkODJlMzAxNjM2MmJkNzQ2M2Y0MWY0MGEwMGFkOWUwY2NhZDkxMWI5NTllZjhkZmFkNWYxY2M0NDgxZWNiNjQ)

```
c2 xor k = p2

06e2f65a4c256d0ba8ada164cecd329cae436069f83476e91757e91bd4a4cce2c60a8f9aac8cb14210d55253cd787c0f6a

xor 

76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e

= printed on flammable material so that spies could
```

[Cyber chef formula](https://cyberchef.org/#recipe=From_Hex('Auto')XOR(%7B'option':'Hex','string':'76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e'%7D,'Standard',false)&input=MDZlMmY2NWE0YzI1NmQwYmE4YWRhMTY0Y2VjZDMyOWNhZTQzNjA2OWY4MzQ3NmU5MTc1N2U5MWJkNGE0Y2NlMmM2MGE4ZjlhYWM4Y2IxNDIxMGQ1NTI1M2NkNzg3YzBmNmE)

```
p3 = c3 xor k


03f9ea574c267249b2b1ef5d91cd3c99904a3f75873871e94157df0fcbb5d1eab94f9386

xor 

76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e

= uiuctf{burn_3ach_k3y_aft3r_us1ng_1t}
```

[Cyber Chef formula](https://cyberchef.org/#recipe=From_Hex('Auto')XOR(%7B'option':'Hex','string':'76909f343840092bc7c38102a2ac5ff1cf210c0cd859179d7225807ab884bf8de67ee7fbd8acc23279b02173ae1709630e'%7D,'Standard',false)&input=MDNmOWVhNTc0YzI2NzI0OWIyYjFlZjVkOTFjZDNjOTk5MDRhM2Y3NTg3Mzg3MWU5NDE1N2RmMGZjYmI1ZDFlYWI5NGY5Mzg2)