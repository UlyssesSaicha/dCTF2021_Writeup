# Strong Password

Another very easy crypto challenge that took me way too long.

To solve we just have to use `john` on the .zip file we are provided.

However, what ended up causing me massive headaches is that John The Ripper for some reason handles files differently depending on how you add them as parameters. Let me explain;

Running:
`john-the-ripper --format=zip ziphash.txt --wordlist rockyou.txt`

_Should_ theoretically work on this challenge, however it doesnt. John handles the wordlist file incorrectly, and the only way I found out about it was through the output screen:
```Warning: invalid UTF-8 seen reading rockyou.txt Using default input encoding: UTF-8```

It seems fine, and it runs for various seconds, however, it doesn't successfully find the password.

Provide the same parameters differently:

```
john-the-ripper --format=zip output.txt --wordlist=rockyou.txt
```

and it runs for MANY more minutes (from around 25 seconds to 4 minutes on an i7-10700k). Eventually, we get the password:

`Bo38AkRcE600X8DbK3600`

and we can decrypt the file, and get the flag:

`dctf{r0cKyoU_f0r_tHe_w1n}`

# Notes

Maybe i'm just not familiar enough with linux and this is supposed to be intended, but idk. Let me know...