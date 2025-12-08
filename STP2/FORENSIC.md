## GOTHAM HUSTLE
The challenge gave a file called gotham.7zip, unziped it and got a gotham.raw file, didnt know what was a raw file so started searching for it , then used the file command but didnt find anything,
then used the exiftool but got an error that few 40 bits are zeros so i thought the file is corrupt, put it in hexedit, didnt find anything , again searched for raw file and their signature,raw files are 
image files it said , then i searched how to analys raw file and it said to binwalk it so i did , binwalk gave a ton of things exe, images, elf , dll and dat files, i copied a chunk of the files and 
pasted it in the chatgpt and i asked what file is it , it was a dump file , a ram memory dump file ,i tried strings and it said something like windows 0.1 stuff and i searched memory dump file had 
a windows version so i was sure it was a memory dump file, now i had to analyze it so i asked how to analyze these type of files it gave me a option to binwalk and extract the whole file which was
stupid or try use some tools ,  i tried some tools for idk how many hours till I found a tool called Volatility i searched memeory dump file on youtube as well and it gave be few Volatility video where one 
of the channels was dfir so i gave it a try, i watched few video, link of the video down below , i tried installing Volatility 3 on my fedora which i did searched some command and the syntax of 
Volatility 3 and tried it, i installed it wrong , there was some fault so i then went to windows and installed Volatility workbench and it gave me an output just like the videos so i was on 
the right track hopefully, but i couldn't use some plugins like cmdscan or console and there were errors when i used the dumpfile, so copy pasted the error in chatgpt and it said that is
because the version of windows was below 8 and Volatility 3 works with windows version 8 and 10, so i had to download Volatility 2 which was a hastle,
fedora doesnt support python 2 and Volatility 2 needs python 2 and it took me hours to figure it out and there are no videos of how to install Volatility 2 properly, then after some time chatgpt
gave me a standalone Volatility 2 file so i used that and it worked, i used imageinfo which gave me some info 
```
.\volatility_2.6_win64_standalone -f gotham.raw imageinfo
```

<img width="1106" height="408" alt="Screenshot 2025-12-08 165131" src="https://github.com/user-attachments/assets/879804c8-d791-42c2-8d60-b9f484efc674" />

We will use the latest profile  Win7SP1x64, the next i did was pslist which gives the list of processes, so many chrome.exe and paint and explorer and notepad and all the other i didnt understand

```
 .\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 pslist
```
<img width="910" height="805" alt="Screenshot 2025-12-08 165656" src="https://github.com/user-attachments/assets/d50ee290-48a3-4a57-b4f7-1894c4d45d65" />


then i used filescan but there was too many files to look at so i did Select-String because i am in windows powershell (learnt if from the video ) and not linux where i would do grep I AM TIRED.
```
.\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 filescan | Select-String flag
```

<img width="1307" height="102" alt="Screenshot 2025-12-08 170433" src="https://github.com/user-attachments/assets/e2fba46d-ce68-410a-8896-0beadde50ed2" />

OHH found the flag but it is flag 5 so i extracted it using the dumpfile 
```
 .\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 dumpfiles -Q 0x000000011fdaff20 -D dump
```
i am dumping the file in dump dir and i renamed the file to flag5.rar and it is password protected ..........  no worries i will use hashdump,(how do i know it the video mentioned it plus few search
on the net.

<img width="1091" height="117" alt="Screenshot 2025-12-08 171001" src="https://github.com/user-attachments/assets/0616390b-ec29-4cbf-9d2c-a2aeffa2ab9a" />

```
bruce:1001:aad3b435b51404eeaad3b435b51404ee:b7265f8cc4f00b58f413076ead262720:::
```
chatgpt explained the parts of the line the first part is RID and the second part is LM hash and the third part is NTLM hash which is important and related to the normal uses 
```
b7265f8cc4f00b58f413076ead262720
```
and i used crackstation.net and i got the password **batman**


<img width="1337" height="452" alt="Screenshot 2025-12-08 171522" src="https://github.com/user-attachments/assets/c4d380b1-cb13-4ad0-929e-c558b386900d" />

```
 the text file contained bTByM18xMzMzNzQzMX0=
```
it is bade64 so decoded it and got **m0r3_13337431}**

then i used other command like cmdscan 
<img width="1057" height="455" alt="Screenshot 2025-12-08 172112" src="https://github.com/user-attachments/assets/d70a013f-f6ee-4009-8ed6-daec41ba2432" />

``` Ymkwc2N0Znt3M2xjMG0zXw== which converts to (base64): bi0sctf{w3lc0m3_ ```
so i have the first part and the last part of the flag 
then i used hivescan hivelist and many more plugins 
and i used handles and then i used pslist for the processs and i got notepad and paint and too many chrome so i used memdump for more info, i used memdump on chrome pid=4456 , notepad  pid = 2592
mspaint pid = 2516 , maybe they wrote something there 
```
.\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 memdump -p 2516 --dump-dir="dump"
.\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 memdump -p 4456 --dump-dir="dump"
.\volatility_2.6_win64_standalone.exe -f gotham.raw --profile Win7SP1x64 memdump -p 2592 --dump-dir="dump"
```
then went to the dump dir and did a string search for flag 
```
strings.exe 2592.dmp | Select-String flag
```
and i got 
```
https://www.google.com/search?q=flag3+%3D+aDBwM190aDE1Xw%3D%3D&oq=flag3+%3D+aDBwM190aDE1Xw%3D%3D&aqs=chrome..69i57j0i512i546l2.321545j0j7&sourceid=chrome&ie=UTF-8
```
<img width="965" height="752" alt="Screenshot 2025-12-08 174942" src="https://github.com/user-attachments/assets/5f7b5acc-5411-4e67-816d-e5304517567c" />

**flag3 = aDBwM190aDE1Xw==** which is **h0p3_th15_**
so i got part 1 3 and 5 i have to find part 2 and 4, i have tried netscan malfind and stuff, will keep trying.


## resources 
https://www.youtube.com/watch?v=Uk3DEgY5Ue8 , https://youtu.be/EqGoGwVCVwM?si=WrJy5AoL8IOlgCBu ,https://www.youtube.com/watch?v=VK3fvNFGAzE
https://blog.onfvp.com/post/volatility-cheatsheet/ , https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1 




