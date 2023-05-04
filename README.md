Download Link: https://assignmentchef.com/product/solved-csci4061-assignment-5-image-server
<br>
<h1>   Overview</h1>

In Assignment 4, you were asked to browse through a particular directory tree, locate and create a catalog of a certain subset of the files. However, this catalog was not accessible over a network. In this assignment, you are tasked with writing two programs, a client and server, which communicate with each other via TCP sockets and facilitate sharing of files over a network. Since files downloaded over a network have a high probability of being corrupted, you will also implement error checking using a widely used hashing algorithm.

In this assignment, you are supposed to implement two C programs, a server and a client, that have the following functionalities:

<ol>

 <li>Files (either image or text) can be transmitted between the server and the client through TCP socket.</li>

 <li>A catalog file will be generated at the server and record all the image files that can be downloaded by the client.</li>

 <li>MD5 checksum is used at the client to verify the correctness of transmission.</li>

 <li>An HTML file, named “download.html” is used to show all the downloaded files.</li>

 <li>Deploy the server and client program on two different computers in the computer lab.</li>

</ol>




<strong>Program Invocation:  </strong>

The server program will be invoked as follows:

./server server.config where server.config is like

<table width="157">

 <tbody>

  <tr>

   <td width="87">Port = &lt;port&gt;</td>

   <td width="70"> </td>

  </tr>

  <tr>

   <td colspan="2" width="157">Dir = &lt;server directory&gt;</td>

  </tr>

 </tbody>

</table>

<ol>

 <li>The server program when started, should open a TCP socket on the specified port and wait for incoming client connections.</li>

 <li>The server should browse the directory specified in &lt;server directory&gt; to find image directory and prepare a catalog.</li>

</ol>




The client program will be invoked as follows:

./client client.config  where client.config is like

Server = &lt;IP_address&gt;

<table width="292">

 <tbody>

  <tr>

   <td width="87">Port = &lt;port&gt;</td>

   <td colspan="2" width="204"> </td>

  </tr>

  <tr>

   <td colspan="2" width="181">Chunk_Size = &lt;chunk size&gt;</td>

   <td width="111"> </td>

  </tr>

  <tr>

   <td colspan="3" width="292">ImageType = &lt;type&gt; (only for passive mode)</td>

  </tr>

  <tr>

   <td width="87"></td>

   <td width="94"></td>

   <td width="111"></td>

  </tr>

 </tbody>

</table>

<ol>

 <li>&lt;ip_address&gt;is an IPv4 address in dotted decimal format, which specifies the IP address of the machine on which your server program is running.</li>

 <li>&lt; port &gt; indicates the port on which your server is listening for connections.</li>

 <li>The &lt;chunk size&gt; is the size of chunks that we separate image files into for transmission.</li>

 <li>The &lt;type&gt; is only for <u>passive mode</u>. You need to transmit all the files satisfying the given image type. You give only one image type at a time.</li>

</ol>




<strong>Server design:  </strong>

The server first reads a configuration file (i.e., server.config), and get necessary configuration data. Then it browses through a specified directory (i.e., &lt;server directory&gt;) and searches for image files of type jpg, png, gif, and tiff (in the assignment we only give jpg files as an example, but the program needs to support all the image types). The server then creates a catalog file, named catalog.csv, which contains a record for each image file found (Details on creating this file are specified later in this handout). The server then starts listening for client connections on a specified port (i.e., &lt;port&gt;).

<strong> </strong>

<strong>Client design:  </strong>

The client first reads a configuration file (i.e., client.config) to get necessary configuration information and then runs in 2 modes: Interactive and passive. In both modes, the client must first attempt to connect to a server through IP address and port (i.e., &lt;IP address&gt; and &lt;port&gt;). After the socket connection has been successfully set up, the client must download catalog.csv from the server. The contents of this file must then be displayed to the user on the screen/console.

<u>Passive mode</u>: In passive mode, the file extension will be provided in the configuration file. This will be one of jpg, png, gif, or tiff. The client program must then download all files corresponding to the specified type from the server.

<u>Interactive mode</u>: In the interactive mode, the client must allow the user to choose files for download. You must allow the user to download multiple image files in this mode of execution.

When you are downloading the files, separate image files into small chunks, because the size of image file may be large. Each chunk has &lt;chunk size&gt; bytes. After a file is downloaded, you need to output the number of chunks you have downloaded.




<strong>Interactive mode</strong>:

<ol>

 <li>Edit client.config to be</li>

</ol>

Server = 127.0.0.1 Port = 54432

Chunk_Size = 500




