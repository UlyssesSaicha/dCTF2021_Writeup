# Behind The Scenes

So we are greeted with an image:

![[Pasted image 20210516215938.png]]

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

![[unknown.png]]

in that the outputted image was the same as the input.

![[bts_cropped.png]]

Eventually, it boiled down to having to crop the image pixel perfect:

![[btsxropped.png]]

And we run it again to get a rough output:

![[output.png]]

Which we can just figure out from context.