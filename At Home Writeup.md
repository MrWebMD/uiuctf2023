## At Home Writeup

**Author: W3BFederation**

This challenge offers you two files, `chal.txt` and `chal.py`

`chal.txt`

```py
e = 359050389152821553416139581503505347057925208560451864426634100333116560422313639260283981496824920089789497818520105189684311823250795520058111763310428202654439351922361722731557743640799254622423104811120692862884666323623693713
n = 26866112476805004406608209986673337296216833710860089901238432952384811714684404001885354052039112340209557226256650661186843726925958125334974412111471244462419577294051744141817411512295364953687829707132828973068538495834511391553765427956458757286710053986810998890293154443240352924460801124219510584689
c = 67743374462448582107440168513687520434594529331821740737396116407928111043815084665002104196754020530469360539253323738935708414363005373458782041955450278954348306401542374309788938720659206881893349940765268153223129964864641817170395527170138553388816095842842667443210645457879043383345869
```


`chal.py`:

From the top, we can annotate what chal.py is doing.

```py
from Crypto.Util.number import getRandomNBitInteger


# Convert the flag into a number
flag = int.from_bytes(b"uiuctf{******************}", "big")

# Generate lots of huge random numbers

a = getRandomNBitInteger(256)
b = getRandomNBitInteger(256)
a_ = getRandomNBitInteger(256)
b_ = getRandomNBitInteger(256)


# Calculate a modulus (M)
M = a * b - 1

# Calculate a public exponent (e)
e = a_ * M + a

# Calculate a private decryption key (d)
d = b_ * M + b 

# Calcuate a public encryption key (n)

n = (e * d - 1) // M

# Encrypt the flag using the public exponent (e) and the
# encryption key (n)

c = (flag * e) % n

print(f"{e = }")
print(f"{n = }")
print(f"{c = }")
```

The variables `a`, `b`, `a_`, and `b_` are random and too large to brute force. Since the variables `M`, `e` and `n` also use these giant random numbers, it's very hard to calcuate their original values and rebuild the decryption key. 

The only line of code left worth look at is the last line

```
c = (flag * e) % n
```


The modulus operator `%` works like this:

result = 20 % 100

1. How many 100's can go into 20?

2. Zero 100's go into 20,

3. What is the remainder then?

4. result = 20

result = 100 % 20

1. How many 20's can go into 100

2. Five 20's can go into 100

3. What is the remainder

4. result = 0

Notice that when the number on the right of the modulus is greater than the number on the left of the modulus, the left number gets returned.

```py
c = (flag * e) % n
```

We can write our own code to see if `flag * e` less than `n`.

```py
flag = int.from_bytes(b"uiuctf{******************}", "big")

print((flag * e) < n)
# True

```

Now that we know this is the case. We can be sure that `c = (flag * e) % n` boils down to something much more simple `c = flag * e` since `n` is too big to divide into `(flag * e)`. 

The flag can be calculated as follows:

```py
from Crypto.Util.number import long_to_bytes, bytes_to_long

e = 359050389152821553416139581503505347057925208560451864426634100333116560422313639260283981496824920089789497818520105189684311823250795520058111763310428202654439351922361722731557743640799254622423104811120692862884666323623693713
n = 26866112476805004406608209986673337296216833710860089901238432952384811714684404001885354052039112340209557226256650661186843726925958125334974412111471244462419577294051744141817411512295364953687829707132828973068538495834511391553765427956458757286710053986810998890293154443240352924460801124219510584689
c = 67743374462448582107440168513687520434594529331821740737396116407928111043815084665002104196754020530469360539253323738935708414363005373458782041955450278954348306401542374309788938720659206881893349940765268153223129964864641817170395527170138553388816095842842667443210645457879043383345869

print(long_to_bytes(c//e))
# b'uiuctf{W3_hav3_R5A_@_h0m3}'
```