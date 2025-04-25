### Sapu Driver
#### Category : `Web`

![Pasted image 20250421164807.png](../images/Pasted%20image%2020250421164807.png)
First i play around with the inspect tool . I found the script.js

> [!NOTE]- script.js
> ```js
> async function fetchDrivers() {
>     try {
>         let response = await fetch('https://sapudriverapi.horizononsight.xyz/api/drivers');
>         let drivers = await response.json();
> 
>         let driverList = document.getElementById('drivers');
>         driverList.innerHTML = '';
> 
>         drivers.forEach(driver => {
>             let card = document.createElement('div');
>             card.className = 'driver-card available';
> 
>             let name = document.createElement('strong');
>             name.textContent = driver.username;
> 
>             let status = document.createElement('div');
>             status.textContent = `Status: ${driver.status}`;
> 
>             let phone = document.createElement('a');
>             phone.href = `tel:${driver.phone_number}`;
>             phone.textContent = driver.phone_number;
> 
>             let phoneWrapper = document.createElement('div');
>             phoneWrapper.innerHTML = '📞: ';
>             phoneWrapper.appendChild(phone);
> 
>             card.appendChild(name);
>             card.appendChild(document.createElement('br'));
>             card.appendChild(status);
>             card.appendChild(document.createElement('br'));
>             card.appendChild(phoneWrapper);
> 
>             driverList.appendChild(card);
>         });
>     } catch (error) {
>         console.error("Error fetching drivers:", error);
>     }
> }
> 
> fetchDrivers();
> setInterval(fetchDrivers, 1000000);
> 
> ```

```js
let response = await fetch('https://sapudriverapi.horizononsight.xyz/api/drivers');
```

This is interesting since it fetch the data in the API . I try to look at the API and only JSON file can be found

```json
[{"username":"handsomeboy kk12","status":"available","phone_number":"+60134567890"},{"username":"Ali","status":"available","phone_number":"+60134567890"},{"username":"testing","status":"unavailable","phone_number":"+60134567890"},{"username":"ctfuser","status":"available","phone_number":"+60134567890"}]
```

