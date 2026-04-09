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

- The result was its raw hexadecimal and ASCII data structure
<img width="564" height="346" alt="2 2" src="https://github.com/user-attachments/assets/8efe2346-bd01-4229-b9e3-2dcb8cbdaec8" />

## 3. ```dog.jpg ```
- Binwalk was used to check for hidden files in the dog image binary
- ```binwalk dog.jpg```
  - This detected a hidden zip file which was then extracted from the image

<img width="564" height="164" alt="3" src="https://github.com/user-attachments/assets/0deaccf1-d0f6-4478-84cc-648b6b3ad03e" />

- We go to the extracted folder and check for its files with ```ls``` where a ```hidden.txt``` file was found

 <img width="388" height="91" alt="3 1" src="https://github.com/user-attachments/assets/ec490d78-17b6-4b7b-af01-0b371d8fd169" />

- ```cat``` was then used to view the hidden text file contents
<img width="390" height="51" alt="3 2" src="https://github.com/user-attachments/assets/35154235-fecc-4240-ba39-fe5aeee50f27" />

## 4. ```computer.jpg ```

## 5. ```soilatire.exe ```

## 6. ```rubiks.jpg ```




