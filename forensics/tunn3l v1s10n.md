# TUNN3L V1S10N
In this challenge i was given an unknown file and i had to find the flag.

Since i was given nothing about what to do i started by trying to determine the type of of the file. I tried using the file command on linux but got nothing useful.
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ file tunn3l_v1s10n
tunn3l_v1s10n: data
```
i opened the file on notepad and saw that it had BM in the initials.
```
BMŽ&,     ºÐ  ºÐ  n  2        X&, %  %          #') *!&1(%5,)3*'8/,/&#3*&-$ ;2.2)%0'#3*&8,(6+'9-+/&##)U=1—
vf‹fR™mVžpXžoTœoT«~cºŒm½ŠiÈ—qÁ“qÁ—tÁ”sÀ“rÀo½Žnºk·j°…d tU£wZ˜oVvR:qR=lO@mRDnSIw^TS93pXRvaYs_T~k^†tc~jYvbPv^LzbP‡m]ƒiYsc›qž„t˜~n›
qscsZJpWGZA1O6&N7'O8(O8(Q:*P9)O8)K5)P:/K5*?)B.#K7,E1&?+ C/$C/$@*H2'K2(G.$@'E,"L4(L4(K3'J2&L2$N4&P5'R7)S6(U8*K0"]B4cI9I/D+M4$M6'J3$F, H."F."D."<&2 02#6'<+">+$B,&^D>fLF63?0-
```
This was a hint that it could be a bmp file but to confirm it i got its meta data using exiftool:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ exiftool tunn3l_v1s10n
ExifTool Version Number         : 12.40
File Name                       : tunn3l_v1s10n
Directory                       : .
File Size                       : 2.8 MiB
File Modification Date/Time     : 2024:12:15 20:00:21+05:30
File Access Date/Time           : 2024:12:16 20:10:49+05:30
File Inode Change Date/Time     : 2024:12:15 20:17:07+05:30
File Permissions                : -rwxrwxrwx
File Type                       : BMP
File Type Extension             : bmp
MIME Type                       : image/bmp
BMP Version                     : Unknown (53434)
Image Width                     : 1134
Image Height                    : 306
Planes                          : 1
Bit Depth                       : 24
Compression                     : None
Image Length                    : 2893400
Pixels Per Meter X              : 5669
Pixels Per Meter Y              : 5669
Num Colors                      : Use BitDepth
Num Important Colors            : All
Red Mask                        : 0x27171a23
Green Mask                      : 0x20291b1e
Blue Mask                       : 0x1e212a1d
Alpha Mask                      : 0x311a1d26
Color Space                     : Unknown (,5%()
Rendering Intent                : Unknown (826103054)
Image Size                      : 1134x306
Megapixels                      : 0.347
```
So now its confirmed that its a bmp file.
I tried changing the file extension but it didnt help and it showed that the file was corrupted.
Well now i was stuck and i was stuck for a long time.

Well i figured that the file had been manipulated and i had to fix it.
i saw the file's hex data to find anaomalies :
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ head -c 54 tunn3l_v1s10n | xxd
00000000: 424d 8e26 2c00 0000 0000 bad0 0000 bad0  BM.&,...........
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000
```
I saw that some hex values have been manipulated and replaced by "bad" whichh corrupts the file.
i had to restore the file so i went to google to find the correct hex values and found them from wikipedia on this website : 
https://en.wikipedia.org/wiki/BMP_file_format

I converted the file to hex by this command to change the values:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ xxd tunn3l_v1s10n > tunn3l_v1s10n.hex
```
i opened tun3l_v1s10n.hex to modify the hex values, initially i changed bad to 000 which was stupid(since i didnt know the correct hex values). I opened wikipedia again to look for the correct hex vales and made the following changes.
I saved this file as fixed.hex
```
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,...........
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000 231a 1727 1e1b 2920 1d2a  ......#..'..) .*
```
Now that the hex values are fixed i connverted the hex file back to bmp:
```
 xxd -r fixed.hex fixed_tunn3l_v1s10n
```
i changes the file extension to .bmp and opened it.
This time the file opened and i got the following image:
![image](https://github.com/user-attachments/assets/9962feac-ec8e-4740-9cdb-544ceb0a164e)

Well guess what i still dont have the flag, i got stuck here again as idk what to do.
I went to wikipedia again to see if i should make any other changes to hex values.
I saw that there are hex values for height and width and well i was satisfied with the width but maybe i can manupulate the height.
The example hex values were a bit different on wikipedia as they didnt have 6e04 and 3201 but i figured that 04 and 01 controlled the width and height.
I changed the hex value of height from 01 to 02 and and image now revealed more:
![image](https://github.com/user-attachments/assets/c59e44ca-4883-469d-bd86-019b3ed48767)

Now i changed the height to 03 and got this:
![image](https://github.com/user-attachments/assets/9098d1dd-5f25-4fb8-a5e8-c0503e4f4c51)

This image did contain the flag and well to fully reveal it i did try to increase the height to 04 but the file became corrupted(maybe cuz the height was too much now).
Anyways i was able to read the flag and got it.
Final hex values:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ head -c 54 fixed_tunn3l_v1s10n5.bmp | xxd
00000000: 424d 8e26 2c00 0000 0000 3600 0000 2800  BM.&,.....6...(.
00000010: 0000 6e04 0000 3203 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000                           ......
```
### Flag:
>picoCTF{qu1t3_a_v13w_2020}
