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
***
# 2. Nutrela Chunks
> One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?
## Solution: 
- nutrela.png doesn't open normally which means that it's png chunks are corrupted.
- Opening it in online hexedit shows that headers of the file are in small letters when a proper png file should have capitals (PNG,IDHR,IDAT).
  <img width="1919" height="865" alt="Screenshot 2025-12-07 164421" src="https://github.com/user-attachments/assets/ccc6e5e8-30dc-4ef8-99e2-a2fab0eaac6f" />
- Changing the offsets to make them capitals, we save the file and now can open nutrela.png which has the flag.
<img width="1000" height="1000" alt="nutrela" src="https://github.com/user-attachments/assets/b26f142f-1044-48ca-81ef-4fe58aa359fe" />

## Flag:
```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```
## Concepts Learnt:
- A corrupted png file can pe repaired by changing the offsets of a file to the offsets of a proper png file.
- A valid PNG image must contain an IHDR chunk, one or more IDAT chunks, and an IEND chunk.
## Resources:
- https://hexed.it/
- https://gist.github.com/webmaster128/8cbace94767faf5d8d9b
- https://www.w3.org/TR/REC-png-961001#:~:text=A%20valid%20PNG%20image%20must,chunks%2C%20and%20an%20IEND%20chunk.
***
# 3. NineTails
> Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?
## Solution:
- Opened the .rar file in WinRAR. It was corrupted. Used thr repair option to repair the .rar file.
- It had a .ad1 file which had a .stl file.
- Opened the .ad1 in FTK Imager.
- GIC2024 -> AppData -> Roaming -> Mozilla -> Firefox -> Profiles -> j4gjesg4.default-release
- Extract all files. This has key4.db and logins.json
- Use firefox_decrypt python code to extract passwords and usernames from the logins file.
- This gave the flag in 3 parts.
```
(venv) vaish@DESKTOP-59LUGSG:/mnt/c/CTF$ python firefox_decrypt.py j4gjesg4.default-release
2025-11-29 14:40:49,065 - WARNING - profile.ini not found in j4gjesg4.default-release
2025-11-29 14:40:49,066 - WARNING - Continuing and assuming 'j4gjesg4.default-release' is a profile location

Website:   https://www.rehack.xyz
Username: 'warlocksmurf'
Password: 'GCTF{m0zarella'

Website:   https://ctftime.org
Username: 'ilovecheese'
Password: 'CHEEEEEEEEEEEEEEEEEEEEEEEEEESE'

Website:   https://www.reddit.com
Username: 'bluelobster'
Password: '_f1ref0x_'

Website:   https://www.facebook.com
Username: 'flag'
Password: 'SIKE'

Website:   https://warlocksmurf.github.io
Username: 'Man I Love Forensics'
Password: 'p4ssw0rd}'
```
## Flag: 
```
GCTF{m0zarella_f1ref0x_p4ssw0rd}
```
## Concepts Learnt:
- Firefox stores it's usernames and passwords in .json and .db files. These can be extracted using a famous python script called firefox_decrypt.
- .stl files are 3-D files.
- FTK Imager performs data acquisition, previewing files and deleted files.
## Notes:
- The .stl file was not showing anything upon opening it in a 3D viewer. This is because it wasn't a 3D file. It was a database file that stored more data.
- This file opened in FTK Imager solved the problem of the corrupted file and aquired the data it has.
## Resources:
- https://github.com/unode/firefox_decrypt
- https://sis.binus.ac.id/2023/09/20/ftk-imager-in-digital-forensic/
***
# 4. RAR of the Abyss
> Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.
# Solution:
- Enter the pcap file in Wireshark. Follow TCP stream of packets where length>0.
- Stream 2 gives the rar file that contains the flag. Convert data to raw. Use XOR decrypt to covert to hex and save in binary file.
- Save the file as .rar and open using WinRAR.
- Password is given in TCP stream 4: b3y0ndG00dand3vil
- Use it to open the .rar file. Open flag.txt to get the flag.
## Flag:
```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```
## Concepts Learnt:
- len>0 means that there is more data in the TCP stream. This helps in directly following the packets with data.
## Notes:
- At first I tried to read the flag through the dump itself. Then realised that the Rar! header means to convert the file to rar.
## Resources:
- https://ctf-wiki.mahaloz.re/misc/archive/rar/
***
# 5. Re:Draw
> Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. Note: There are three stages to this challenge and you will find three flags. What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work.
## Solution: 
- Use volatality imageinfo plugin to see the suggested profiles. In this case, it is Win7SP1x64.
- Use pslist with this profile to see the timing and PID of each program.
- Use consoles plugin to see what was last at the command prompt before system crash.
- We get this string ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0= which base64 decoded gives flag{th1s_1s_th3_1st_st4g3!!}.
- memdump plugin to dump the memory of mspaint.exe to .dmp file. Convert .dmp to .dat file.
- Use GIMP3 to open the file. Change the offset, height and width to get clearer image.
<img width="1920" height="1080" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/59433679-6dfb-48cd-844d-2e016d7e63c9" />
<img width="1920" height="1080" alt="Screenshot (180)" src="https://github.com/user-attachments/assets/938e704f-1722-4473-ae69-a08cf65791c9" />

- 2nd flag is flag{G00d_BoY_good_girl}
- Put the output of filescan plugin in a .txt file. Use grep to find .rar/.zip file in the output.
- Use 7z x to extract archive with the offset of \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
- Also we get to know that the password is NTLM hash (uppercase) of Alissa's password.
- Use hashdump plugin to get the password Alissa Simpson:1003:aad3b435b51404eeaad3b435b51404ee:f4ff64c8baac57d22f22edc681055ba6:::
- Password becomes F4FF64C8BAAC57D22F22EDC681055BA6.
- Use unrar x to extract the file with the password.
- Transfers the flag to flag3.png
<img width="500" height="500" alt="flag3" src="https://github.com/user-attachments/assets/1cf4d809-d10d-495b-80de-aecbab317b13" />

- 3rd flag is flag{w3ll_3rd_stage_was_easy}
## Flag: 
```
flag{th1s_1s_th3_1st_st4g3!!}
flag{G00d_BoY_good_girl}
flag{w3ll_3rd_stage_was_easy}
```
## Concepts learnt:
- Volatility is a powerful tool specifically designed for analyzing and extracting information from computer memory (RAM) images.
- Volatility 2 is Profile-based (--profile=<PROFILE>). Often used when analysts already have known profiles or prefer Windows-specific plugins.
## Resources:
- https://hackers-arise.com/digital-forensics-volatility-memory-analysis-guide-part-2/
- https://infosecwriteups.com/memory-dump-analysis-by-using-volatility-framework-742d70663d41
- https://medium.com/@0x0Aleem/practical-memory-forensics-with-volatility-2-3-windows-and-linux-cheat-sheet-ef5eee325863
