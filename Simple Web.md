# Simple Web
As the name suggests, simple web is rather simple

URL: http://dctf1-chall-simple-web.westeurope.azurecontainer.io:8080/

![[Pasted image 20210516142842.png]]

Submitting this returns
`Not authorized`

Looking at the post request:
`flag=1&auth=0&Submit=Submit`

Simply changing `auth` to `1` returns the flag:

`There you go: dctf{w3b_c4n_b3_fun_r1ght?}`