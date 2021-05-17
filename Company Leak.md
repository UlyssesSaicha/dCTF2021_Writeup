# Company Leak

![[Pasted image 20210516213709.png]]

README:

```
# Super secret file repository

This zip file contains super secret files, protected by our most advanced encryption systems.
These should never be leaked to the public because it may cause our company to shut down.

## Our evil goals

As described by the conveniently placed evidence which nobody will ever read, our plans are evil.

## If somebody gets the files

Yeah, we're done for. But fortunately, if somebody steals only this file, we should be fine. Probably.
```

The nested ZIP file has two files: 

`-README.md
-top_secret.txt`

Any attempt at cracking the password will fail, although I did try :P

What eventually lead me to the solution was how zip2john gives extra details:

Running zip2john on the zip with the password-protected file yields (among other data):

```
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.
```

This had me thinking, and I compared the password to both files:

```
john-the-ripper.zip2john super_secret.zip -o README.md
Using file README.md as only file to check
ver 2.0 efh 5455 efh 7875 super_secret.zip/README.md PKZIP Encr: 2b chk, TS_chk, cmplen=305, decmplen=481, crc=8A79A8A2
super_secret.zip/README.md:$pkzip2$1*2*2*0*131*1e1*8a79a8a2*0*43*8*131*8a79*8c50*3dd62a466482225cd36f28a8c0c157ce39fb1d019ce0b23f613875f0a19c9f6159006676143fc9a7869f590f53637b7bf69b54ec91490ed91da2bb737342bae62aa12d2f20017de188eec045d1b3edacca6faf83121743e6efc8640c794fce217093fb819ecb74a13b489f857412dcc8032c43085980c0272a8c7123da20eb492d43bf948604fe606fd7b80349f254bb1e068dc88a22c082bae30f0aea1924c24415d61964a9aa9e0172ae9500dc0290fd4c32ec5bf1cf6ec10897fe3e8c7b50abbbcb64396f32d3dfef3399c601f5a6d554830eb90db5ac8c2c21860f48321c01cc74992898914dff6893c03925f01bd72e6b883edf4bd1a5127b69a7ad9bb331dc96a375134cb7953a5265a6305b0c57143c658dc10ed4c03e8942464f112fc79405857de3968e0de8859f3b3627c705*$/pkzip2$:README.md:super_secret.zip::super_secret.zip
```

vs

```
super_secret.zip/top_secret.txt:$pkzip2$1*2*2*0*2a9*543*8283fa7c*184*48*8*2a9*8283*9bc4*70e76b448589dcc9edf8c0d2fd0334fd98523550791d7021096fe6270bb962446945733a9d560aa74865d480dab63e6ea2c2aedba6c14e93a25284dac08df90c0ce2de3d56d3cdf44d208a81c84108137546593303ef0dbb8cd4a5dcb2574b9bf49bdce80f404a1ece4037b336653399b727d8b19841ca29d04ac3a3818998cda5047785588a773aea63349ecee47317c3049045f78bcc6bb462d55d4372ac44bbc55d3f55dacfb5e587379f006ba23cf71a9b32b33a6958204eaf0a8cf9dd693bcaa39d6dea795e51c777f732bc3a8247fd92e17cd3debf325c74521fe5bec511be5a8f193d17543ddff29b33baa015f3e81ce4d619c679b592c839fcd018bb811e92b58723df14db0f91b9e894b536d9fa10e019f7e44a5f96a87e99239e0e6d8497dc47e5589e2675ddd75f17a8b299ac9d358d32cfb1894c4a8cb8d34b6a3fd61966af4233b5cd17d395c9eb7f8a584e5d6fdb08eaf3b256b00c4a76060cfcf95203f72533296efcb1350da56f8b69d2016da599f16b669e11aaff8d7692ec0775b1a4cccf91ad59a42325a223dfc87c70b99e01c72fcff793c1ac12a779b86cde6d81dab8ffdf9e8718484b96907257a4f8cdff8b7e30f861a713415394cc6fa814a1c75f8b89261d67b0b02bc3b821c3a670797c50c598e887bd22e3bb8a84b4d3684ba756088b6d55839510bd7e0859e53e9fd0985780bc5004264e772cf3e4994cfe7e7df148df741cd2a82e611b7737c41f069437b76002ad1ad8a3825c12434c891e936e246d52905e2df9ace071204a33d633efff9e02b00dfee04efbc7e78c08a824cf8a6b311f182a9f36ec5fbcd0681d83d3279991b2da177b3890ac883984ac356baa1860ab3576a6f04dd5affbe97d0d154c9f2e7be5450c1f39fbb29953ab0297302eacec6d6279fc81b4d35c4a9eb04f0253ce43a05bed1ee620089b65c3a558*$/pkzip2$:top_secret.txt:super_secret.zip::super_secret.zip
```

I didn't know if there was any significance about this, but it did tip me off that both of these were different^ and that the file size for both README.md's were the same.

After googling for a while I eventually I landed on this github page:

https://github.com/kimci86/bkcrack

And followed this tutorial:

https://github.com/kimci86/bkcrack/blob/master/example/tutorial.md
