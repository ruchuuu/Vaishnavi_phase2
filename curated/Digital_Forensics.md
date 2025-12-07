# 1. Hide and Seek
> Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do? Hint: Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.
## Solution:
- Using the hint, execute stegseek command with sakamoto.jpg to crack the password of the image and along with any data hidden in it.
- It shows that there is a flag.txt file in the image that is then extracted to sakamoto.jpg.out.
- See what type of file sakamoto.jpg.out is using file command (ASCII text file).
- Use cat to read the flag.
```
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ stegseek sakamoto.jpg
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".

vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ file sakamoto.jpg.out
sakamoto.jpg.out: ASCII text
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ cat sakamoto.jpg.out
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```
## Flag:
```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```
## Concepts Learnt:
- stegseek: used to brute-force or crack the password of an image file that has hidden data embedded using the steghide command.
## Resources:
- https://github.com/RickdeJager/stegseek