<ol start="2">

 <li>Then run client in the command line</li>

</ol>

$ ./client client.config

===============================

Connecting server at 127.0.0.1, port 54432

Chunk size is 500 bytes. No image type found.

===============================

Dumping contents of catalog.csv

<ul>

 <li>tiff</li>

 <li>png</li>

 <li>gif</li>

 <li>tiff</li>

 <li>png</li>

 <li>gif</li>

</ul>

===============================

Enter ID to download (0 to quit): 1

Downloaded image01.tiff, 172 chunks transmitted.

Enter ID to download (0 to quit): 3

Downloaded image03.gif, 64 chunks transmitted.

Enter ID to download (0 to quit): 0

Computing checksums for downloaded files…

Generating HTML file…

Exiting…

$




<strong>Passive mode</strong>:

<ol>

 <li>Edit client.config to be Server = 0.0.1 Port = 54432</li>

</ol>

Chunk_Size = 500

ImageType = png




<ol start="2">

 <li>Then run client in the command line</li>

</ol>

$ ./client client.config

===============================

Connecting server at 127.0.0.1, port 54432 Chunk size is 500 bytes. Image type is png

===============================

Dumping contents of catalog.csv

<ul>

 <li>tiff</li>

 <li>png</li>

 <li>gif</li>

 <li>tiff</li>

 <li>png</li>

 <li>gif</li>

</ul>

===============================  Running in passive mode… Downloading ‘png’ files…  Downloaded image02.png, 100 chunks transmitted.

Downloaded image05.png, 132 chunks transmitted.

Computing checksums for downloaded files…

Generating HTML file…

Exiting…

$

The text in red indicates user input. The downloaded images must be saved in a subdirectory named images. This subdirectory must be created in the current working directory of the client program. After the files have been downloaded, the checksum must be computed for each downloaded file and compared with the checksum in the downloaded catalog.csv file. An HTML file called download.html must also be created in the current working directory of the client program.

<strong> </strong>

<strong>Record available images through “catalog.csv”: </strong>

This is a CSV formatted file. You need to use a comma(,) as the delimiter. The header line for this file should be  filename, size, checksum

Following the header line, each image file found in the directory must have its own record in the file on a separate line. The filename must be stripped down to the last / (e.g., image01.png instead of /home/grad03/student/hw5/image01.png) . You need to compute the checksum for each image file. You may use the provided API for this. You may refer to catalog.csv in the attached tar ball.  <strong> </strong>

<strong>Verify the correctness of downloading through checksum: </strong>

You have been provided with an API which computes the MD5 hash for a given file. You need to compile md5sum.c and link it with your server and client programs. You may use the provided md5_test.c and Makefile as a reference. (This program is identical to the md5sum command in Linux). Do NOT modify the md5sum.[c/h] files . You will lose significant points if you do so.

<strong> </strong>

<strong>Show the downloaded files through “download.html”: </strong>

This is an HTML document which should list the names of all downloaded files with hyperlinks to the full size images. Additionally, “Checksum match” or “Checksum mismatch” must be displayed next to each filename depending on the result of the checksum comparison. (Note: If, for a particular file there is a checksum mismatch, you do not need to provide a link to the full size image, since the image is probably corrupted). You can find a sample download.html in the attached tar ball.

<strong> </strong>




<strong>Deployment of server/client: </strong>

Although it is possible to run both the server and the client on the same machine, for this assignment you need to ensure that the server and the client both run on different machines. For instance, you can run the server on one of the machines in Lind40 and the client on one of the machines in Keller 4250. (This is how your assignment will be graded, by running your server and client programs on physically different machines, so make sure that it works).







<h1>Assumptions</h1>

<strong>General: </strong>

<ol>

 <li>You are not allowed to use commands in shell scripts, such as “find” &amp; “grep”, use functions in C libraries, such as “opendir()”and “readdir()”.</li>

 <li>You should not change md5sum.[c/h], but you can put them in your own directory.</li>

 <li>Maximum length of file names &lt; 255 bytes; maximum length of dir path &lt; 1024 bytes</li>

 <li>There’s only one client and one server.</li>

 <li>We assume the catalog.csv file will transmit successfully.</li>

</ol>




<strong>Server-side: </strong>

<ol>

 <li>The directory path in server’s configuration file to the server can be absolute or relative.</li>

 <li>The specified directory can contain files other than the image files.</li>

 <li>The specified directory can contain any number of subdirectories. You need to go through all the subdirectories and check for image files while preparing the catalogue.</li>

 <li>The filenames encountered in the directory are guaranteed to be unique.</li>

 <li>If catalog.csv exists, you must truncate/delete it and create a new file.</li>

