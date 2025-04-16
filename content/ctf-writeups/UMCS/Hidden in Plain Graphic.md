Challenge : Hidden in Plain Graphic
Category : Forensic 
Scenario : Agent Ali, who are secretly a spy from Malaysia has been communicate with others spy from all around the world using secret technique . Intelligence agencies have been monitoring his activities, but so far, no clear evidence of his communications has surfaced. Can you find any suspicious traffic in this file?

## Solution
1. Open the PCAP file using WireShark 
2. By filtering the packet length . I can see there is only one packet that are so large compare to others
![[../images/Pasted image 20250308111803.png]]
3. Lets try follow the stream by clicking Follow>TCP Stream
![[../images/Pasted image 20250308111910.png]]
4. I see there is a PNG inside . Maybe we should extract it 
5. By Clicking Show as and choose RAW . We can save this file as .PNG
6. ![[../images/Pasted image 20250308111543.png]]
7. Lets try to run zsteg to see if there any hidden message there
> [!NOTE]- zsteg example
> ```
> ┌──(kali㉿kali)-[~/Desktop]
> └─$ zsteg cyber-security_stego.png 
> b1,b,lsb,xy         .. file: OpenPGP Public Key
> b1,rgb,lsb,xy       .. text: "23:CTF{h1dd3n_1n_png_st3g}"
> b1,abgr,lsb,xy      .. text: "ega7 6d71"
> b1,abgr,msb,xy      .. file: Linux/i386 core file
> b2,r,lsb,xy         .. file: Linux/i386 core file
> b2,r,msb,xy         .. file: Linux/i386 core file
> b2,g,lsb,xy         .. file: locale data table
> b2,g,msb,xy         .. file: Linux/i386 core file
> b2,b,lsb,xy         .. file: Linux/i386 core file
> b2,b,msb,xy         .. file: Linux/i386 core file
> b2,rgba,msb,xy      .. file: GeoSwath RDF "\010", version , creation time 0x28020220, ping header size 2090, frequency 0, echo type 0, pps mode 0, 1st ping number 0                                                                                      
> b2,abgr,lsb,xy      .. file: 0420 Alliant virtual executable not stripped
> b4,g,lsb,xy         .. file: raw G3 (Group 3) FAX, byte-padded
> b4,b,lsb,xy         .. file: 0420 Alliant virtual executable not stripped
> b4,rgba,msb,xy      .. file: Applesoft BASIC program data, first line number 8
> b4,abgr,msb,xy      .. file: Atari DEGAS Elite compressed bitmap 320 x 200 x 16, color palette 0080 0080 8008 8000 0080 ...
> ```
> 

And we got the flag
flag : umcs{h1dd3n_1n_png_st3g}