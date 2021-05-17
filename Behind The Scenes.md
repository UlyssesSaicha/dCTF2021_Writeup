# Behind The Scenes

So we are greeted with an image:

![Pasted image 20210516215938](https://user-images.githubusercontent.com/70921512/118423490-7c81ce80-b69b-11eb-8f15-a40b9475977f.png)

Look at it long enough, and you will see in the background a virtual machine running linux, with the 'password' in notepad.

However, it's pixelated.

At first I thought that it would be a challenge about somehow separating image layers packed into the png file. I thought this because if you run `zsteg` on the image you get, among other data:

`xmp:CreatorTool=\"Pixelmator Pro 2.0.8`

Googling says:

```
## Pixelmator

Pixelmator is a graphic editor developed for macOS by Lithuanian brothers Saulius and Aidas Dailide, and built upon a combination of open-source and macOS technologies. [Wikipedia](https://en.wikipedia.org/wiki/Pixelmator)
```

However I was wrong.

# Solution

There is a tool named `Depix` which generates pixelated data of a set of characters, and compares it to your pixelated image and roughly replaces it with what it thinks are the letters.

https://github.com/beurtschipper/Depix

At first I was having some issues:

![unknown](https://user-images.githubusercontent.com/70921512/118423520-886d9080-b69b-11eb-8910-a3cf93db85ef.png)

in that the outputted image was the same as the input.

![bts_cropped](https://user-images.githubusercontent.com/70921512/118423538-93c0bc00-b69b-11eb-9357-3f9af6169083.png)

Eventually, it boiled down to having to crop the image pixel perfect:

<img width="90" alt="btsxropped" src="https://user-images.githubusercontent.com/70921512/118423543-96231600-b69b-11eb-94e2-4721ed788df4.png">

And we run it again to get a rough output:

![output](https://user-images.githubusercontent.com/70921512/118423548-9b806080-b69b-11eb-9da3-27364a531ac7.png)

Which we can just figure out from context.
