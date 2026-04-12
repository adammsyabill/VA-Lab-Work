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


