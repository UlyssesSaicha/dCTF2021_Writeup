# Very Secure Website

![Pasted image 20210516145338](https://user-images.githubusercontent.com/70921512/118423895-76402200-b69c-11eb-8335-9eba799a46a4.png)

The source code provided was:
```
 <?php
    if (isset($_GET['username']) and isset($_GET['password'])) {
        if (hash("tiger128,4", $_GET['username']) != "51c3f5f5d8a8830bc5d8b7ebcb5717df") {
            echo "Invalid username";
        }
        else if (hash("tiger128,4", $_GET['password']) == "0e132798983807237937411964085731") {
            $flag = fopen("flag.txt", "r") or die("Cannot open file");
            echo fread($flag, filesize("flag.txt"));
            fclose($flag);
        }
        else {
            echo "Try harder";
        }
    }
    else {
        echo "Invalid parameters";
    }
?> 
```

Upon looking up the hash for the username, we can find online that it has already been dehashed 1000 times, and it's value is `admin`.

This one was hard, specially because it was the first challenge I tried and I don't know PHP.

I looked at the source code for what felt like were 20 minutes straight until I realized that the implementation was tight, there was nothing I could inject into it (atleast not with my level of knowledge) that would allow me to purposefully evaluate the if statement to true.

The only thing that popped up to me was the hash for the password.

The fact that in a sequence that long, there was only one letter stood out a LOT.


# Solution

Eventually I found out about how PHP handles loose comparison `==` vs tight comparison `===`:

With enough googling you will find that the hash does only have one letter for a reason.

With loose comparison, it parses `0e132798983807237937411964085731` as a float number (scientific notation, anyone?)

Which means that while comparing it to something else, it will evaluate to `0`. All we need to do now is provide a value that when hashed, will return a hash which also resembles `0`.

Luckily, some people have done the work for us:

https://github.com/spaze/hashes/blob/master/tiger128%2C4.md

Now just pick any value, and this is what the result would look like:

`else if (hash("tiger128,4", "LnFwjYqB") == "0e132798983807237937411964085731")`

which would be converted to:

`"0e087005190940152635463034029558" == "0e132798983807237937411964085731")`

which gets converted to...

`"0" == "0"`

Which evaluates to TRUE.


And we input that to get the flag:

`dctf{It’s_magic._I_ain’t_gotta_explain_shit.}`