</ol>

<strong> </strong>

<strong>Client-side: </strong>

<ol>

 <li>The images subdirectory may or may not exist initially. If it exists, it may contain some files. However, these files will not be found on the server (i.e. There will not be any filename conflicts during the download phase)</li>

 <li>You can place all downloaded images in the top-level images subdirectory. There is no need to recreate the directory structure as per what is present on the server.</li>

 <li>If download.html exists, you must truncate/delete the file and create a new file.</li>

 <li>You may safely assume that the server program is always started before the client program starts.</li>

</ol>




<h1>Trouble shooting</h1>

<ol>

 <li>Error in compiling “fatal error: openssl/md5.h: No such file or directory”

  <ul>

   <li>First check whether you have installed libssl-dev, and openssl</li>

   <li>Then check your make file, remember to (1) compile server.c with linkers: -lcrypto – lssr, and (2) use linker -pthread for threads</li>

  </ul></li>

 <li>Sometimes you need to convert signed char*/unsigned char* to char*, you can try “sprintf”.</li>

 <li>When you are reading/writing from/to sockets, try to use unbuffered I/O, such as read(), write(), send(), recv(), instead of buffered I/O, such as fprintf() and fread().</li>

</ol>




<h1>What you need to turn in</h1>

<ol>

 <li>The source files of your programs ( *.[c/h] files)</li>

 <li>The configuration files ([server/client].config)</li>

 <li>A README.txt which contains the name and student ID of the team members. This file must also contain instructions about the way your programs must be run.</li>

 <li>A Makefile for compiling the two programs. The two executables must be named server</li>

</ol>

and client respectively.

<ol start="5">

 <li>Please do not include download.html, catalog.csv, images/and other image files in
 
your submissions.</li>

 <li>Submitting your assignment: All the above files must be in a single directory and the directory name must be the student IDs of the team members. This directory must be compressed and named studentid1_studentid2.tar.gz. The directory structure should look like this: 
studentid1_studentid2/Makefile
 
studentid1_studentid2/README.txt  studentid1_studentid2/*.[c/h]
 
studentid1_studentid2/[server/client].config</li>

 <li>Failure to follow these instructions will result in a significant reduction in points.</li>

</ol>




<h1>Resources and Hints</h1>

<ol>

 <li>Socket programming under UNIX.</li>

 <li>Test your code with plaintext files initially, and then switchover to image files.</li>

 <li>Since you need to transfer multiple files over the sockets, figure out a way to specify the end of a particular file/message and the start of a new file/message: This can be a little tricky but not very hard.</li>

 <li>Your code will be graded on the CSELabs machines. Please make sure that it runs on these machines. Failure to do so will result in a significant reduction in points.</li>

 <li>Feel free to post any additional questions you might have on the moodle project discussion forum.</li>

</ol>




<strong>Sample server &amp; client: </strong>

You have been provided with 2 executables, server and client which implement a TCP server and a TCP client respectively. You can first write a server and test it against the provided client or first write a client and test it against the provided server. You can then add the other modules as required for the assignment.

The messages printed on the screen are self-explanatory, and should give you a clear idea of

what is going on behind-the-scenes. Please be aware of this when testing your client/server with the provided server/client.




To start the server on a port (say 56789) on a machine with IP address 212.35.65.123, execute:

$./server 56789

To start the client and connect to the server, execute:

$./client 212.35.65.123 56789




Note that the sample server &amp; client is just for TCP connection test. They are not required in the assignment.

Image copyrights:

<ol>

 <li>“Angla tuulikud Saaremaal” by Abrget47j. Licensed under CC BY-SA 3.0 ee via Wikimedia Commons http://commons.wikimedia.org/wiki/File:Angla_tuulikud_Saaremaal.jpg#/media/File:Angl a_tuulikud_Saaremaal.jpg</li>

 <li>“Frecce Tricolori mijanka 2” by Łukasz Golowanow, Konflikty.pl – konflikty.pl. Via Wikimedia Commons –</li>

</ol>

http://commons.wikimedia.org/wiki/File:Frecce_Tricolori_mijanka_2.jpg#/media/File:Frec ce_Tricolori_mijanka_2.jpg

<ol start="3">

 <li>“Hommik Mukri rabas” by Amadvr – Own work. Licensed under CC BY-SA 3.0 ee via Wikimedia Commons –</li>

</ol>

http://commons.wikimedia.org/wiki/File:Hommik_Mukri_rabas.jpg#/media/File:Hommik_ Mukri_rabas.jpg


