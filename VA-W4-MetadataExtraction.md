# VA-W4-MetadataExtraction
Extracting hidden data from images and files.

## TOOLS
- Exiftool
- Hexeditor
- Binwalk
- Strings
- File

## 1. ```ocean.jpg ```
- Exiftool was used to analyse the metadata of the Ocean image jpeg file
- ```exiftool ocean.jpg```
<img width="548" height="666" alt="1" src="https://github.com/user-attachments/assets/42de4772-2a31-4d70-ba00-0248b11b5550" />

- Final result is a flag in the comment section ```THIS IS THE HIDDEN FLAG```
## 2. ```computer.jpg ```
- Hexeditor was used to analyse the metadata of the computer image jpeg file
  ```hexeditor computer.jpg```
- <img width="266" height="38" alt="2 1" src="https://github.com/user-attachments/assets/4f8e4666-7b0e-4f47-8481-4c584049902a" />

- The result was its raw hexadecimal and ASCII data structure where we can see ICC profile information and various other background strings
<img width="564" height="346" alt="2 2" src="https://github.com/user-attachments/assets/8efe2346-bd01-4229-b9e3-2dcb8cbdaec8" />

## 3. ```dog.jpg ```
- Binwalk was used to check for hidden files in the dog image binary
- ```binwalk dog.jpg```
  - This detected a hidden zip file which was then extracted from the image

<img width="564" height="164" alt="3" src="https://github.com/user-attachments/assets/0deaccf1-d0f6-4478-84cc-648b6b3ad03e" />

- We go to the extracted folder and check for its files with ```ls``` where a ```hidden.txt``` file was found

 <img width="388" height="91" alt="3 1" src="https://github.com/user-attachments/assets/ec490d78-17b6-4b7b-af01-0b371d8fd169" />

- ```cat``` was then used to view the hidden text file contents where the flag was revealed
<img width="390" height="51" alt="3 2" src="https://github.com/user-attachments/assets/35154235-fecc-4240-ba39-fe5aeee50f27" />

## 4. ```computer.jpg ```
- String was used to analyse the computer jpeg image file to look for any hidden readable text inside the file
- ```strings computer.jpg```

<img width="302" height="614" alt="4" src="https://github.com/user-attachments/assets/dad88ffe-ddc9-4cb9-9467-69d14bbdc896" />

- The result was ICC profile information and numerous background strings that are embedded in the file structure

## 5. ```soilatire.exe ```
- File was used to check the true file identity of the solitaire executable file
- ```file solitaire.exe```
  - The result: ```PNG image data, 640 x 449, 8-bit/color RGBA, non-interlaced```
- The file was then renamed to png with ```mv solitaire.exe solitaire.png```
- The file is checked again with ```file solitaire.png``` to verify its identity
- The result is that solitaire was identified as a PNG image with this information: `PNG image data, 640 x 449, 8-bit/color RGBA, non-interlaced` which 
matches the first check with the `file` command

<img width="416" height="128" alt="5" src="https://github.com/user-attachments/assets/5a15bb82-64d8-4682-a1fb-1b41cb1b85f5" />

## 6. ```rubiks.jpg ```
- Just like for solitaire, file was used to check the true file identity of the rubiks jpeg image file
- ```file rubiks.exe```
  - The result: `PNG image data, 609 x 640, 8-bit/color RGBA, non-interlaced`
- The file was then renamed to png with `mv rubiks.jpg solitaire.png`
- The file is checked again with ```file rubiks.png``` to verify its identity
- The result is that rubiks was identified as a PNG image with this information: `PNG image data, 609 x 640, 8-bit/color RGBA, non-interlaced` which 
matches the first check with the `file` command

<img width="395" height="131" alt="6" src="https://github.com/user-attachments/assets/c7bd6dc2-64ae-4de6-a706-2e60b8702fff" />





