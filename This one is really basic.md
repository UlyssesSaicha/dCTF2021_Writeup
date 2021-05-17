# This one is really basic

The answer to life, the universe, and everything.

# Solution

So we are given a cipher.txt file which is a file with thousands upon thousands of characters

It's encoded with base64 recursively. We tried to use CyberChef or the basecrack repo on github. However none of them would work.

The hint was that the b64 encoded string started with Vm0, searching for b64 Vm0 revealed that for some reason, if you base64 encode a string enough times it will begin to always start with the same string...

This meant that it wasn't switching with other base types such as base32 or base58.

So I just wrote the following script:
```
import base64
# original = open("cipher.txt", r)

with open("cipher.txt", "r") as f:
	b64 = base64.b64decode(f.read()).strip()
	for i in range(42):
		b64 = base64.b64decode(b64)
		print(b64)
```

eventually it gives an exception and crashes, but good enough, it gives us the flag:

`dctf{Th1s_l00ks_4_lot_sm4ll3r_th4n_1t_d1d}`