# 1. m00nwalk
> Decode the audio message (.wav) from moon to get the flag.

## Solution:
- Since the message is transmitted from moon and also the hint 'How did pictures from the moon landing get sent back to Earth?', I searched and found that pictures could be transmitted using analog signals.
- One of which is SSTV (slow-scan television).
- Opened the .wav audio file in an online SSTV decoder and got an image which had the flag in it.

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