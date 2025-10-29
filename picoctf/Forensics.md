# 1. m00nwalk
> Decode the audio message (.wav) from moon to get the flag.

## Solution:
- Since the message is transmitted from moon and also the hint 'How did pictures from the moon landing get sent back to Earth?', I searched and found that pictures could be transmitted using analog signals.
- One of which is SSTV (slow-scan television).
- Opened the .wav audio file in an online SSTV decoder and got an image which had the flag in it.
<img width="320" height="256" alt="decoded-image" src="https://github.com/user-attachments/assets/97214c9f-e59e-4b2f-8ca6-54d2d79de450" />

## Flag:
```
picoCTF{beep_boop_im_in_space}
```
## Concepts Learnt:
- SSTV: Classic method to hear from amateur radio operators or transmissions from the International Space Station (ISS).
- Often used to hide images in audio files.
- Besides online decoder, QQSTV tool can be downloaded to decode SSTV signal.

## Notes:
- At first, I tried to go with Morse code and Spectrogram. But when, the file didn't seem to be coded like that, I used the hints to reach SSTV encoder.
## Resources:
- https://sstv-decoder.mathieurenaud.fr/
- https://en.wikipedia.org/wiki/Apollo_11_missing_tapes
***
# 2. tunn3l v1s10n
>   Recover the flag from the file.
## Solution:
- The has no extension, running it through online hex editor tool gave 1st line BM which indicates .bmp file.
- Changing the extension still does not open the file and trying to convert to jpg shows that the file is corrupted.
- Usual guess is corruption in header. Then I noticed BA D0 values where there should be integers.
<img width="1910" height="1028" alt="Screenshot 2025-10-28 022157" src="https://github.com/user-attachments/assets/5f6329ca-8a24-42d2-98ac-207c8275237a" />
- So I searched the correct values for a non-corrupted BMP file and changed them to 36 00 and 28 00.
- This gave a bmp file that seemed like an incomplete image and had a fake flag.
<img width="1405" height="714" alt="Screenshot 2025-10-28 022903" src="https://github.com/user-attachments/assets/083aa49f-9fb0-49b0-a160-a7c654454d6a" />
- Now finding current size of the image- 1134x306 which converted to hex is 0x46E and 0x132 respectively.
- We have to change the height of the image, which for a guess I took 840- 0x348 in hex.
- I changed 32 01 to 48 03
<img width="1914" height="1025" alt="Screenshot 2025-10-28 023514" src="https://github.com/user-attachments/assets/e06a634e-c57c-453f-b9eb-39c47c3ae53f" />
- This gave me the clear picture with correct flag.
<img width="1416" height="724" alt="Screenshot 2025-10-28 023242" src="https://github.com/user-attachments/assets/206c7552-9731-44ea-9cf7-cd209853489e" />

## Flag:
```
picoCTF{qu1t3_a_v13w_2020}
```

## Concepts Learnt:
- Headers and offset values of .bmp and other file extensions.
- 0x46E is represented as 6E 04. This is little-endian format.
- BMP files use little-endian format: The first byte is the low-order (smallest) part and second byte is the high-order (larger) part.
- BMP does not automatically add missing pixel data if I change height, I'm just changing the header.
- If the new header exceeds the actual file size, the BMP viewer sees an error.
  
## Notes:
- I tried to change height to 900 and the file still won't open because it exceeded the actual file size. So, I decreased it and made a guess of 840 pixels which was within limits.
  
## Resources:
- https://hex-works.com/
- https://www.geeksforgeeks.org/dsa/little-and-big-endian-mystery/
***
# 3. Trivial Flag Transfer Protocol
> Inspect the .pcapng file and figure out how they moved the flag.
## Solution:
-  Open the file in wireshark.
-  Filter out tftp type files to see instructions.txt, plan, program.deb, picture1.bmp, picture2.bmp, picture3.bmp
  
<img width="1010" height="605" alt="Screenshot 2025-10-29 235856" src="https://github.com/user-attachments/assets/fd40a988-5cc1-4451-83e5-91714f985392" />

-  Export all the files and cat the instructions file.
-  That gave- GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA. Decoding using ROT13, we get- TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN.
-  Upon spacing- TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN
-  This means we have to open plan file using cat.
-  That gave- VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF. Decoding this, we get- IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS.
-  Upon spacing- I USED THE PROGRAM AND HID IT WITH- DUEDILIGENCE. CHECK OUT THE PHOTOS
-  For steghide, we need a passphrase, which according the above hint could be DUEDILIGENCE.
-  Using steghide, extract the stego file (-sf) for hidden data with each picture, we get this message in picture3.bmp -  "wrote extracted data to "flag.txt".".
-  Opening flag.txt using cat gives the flag.
  
```
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ wireshark tftp.pcapng
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads$ cd Old_Download
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ cat instructions.txt
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ cat plan
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ steghide extract -sf picture1.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ steghide extract -sf picture2.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ steghide extract -sf picture3.bmp
Enter passphrase:
wrote extracted data to "flag.txt".
vaish@DESKTOP-59LUGSG:/mnt/c/Users/Vaishnavi/Downloads/Old_Download$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Flag:
```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts Learnt:
-  Wireshark can monitor what is sent or received via the internet on your system and monitor saved network traffic like PCAP files.
-  Steghide is steganography program which hides bits of a data file in some of the least significant bits of another file in such a way that the existence of the data file is not visible.
  
## Notes:
-  Since the type of cipher is not mentioned, I first started decoding with caeser cipher and rot 47 but got no valuable output.
-  At first while spacing the text from plan file, I spaced DUEDILIGENCE as well.
-  But then the passphrase and hint made it clear that it is supposed to be a password and hence have no spacing.
  
## Reources:
- https://www.kali.org/tools/steghide/
- https://medium.com/@viewshola/analyzing-pcap-files-using-wireshark-73fc1bef3c05
- https://www.dcode.fr/rot-13-cipher