Since we dealt with API . I opened my [Postman](https://web.postman.co/) . I try to change the GET to POST and others method but all method is not allow. Then maybe it has other API point?

I try https://sapudriverapi.horizononsight.xyz/api/flag
![Pasted image 20250423001905.png](../images/Pasted%20image%2020250423001905.png)
It got unauthorized access . Okay we are close . How to get access

I try https://sapudriverapi.horizononsight.xyz/api/login and paste this json file

```
{
    "username": "admin",
    "password": "admin"
}
```

It got login successful! Yay. So we try back /admin and we got the flag

Flag : Hunt{3a5y_m4ss_4ss1gnm3nt}

---
### DNS : Do Not Sleep
#### Category : `Forensics`

![Pasted image 20250421165739.png](../images/Pasted%20image%2020250421165739.png)

It gave us a `.pcap File ` so i opened in WireShark

![Pasted image 20250421165940.png](../images/Pasted%20image%2020250421165940.png)
when we view the packet we can see there . Then copy the value and paste in my favorite decoder Cyberchef.io

```
BYcfFwTkOfSHVudHs0RE41XzN4Zmk0bHRyYTR0aTBufQ==1NdaOgdga0
```

since we know == indicate base64 we can try put `1NdaOgdga0` to the front
```
1NdaOgdga0BYcfFwTkOfSHVudHs0RE41XzN4Zmk0bHRyYTR0aTBufQ==
```

Decode text : Ô×Z:.`k@XqñpNC.Hunt{4DN5_3xfi4ltra4ti0n}

And we get the Flag : `Hunt{4DN5_3xfi4ltra4ti0n}`

---
### Simple CrackMe
#### Category : `Reverse Engineering`
![Pasted image 20250421170709.png](../images/Pasted%20image%2020250421170709.png)

Since this is reverse i Opened my disassembler which is Cutter 

in the main function i can see 

> [!NOTE]- main
> ```c
> undefined8 main(void)
> {
>     int32_t iVar1;
>     int64_t iVar2;
>     uint64_t uVar3;
>     uint64_t uVar4;
>     char *s1;
>     int64_t var_1ch;
>     int64_t var_10h;
>     
>     printf("Enter the password: ");
>     fgets(&s1, 100, _stdin);
>     iVar2 = strcspn(&s1, data.0000201d);
>     *(undefined *)((int64_t)&s1 + iVar2) = 0;
>     var_1ch._0_4_ = 0;
>     while( true ) {
>         uVar4 = (uint64_t)(int32_t)var_1ch;
>         uVar3 = strlen(&s1);
>         if (uVar3 <= uVar4) break;
>         *(uint8_t *)((int64_t)&s1 + (int64_t)(int32_t)var_1ch) =
>              *(uint8_t *)((int64_t)&s1 + (int64_t)(int32_t)var_1ch) ^ 5;
>         var_1ch._0_4_ = (int32_t)var_1ch + 1;
>     }
>     iVar1 = strcmp(&s1, "Mpkq~d6130a0`0``761gdZfwdfnh`x");
>     if (iVar1 == 0) {
>         puts("Correct! You cracked it!");
>     } else {
>         puts("Incorrect password.");
>     }
>     return 0;
> }
> ```

so it get your input in fgets() 
and make some cryptography to your input 

as i dont understand the cryptography method lets ask our bestfriend about it Deepseek

We found that it used XOR with 5 as a key to encrypt `Mpkq~d6130a0`0``761gdZfwdfnh`x`

So we put `Mpkq~d6130a0`0``761gdZfwdfnh`x`` as input and XOR with 5 as key in recipe in CyberChef and we got the flag

Flag : Hunt{a3465d55ee234ba_crackme}

---
### Save Ali From Deadline
#### Category : `Forensic`
![Pasted image 20250421171553.png](../images/Pasted%20image%2020250421171553.png)

when we unzip the file and cd to the ctf-repo

i try `ls`
okay nothing

Let's try `ls -la`

```
┌──(kali㉿kali)-[~/UMCyberHunt/Forensic/ctf-repo]
└─$ ls -la
total 12
drwxrwxr-x 3 kali kali 4096 Apr 21 06:00 .
drwxrwxr-x 3 kali kali 4096 Apr 21 06:00 ..
drwxrwxr-x 8 kali kali 4096 Apr  8 07:25 .git
```

we can see there is `.git`

so lets try see all log
```bash
$ git log --all --oneline --graph
* d5e7027 (recover-branch) Add another file
* 39eea18 (HEAD -> master) Add flag
```

Lets view both commit

```bash
$ git show 39eea18       # Check the initial commit
git show d5e7027       # Check the deleted commit
commit 39eea181779720bba6840a078299c5dd1b04ca7e (HEAD -> master)
Author: Eunice Eng <euniceeng04@gmail.com>
Date:   Tue Apr 8 07:23:17 2025 -0400

    Add flag

diff --git a/flag.txt b/flag.txt
new file mode 100644
index 0000000..b9cfd5c
--- /dev/null
+++ b/flag.txt
@@ -0,0 +1 @@
+Hunt{G1t_1s_sup3rrrrr_Aw3s0me}
commit d5e7027af0aaf2ec725ab84e561f37b7374eea7b (recover-branch)
Author: Eunice Eng <euniceeng04@gmail.com>
Date:   Tue Apr 8 07:23:48 2025 -0400

    Add another file

diff --git a/anotherfile.txt b/anotherfile.txt
new file mode 100644
index 0000000..f6e3ee4
--- /dev/null
+++ b/anotherfile.txt
@@ -0,0 +1 @@
+Random content
```

and we got the flag 

Flag : Hunt{G1t_1s_sup3rrrrr_Aw3s0me}

---
### Traffic Light
#### Category : `Cryptography`
![Pasted image 20250422162244.png](../images/Pasted%20image%2020250422162244.png)

the link direct us to youtube short video where there is a number with changing red and green background . My first thought is to exchange the color to binary

0 : green
1 : red

and i get this
```
01001000 01000001 01000011 01001011 01001101 01011001 01001100 01001001 01000110
01000101 00001010
```

then we can just open CyberChef and add From Binary as recipe

and we got : `HACKMYLIFE`

Flag : HUNT{HACKMYLIFE}

---
### Get The Flag
#### Category : `Reverse Engineering`
### ![Pasted image 20250422162918.png](../images/Pasted%20image%2020250422162918.png)

#### File Analysis

```
└─$ ./main 
Reversing Challenge

Enjoy it !!!
Usage:Hunt{flag}
```

I analyze the file with redare2 but can only found 4 function and a lot of code that i dont understand .This was going for quite sometimes . Then my friends found out that the file has UPX

```
└─$ binwalk main       

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             ELF, 64-bit LSB shared object, AMD x86-64, version 1 (SYSV)
4061          0xFDD           Copyright string: "Copyright (C) 1996-2024 the UPX Team. All Rights Reserved. $
```

lets run 
```
┌──(kali㉿kali)-[~/Downloads]
└─$ upx -d main
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.2       Markus Oberhumer, Laszlo Molnar & John Reiser    Jan 3rd 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
     23995 <-      8180   34.09%   linux/amd64   main

Unpacked 1 file.
```

Okay lets analyze with redare again. Alright this time we got a lot more familiar function and readeable code . After that i fire up my Cutter to look to inspect the main function

![Pasted image 20250422164159.png](../images/Pasted%20image%2020250422164159.png)
This function first compare the input whether = "Hunt{"

and then it check whether it has the length of 0x18(24 in decimal) 
then it check the last input has } or not then at 0xd(13) there is _ and at (0x12) there is _ again

