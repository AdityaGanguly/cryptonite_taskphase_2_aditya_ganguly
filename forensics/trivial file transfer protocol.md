# TRIVIAL FILE TRANSFER PROTOCOL
In this challenge we were given with a pcapng file and had to figure out how to get the flag from it.
I found out an application called wireshark which can help me open this file so i downloaded it and opened the file on it :
![image](https://github.com/user-attachments/assets/e0fb9a40-7281-4403-bced-4d380c66ecbd)

We can see instructions.txt so i tried to download it by exporting a TFTP type object :
![image](https://github.com/user-attachments/assets/143b015a-56f8-4428-9299-85771ec37c18)

There are a bunch of files here so i downloaded them and started with instrustions.txt which had this text : 
```
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
```
This was a cipher text so i tried to figure out which one it was and eventually got ROT13 so i used it and got this output:
```
TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER.FIGURE OUT AWAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN
```
This hint tells us to open the plan file so i did that and found this text in it:
```
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
```
I tried ROT13 on this too and got this:
```
I USED THE PROGRAM AND HID IT WITH-DUE DILIGENCE.CHECK OUT THE PHOTOS
```
Now we had to work with the 3 photos given and also a file called program.deb.
I used this command to unpack the .deb file into the extracted_program directory:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ dpkg-deb -xv program.deb extracted_program
./
./usr/
./usr/share/
./usr/share/doc/
./usr/share/doc/steghide/
./usr/share/doc/steghide/ABOUT-NLS.gz
./usr/share/doc/steghide/LEAME.gz
./usr/share/doc/steghide/README.gz
./usr/share/doc/steghide/changelog.Debian.gz
./usr/share/doc/steghide/changelog.Debian.amd64.gz
./usr/share/doc/steghide/changelog.gz
./usr/share/doc/steghide/copyright
./usr/share/doc/steghide/TODO
./usr/share/doc/steghide/HISTORY
./usr/share/doc/steghide/CREDITS
./usr/share/doc/steghide/BUGS
./usr/share/man/
./usr/share/man/man1/
./usr/share/man/man1/steghide.1.gz
./usr/share/locale/
./usr/share/locale/ro/
./usr/share/locale/ro/LC_MESSAGES/
./usr/share/locale/ro/LC_MESSAGES/steghide.mo
./usr/share/locale/fr/
./usr/share/locale/fr/LC_MESSAGES/
./usr/share/locale/fr/LC_MESSAGES/steghide.mo
./usr/share/locale/de/
./usr/share/locale/de/LC_MESSAGES/
./usr/share/locale/de/LC_MESSAGES/steghide.mo
./usr/share/locale/es/
./usr/share/locale/es/LC_MESSAGES/
./usr/share/locale/es/LC_MESSAGES/steghide.mo
./usr/bin/
./usr/bin/steghide
```
In this list of files we can see 2 that caught my intrest :  README and TODO.
So i tried opening them but README was filled with giberish but TODO had some stuff in it:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads/extracted_program/usr/share/doc/steghide$ cat TODO
These are things that I plan to implement in the future:

* partition code into library and frontend

* graphical user interface

* user-friendly installer for Windows version (InnoSetup)

* use libaudiofile for audio file format support

* make embedding data in audio cds possible (embed markers for synchronization)

* rewrite memory management such that cover-/stego-file must no longer be kept in memory as a whole

* support for other file formats (mp3, png, gif, avi)

* user's guide (sgml?, docbook?, gnu texinfo?)

* support for RLE-encoded bmps

* matrix encoding

? support for spreading one secret file into a set of >= 1 cover files

? support for embedding more than one message into one cover file (different passphrases)

? allow PGP encryption of embedded data (gpgme?)
```
The hints in this point towards steganography so i i used steghide on the photos.
The problem is steghide requires a passphrase which i dont have so i was stuck here until i went to the plan file again and saw a hyphen(-) before DUE DILIGENCE. This could be the passphrase. 

Unfortunately all 3 of the photos didnt give me anything but i tried again without the space and the 3rd pic returned an output:
```
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ steghide extract -sf picture1.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
aditya_cryptonite@DESKTOP-SG1LV2M:/mnt/c/Users/Admin/Downloads$ steghide extract -sf picture2.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
aditya_cryptonite@DESKTOP-SG1LV2M:~$ steghide extract -sf /mnt/c/Users/Admin/Downloads/picture3.bmp
Enter passphrase:
wrote extracted data to "flag.txt".
```
I used the cat command and got the flag:
```
aditya_cryptonite@DESKTOP-SG1LV2M:~$ ls
aditya_cryptonite      cryptonite_taskphase_aditya  flag.txt  home.pub  key.pub  test.xml
aditya_cryptonite.pub  dont_read                    home      key       qsstv    waterfall-20241218193043.jpg
aditya_cryptonite@DESKTOP-SG1LV2M:~$ cat flag.txt
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```
### Flag:
>picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
