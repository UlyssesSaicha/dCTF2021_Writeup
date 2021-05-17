# Discord PingPong

Oh god this one was really annoying to set up

Running the executable returns:

```Successfully connected to Discord API
Last message in #general is: Welcome to our secret server! Only trusted bots and users have access to the secret channels. Our secret plans have been moved from here to a secret channel.
Bot is now running.  Press CTRL-C to exit.
```

Sending anything through stdin wouldn't yield anything. 

running `file` on it:

`discordbot: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, Go BuildID=_ZEAdEO-wvSQGBQYhkGM/xFUNbqa5yDNs4HrN4WQF/IfIgpWbUNMnt11lPcn7W/sNa0sC3hsaXC9dymZEeD, not stripped`

So yeah, golang.

Searching google for a Go discord bot returns:

`https://github.com/bwmarrin/discordgo`

And it contains the pingpong example:

`https://github.com/bwmarrin/discordgo/blob/master/examples/pingpong/main.go`

Which also matches this string in the source:
`Bot is now running. Press CTRL-C to exit.` 

However not much of it is useful.. any passed arguments to the bot are ignored.

# Intended solution

Apparently, what was needed to complete the challenge was just to run `strings` on the discordbot file and extract the discord api token, and then get the flag from the secret discord server.

# How I worked it out

So I had originally run `strings` on the file, but it returned so much data and sifting through it would have been hell..

Having some knowledge of Golang requests, I knew that by default the net/http library respects the environment variables for proxies.

Knowing this, launch burp and set the environment variables in my terminal for the HTTP and HTTPS proxies to `127.0.0.1:8080`, and added the Burp Suite CA certificate to `/usr/local/share/ca-certificates`

Helpful links for this:
`https://superuser.com/questions/437330/how-do-you-add-a-certificate-authority-ca-to-ubuntu
https://stackoverflow.com/questions/51845690/how-to-program-go-to-use-a-proxy-when-using-a-custom-transport`

After that, you can see the requests through Burp, you just run the discordbot, get the token, and retrieve the flag from one of the channels using the discord api.

Sorry, I didn't take screenshots, and it was annoying to set up again :/