So the format is like this : Hunt{xxxxxxxx_xxxx_xxxx}

Alright Lets play around with Cutter more to find more information. In the strings section I found some word that might be the flag

![Pasted image 20250422164636.png](../images/Pasted%20image%2020250422164636.png)
Let try and error and submit the flag to the program. And we got the flag

```
└─$ ./main Hunt{Princess_Mahe_Deva}
Reversing Challenge

Enjoy it !!!
CONGRATULATIONS, you found the flag:  Hunt{Princess_Mahe_Deva}

Elapsed time in clock ticks: 60
All Done!             
```

Flag : Hunt{Princess_Mahe_Deva}

---
### Can you Reverse it ?
#### Category : `Reverse Engineering`
![Pasted image 20250422164917.png](../images/Pasted%20image%2020250422164917.png)

we were provided with text file which is weird for RE challenge so i try to view it and saw a lot of A so i try to remove it and make it executable .Unfortunately that didnt work . I notice there is == in the file so maybe it is base 64. 

I put it in CyberChef and saw `.ELF......` 

```
┌──(kali㉿kali)-[~/Downloads]
└─$ cat suspicious.txt | base64 -d > suspicious
```

yup we got executable file

After play around with redare and Cutter i notice that this function only has 2 function which is main and iterate_int() . I try to understand it for quite sometimes . Since the Title of file is reverse . Then we need to reverse it i guess

Lets just copy the main and iterate_int() in the same file and prompt our AI to reverse it . After few hours trying it . the AI finally can do it an we got the flag

> [!NOTE]- reverse.c
> ```c
> #include <stdio.h>
> #include <stdint.h>
> #include <stdbool.h>
> 
> static int count1 = 0;
> static int count2 = 0;
> 
> // Transformation magic numbers
> const char magic[] = "RdqQTv\x01\x03\x04\x02\x06\x05";
> 
> // Output from forward transform
> uint8_t transformed[] = {
>     0x34, 0x27, 0xcd, 0x10, 0xaf, 0x16, 0x19, 0x36, 0x98, 0x7d, 0x43, 0x17,
>     0x34, 0x33, 0xb3, 0xcc, 0xa6, 0x06, 0x1d, 0x28, 0x19, 0x7d, 0xa3, 0xc0,
>     0x85, 0x3e, 0x2c, 0x0d, 0xa6, 0x12, 0x58, 0x38, 0x0b
> };
> 
> 
> char reverse_byte(int index, uint8_t target_byte) {
>     for (int c = 0; c <= 127; ++c) {
>         if (!((c >= 'a' && c <= 'z') || (c >= '0' && c <= '9') || c == '_' || c == '{' || c == '}')) continue;
> 
>         uint8_t uVar1 = (uint8_t)c;
>         bool bVar4 = (index & 1U) != 0;
> 
>         uint8_t uVar2 = (uVar1 < 0x61 || 0x7a < uVar1) ? 0 : 1;
> 
>         uint8_t result =
>             (-bVar4 & ((uVar1 >> 6 | uVar1 * 4) & -(uVar2 ^ 1)) +
>                       (-uVar2 & (magic[count1] ^ uVar1))) +
>             (-!bVar4 &
>              ((magic[count1] ^ (uVar1 >> 2 | uVar1 << 6)) & -(uVar2 ^ 1)) +
>              (-uVar2 & (uVar1 << (8U - magic[count2 + 6] & 0x1f) |
>                       (uint8_t)((int32_t)(uint32_t)uVar1 >>
>                               (magic[count2 + 6] & 0x1fU)))));
> 
>         if (result == target_byte) {
>             count1 = ((int32_t)((uint32_t)bVar4 + count1)) % 6;
>             count2 = ((int32_t)((uint32_t)bVar4 + count2)) % 6;
>             return (char)c;
>         }
>     }
>     return '?'; // failed to reverse
> }
> 
> 
> int main(void) {
>     for (int i = 0; i < sizeof(transformed); i++) {
>         char c = reverse_byte(i, transformed[i]);
>         printf("%c", c);
>     }
>     printf("\n");
>     return 0;
> }
> ```

