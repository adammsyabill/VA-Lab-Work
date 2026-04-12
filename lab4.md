LAB 4

1.0 exiftool

    exiftool ocean.jpg

We use exiftool to extract metadata from the image file. Metadata is hidden information stored inside the image such as file details, creation data and additional embedded information that cannot be seen normally

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/0e7ac7852a9f320dbcde9dca074f080e9a91fe2d/image/Screenshot%202026-04-13%20040208.png)

and then we found in the comment: THIS IS HIDDEN FLAG

This shows that images can contain hidden data within metadata fields such as comments.

2.0 hexeditor

    hexeditor computer.jpg

We use hexeditor to view the raw binary content of the file. It allows us to analyze both hexadecimal values and ASCII-readable text inside the image, which may reveal hidden or embedded information.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/c233523959e4046d93f5518b2ce823c8f7557420/image/Screenshot%202026-04-13%20040250.png)

From the ouput, we can see that the hexeditor displays two sections: • Left side shows hexadecimal values • Right side shows ASCII-readable text

and then we press w in the hexeditor to find possible strings such as "flag" (in this lab) but we found nothing. so the computer.jpg does not contains anything unusual.

3.0 binwalk

    binwalk dog.jpg

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/028d10bfe47f7197c17b50b41f8b99e621bb318b/image/Screenshot%202026-04-13%20040709.png)

We can see zip file is embedded in this file. So now we can extract it using

    binwalk -e dog.jpg

and then we will get _dog.jpg.extracted folder. so we can see what is in the folder.

    cd _dog.jpg.extracted
    ls
    
So, we can see it contains hidden_text.txt now open the hidden_text.txt to see what is inside

    open hidden_text.txt

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/6a50a3eb80d55e66422088e88e8b27f6c416dd69/image/Screenshot%202026-04-13%20040643.png)

and then we found the THIS IS A HIDDEN FLAG

4.0 strings

    strings computer.jpg

We use the strings command to extract human-readable text from a binary file. This helps identify any embedded text or information without analyzing raw hexadecimal data

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/b5c205bce7aa21f99732fbc4224ed9b3bdb72aa1/image/Screenshot%202026-04-13%20040903.png)

The command displays multiple readable strings such as: • JFIF • ICC_PROFILE • RGB XYZ • lcms

Some random or unreadable characters are also shown. No hidden flag or meaningful secret message was found

5.0 file

    file solitaire.exe

We use the file command to determine the actual type of a file based on its content, not just its file extension.

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/36d5eef0e8abbe5e67b50cdeb34f9bcca5284696/image/Screenshot%202026-04-13%20041318.png)

and we can see that the actual file extension of solitaire.exe is actually .png so we can change the file's extension

    mv solitaire.exe solitaire.png

    open solitaire.png

now we can open solitaire image

![image alt](https://github.com/adammsyabill/VA-Lab-Work/blob/d881be227cd5bf0e20058eba179c1aeab97f0105/image/Screenshot%202026-04-13%20041128.png)

it also same to rubiks.jpg


