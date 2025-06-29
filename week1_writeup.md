# TryHackMe - Advent of Cyber 2023: Day 13, 14 & 25 Write-up

## Task 13 – Proxy Log Analysis

### Commands Used

- Find the number of unique IP addresses connected to the proxy server:
  ```bash
  cut -d ' ' -f2 access.log | sort | uniq | wc -l
  ```

- Find the number of unique domains accessed by all workstations:
  ```bash
  cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq | wc -l
  ```

- Find the least accessed domain:
  ```bash
  cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort -n | head -n 1
  ```

- Find status code of the least accessed domain:
  ```bash
  grep partnerservices.getmicrosoftkey.com access.log | head -n 5
  ```

- Find suspicious domain with high connection attempts:
  ```bash
  cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort -n | tail -n 15
  ```

- Find source IP that accessed the malicious domain:
  ```bash
  grep frostlings.bigbadstash.thm access.log | head -n 5
  ```

- Extract exfiltrated base64 data and find the flag:
  ```bash
  grep frostlings access.log | cut -d ' ' -f5 | cut -d '=' -f2 | base64 -d | grep 'THM{'
  ```

---

## Task 14 – USB Image & Windows Forensics

### Steps Taken

- Exported the file named `DO_NOT_OPEN`, opened `secretchat.txt`, and found the name of the C2 malware server.
- Exported the ZIP file and found the deleted file inside it.
- Opened `portrait.png` in hex view and searched for `THM{` to find the flag.
- Used the "Verify Drive/Image" feature to retrieve the SHA1 checksum for the USB drive.

---

## Task 25 – Memory Forensics with Volatility

### Commands Used

- View bash history:
  ```bash
  vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_bash
  ```

- Find PID of the miner process:
  ```bash
  vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" linux_pslist
  ```

- Extract and get MD5 hash of the miner process:
  ```bash
  md5sum extracted/miner.10280.0x400000
  ```

- Extract and get MD5 hash of the mysqlserver process (same method as above).

- Find malicious URL inside the miner binary and defang it:
  ```bash
  strings extracted/miner.10280.0x400000 | grep http://
  ```

- Find location of the mysqlserver process using elfie:
  ```bash
  cat extracted/elfie
  ```

---

# picoCTF

## Challenge 1 - Information

- Downloaded `cat.jpg`  
- Ran:
  ```bash
  strings cat.jpg | less
  ```
- Found base64-looking strings  
- Decoded using:
  ```bash
  echo 'base64_encoded_string' | base64 -d
  ```
- Flag: `picoCTF{the_m3tadata_1s_modified}`

---

## Challenge 2 - Matryoshka Dolls

- Installed binwalk:
  ```bash
  sudo apt install binwalk
  ```
- Scanned image:
  ```bash
  binwalk dolls.jpg
  ```
- Found embedded files  
- Extracted using:
  ```bash
  binwalk -e dolls.jpg
  ```
- Repeated the process for each extracted image  
- Flag found after several layers

---

## Challenge 3 - tunn3l_v1s10n

- File wouldn’t open—suspected broken header  
- Identified file as BMP using hex signature (`BM`)  
- Compared with a sample BMP file  
- Repaired header manually  
- Flag recovered after fixing BMP structure

---

## Challenge 4 - MacroHard WeakEdge

- Ran:
  ```bash
  binwalk "Forensics is fun.pptm"
  ```
- Found embedded zip files  
- Extracted using:
  ```bash
  binwalk -e "Forensics is fun.pptm"
  ```
- Found `vbaProject.bin` and `hidden`  
- Inspected `hidden`, found base64-encoded data  
- Decoded it and found:  
  Flag inside the decoded content

---

## Challenge 5 - Enhance!

- Given file: `.svg`  
- Installed Inkscape:
  ```bash
  sudo apt install inkscape
  inkscape file.svg
  ```
- Tweaked object layers in Inkscape  
- Found white-colored, tiny flag text  
- Flag revealed visually

---

## Challenge 10 - Extensions

- Checked file type:
  ```bash
  file flag.txt
  ```
- It was a PNG image  
- Opened with:
  ```bash
  display flag.txt
  ```
- Flag was visible in the image