Flag : hunt{gdg3_haha_3_w1y5_t0_lai_cai}

---
### BrainF''k
#### Category : `Reverse Engineering`
![Pasted image 20250422234758.png](../images/Pasted%20image%2020250422234758.png)

We got a txt file below 

> [!NOTE]- BF.txt
> ```
> [><]>>+++++++++++[<+++>-]<[-<+>]<---------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<+++++++>-]<[-<+>]<-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<+++++++>-]<[-<+>]<------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++[<+++>-]<[-<+>]<------------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++[<++++++++>-]<[-<+>]<-------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++[<+++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++[<+++++>-]<[-<+>]<----------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<+++++>-]<[-<+>]<---------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++[<+++++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<+++>-]<[-<+>]<--------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++[<+++>-]<[-<+>]<--------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<+++++>-]<[-<+>]<--------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++[<+++++++>-]<[-<+>]<------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++[<+++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++[<+++++++>-]<[-<+>]<------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++[<+++++++>-]<[-<+>]<---------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++[<+++++>-]<[-<+>]<-------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++[<+++++>-]<[-<+>]<---------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<+++++>-]<[-<+>]<-------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<+++++++>-]<[-<+>]<-----------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<+++++++>-]<[-<+>]<--------------------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++++++[<++++++>-]<[-<+>]<----------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++[<+++>-]<[-<+>]<--------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++[<++++>-]<[-<+>]<-----------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++++++++[<++>-]<[-<+>]<------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++++++++[<+++>-]<[-<+>]<--------------------------------------------------------------------------------------------------------------------[><],
> [><]>>+++++++++++++++++++[<+++>-]<[-<+>]<-------------------------------------------------------------------------------------------------------------------------------------------------------------[><],
> [><]>>++++++[<+++++>-]<[-<+>]<-----------------------------------------------------------------------------------------------------------------------------------------------------------[><]
> ```

First lets ask Cloude to analyze this code
Finding :

- `+...+[<+...+>-]<`: This performs multiplication (outer_loop × inner_loop)
- `[-<+>]<`: This moves the value
- `-...-`: This performs a series of decrements
- `[><]`: This is likely just a separator or pointer manipulation

Next Lets ask Cloude to write a parser and process the code for us

> [!NOTE]- analyze()
> ```python
> def analyze_brainfuck_line(line):
>     # Parse the multiplication part
>     parts = line.split("[<")
>     outer_loop = parts[0].count("+")
>     
>     # Get the inner loop multiplier
>     inner_part = parts[1].split(">-]")[0]
>     inner_loop = inner_part.count("+")
>     
>     # Calculate the value created by the multiplication
>     multiplied_value = outer_loop * inner_loop
>     
>     # Count decrements
>     decrements_part = line.split("[-<+>]<")[1].split("[><]")[0]
>     decrements = decrements_part.count("-")
>     
>     # Calculate final value
>     final_value = (multiplied_value - decrements) % 256
>     
>     return {
>         "calculation": f"{outer_loop} × {inner_loop} = {multiplied_value}",
>         "decrements": decrements,
>         "final_value": final_value,
>         "hex": f"0x{final_value:02x}"
>     }
> ```

> [!NOTE]- process()
> ```python
> def process_brainfuck(code):
>     lines = code.strip().split("\n")
>     byte_values = []
>     results = []
>     
>     for i, line in enumerate(lines, 1):
>         result = analyze_brainfuck_line(line)
>         byte_values.append(result["final_value"])
>         results.append((i, result))
>     
>     return byte_values, results
> ```

After running the script we got this value . Lets ask Cloude any relation this value with our flag format which is Hunt{...}

```
b8 8b 92 8c 85 cd a1 98 cc 8c cf a1 d1 8d d2 96 cc 94 99 ca a1 c9 9c cc 9b cd 9e d1 9c 83
```


Cloude find out that subtracting each byte value from 0x100 (256 in decimal), which gave:

```
48 75 6e 74 7b 33 5f 68 34 74 31 5f 2f 73 2e 6a 34 6c 67 36 5f 37 64 34 65 33 62 2f 64 7d
```

Converting these hex values to ASCII revealed the flag:

```
Hunt{3_h4t1_/s.j4lg6_7d4e3b/d}
```

---
