# Rollei AC 420
Reverse Engineering of the Rollei AC 420 WiFi connection

## Connection
Activate WiFi on camera and connect with an computer. Use the shown login details. You will recieve an IP address via DHCP from camera:
```
192.168.1.10
```

## Play Livestream
Open this with VLC (i.e.)
```
rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000
rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/track1
```

If you're using VLC and you are wondering about a delay with ~1 second delay:
Have a look in streaming configuration at ```Show more options``` and set the caching to 0 i.e.

### Network sniffer output
192.168.1.1:554 TCP
```
DESCRIBE rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000 RTSP/1.0
CSeq: 2
User-Agent: ICatchMedia (LIVE555 Streaming Media v2013.06.06)
Accept: application/sdp

RTSP/1.0 200 OK
CSeq: 2
Date: 2012/1/1
Content-Base: rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/
Content-Type: application/sdp
Content-Length: 330

v=0
o=- 1363853598 1 IN IP4 192.168.1.1
s=Motion JPEG. Streamed by iCatchTek.
i=MJPG
t=0 0
a=tool:iCatchTek
a=type:broadcast
a=control:*
a=range:npt=0-
a=x-qt-text-nam:Motion JPEG. Streamed by iCatchTek.
a=x-qt-text-inf:MJPG
m=video 0 RTP/AVP 26
c=IN IP4 0.0.0.0
b=AS:12288
a=frmerate:167.1016082
a=control:track1
SETUP rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/track1 RTSP/1.0
CSeq: 3
User-Agent: ICatchMedia (LIVE555 Streaming Media v2013.06.06)
Transport: RTP/AVP;unicast;client_port=39990-39991

RTSP/1.0 200 OK
CSeq: 3
Date: 2012/1/1
Transport: RTP/AVP;unicast;destination=192.168.1.10;source=192.168.1.1;client_port=39990-39991;server_port=7004-7005
Session: A5CD1204

PLAY rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/ RTSP/1.0
CSeq: 4
User-Agent: ICatchMedia (LIVE555 Streaming Media v2013.06.06)
Session: A5CD1204
Range: npt=0.000-

RTSP/1.0 200 OK
CSeq: 4
Date: 2012/1/1
Range: npt=0.000-
Session: A5CD1204
RTP-Info: url=rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/track1;seq=46383;rtptime=2000000000

TEARDOWN rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/ RTSP/1.0
CSeq: 5
User-Agent: ICatchMedia (LIVE555 Streaming Media v2013.06.06)
Session: A5CD1204

RTSP/1.0 200 OK
CSeq: 5
Date: 2012/1/1
```

## Play a saved File
```
rtsp://192.168.1.1/VIDEO/20171006_125541.MOV
rtsp://192.168.1.1/VIDEO/20171006_125541.MOV/track1
rtsp://192.168.1.1/VIDEO/20171006_125541.MOV/track2
```

## 192.168.1.1:21 (FTP login)

Login used by camera:
```
USERNAME: wificam
PASSWORD: wificam
```
But you can use any username and password.

### Network sniffer output
192.168.1.1:21 TCP
```
220 Welcomd to iCatch FTP Server
USER wificam
331 User name okay, need password.
PASS wificam
230 User logged in, proceed.
SYST215 UNIX TYPE: L8
PASV
227 Enter Passive Mode (192,168,1,1,192,1)
RETR /JPG/20170915_195648.JPG
150 File status okay, about to open data connection.
226 Closing data connection. Requested file action successful.
QUIT
221 Good bye.
```

### First test
Linux terminal:
* cd doesn't work
* File download needs to be binary
```
user@computer:~$ ftp 192.168.1.1
Connected to 192.168.1.1.
220 Welcomd to iCatch FTP Server
Name (192.168.1.1:user): wificam
331 User name okay, need password.
Password:
230 User logged in, proceed.
Remote system type is UNIX.

ftp> help
Commands may be abbreviated.  Commands are:

!          dir           mdelete    qc          site
$          disconnect    mdir       sendport    size
account    exit          mget       put         status
append     form          mkdir      pwd         struct
ascii      get           mls        quit        system
bell       glob          mode       quote       sunique
binary     hash          modtime    recv        tenex
bye        help          mput       reget       tick
case       idle          newer      rstatus     trace
cd         image         nmap       rhelp       type
cdup       ipany         nlist      rename      user
chmod      ipv4          ntrans     reset       umask
close      ipv6          open       restart     verbose
cr         lcd           prompt     rmdir       ?
delete     ls            passive    runique
debug      macdef        proxy      send

ftp> ls
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Jun 10 22:13 VIDEO
drw------- 1 user group 0 Jun 10 21:54 JPG
drw------- 1 user group 0 Jan 7 17:18 SYSTEM~1
-rw------- 1 user group 4 Jan 7 17:28 _disk_id.pod
226 Closing data connection. Requested file action successful.

ftp> dir VIDEO
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Dec 19 21:51 .
drw------- 1 user group 0 Dec 19 21:51 ..
-rw------- 1 user group 288028567 Jul 6 07:57 201707~1.MOV
-rw------- 1 user group 1294282334 Jul 6 09:40 201707~2.MOV
-rw------- 1 user group 128498552 Jul 6 10:42 201707~3.MOV
-rw------- 1 user group 31259876 Jul 6 10:45 201707~4.MOV
-rw------- 1 user group 23714753 Jul 8 08:52 201707~5.MOV
-rw------- 1 user group 1199789953 Jul 8 09:47 201707~6.MOV
-rw------- 1 user group 28268656 Jul 9 10:48 201707~7.MOV
-rw------- 1 user group 29296196 Jul 9 10:49 201707~8.MOV
-rw------- 1 user group 42177487 Jul 9 10:49 201707~9.MOV
-rw------- 1 user group 28810232 Jul 9 10:50 20170~10.MOV
-rw------- 1 user group 1089026492 Jul 9 10:54 20170~11.MOV
-rw------- 1 user group 579015866 Jul 9 11:02 20170~12.MOV
-rw------- 1 user group 42500467 Jul 9 16:00 20170~13.MOV
-rw------- 1 user group 25413442 Jul 9 16:01 20170~14.MOV
-rw------- 1 user group 32251406 Jul 9 16:03 20170~15.MOV
-rw------- 1 user group 53108217 Jul 9 16:07 20170~16.MOV
-rw------- 1 user group 108504889 Jul 9 16:07 20170~17.MOV
-rw------- 1 user group 54372308 Jul 9 16:08 20170~18.MOV
-rw------- 1 user group 77415762 Jul 10 16:52 20170~19.MOV
-rw------- 1 user group 375038113 Jul 12 12:19 20170~20.MOV
-rw------- 1 user group 532701980 Jul 12 12:23 20170~21.MOV
-rw------- 1 user group 134693378 Jul 12 12:40 20170~22.MOV
-rw------- 1 user group 471903227 Jul 12 12:41 20170~23.MOV
-rw------- 1 user group 193173909 Jul 12 12:45 20170~24.MOV
-rw------- 1 user group 261373362 Jul 12 12:47 20170~25.MOV
-rw------- 1 user group 725680174 Jul 12 12:50 20170~26.MOV
-rw------- 1 user group 7896492 Jul 12 13:14 20170~27.MOV
-rw------- 1 user group 26640698 Jul 12 13:15 20170~28.MOV
-rw------- 1 user group 1615563 Sep 15 19:33 20170~29.MOV
-rw------- 1 user group 133522649 Sep 15 19:33 20170~30.MOV
-rw------- 1 user group 1896899 Oct 3 11:32 20171~31.MOV
-rw------- 1 user group 2887887 Oct 3 11:38 20171~32.MOV
-rw------- 1 user group 1768987 Oct 3 11:38 20171~33.MOV
-rw------- 1 user group 1836615 Oct 3 11:41 20171~34.MOV
-rw------- 1 user group 4667673 Oct 3 11:50 20171~35.MOV
-rw------- 1 user group 1954425 Oct 3 11:50 20171~36.MOV
-rw------- 1 user group 2227515 Oct 3 11:52 20171~37.MOV
-rw------- 1 user group 3881179 Oct 3 11:53 20171~38.MOV
-rw------- 1 user group 2656633 Oct 3 11:56 20171~39.MOV
-rw------- 1 user group 1806321 Oct 3 11:57 20171~40.MOV
-rw------- 1 user group 2181519 Oct 3 12:01 20171~41.MOV
-rw------- 1 user group 2178501 Oct 3 12:01 20171~42.MOV
-rw------- 1 user group 2337317 Oct 3 12:02 20171~43.MOV
-rw------- 1 user group 2347097 Oct 3 12:03 20171~44.MOV
-rw------- 1 user group 2619839 Oct 3 12:03 20171~45.MOV
-rw------- 1 user group 13523739 Oct 3 12:03 20171~46.MOV
-rw------- 1 user group 3162253 Oct 3 12:08 20171~47.MOV
-rw------- 1 user group 4231281 Oct 3 12:18 20171~48.MOV
-rw------- 1 user group 3404543 Oct 3 12:19 20171~49.MOV
-rw------- 1 user group 7263007 Oct 3 12:23 20171~50.MOV
-rw------- 1 user group 524288 Oct 3 12:25 20171~51.MOV
-rw------- 1 user group 2460275 Oct 3 12:51 20171~52.MOV
-rw------- 1 user group 53154637 Oct 3 13:39 20171~54.MOV
-rw------- 1 user group 92012720 Oct 6 12:23 20171~55.MOV
-rw------- 1 user group 28740714 Oct 6 12:24 20171~56.MOV
-rw------- 1 user group 29999744 Oct 6 12:25 20171~57.MOV
-rw------- 1 user group 26433487 Oct 6 12:26 20171~58.MOV
-rw------- 1 user group 36563568 Oct 6 12:47 20171~59.MOV
-rw------- 1 user group 19486193 Oct 6 12:52 20171~60.MOV
-rw------- 1 user group 36590486 Oct 6 12:52 20171~61.MOV
-rw------- 1 user group 19727915 Oct 6 12:55 20171~62.MOV
-rw------- 1 user group 61499754 Oct 8 17:38 20171~63.MOV
-rw------- 1 user group 10882522 Oct 12 16:12 20171~64.MOV
226 Closing data connection. Requested file action successful.

ftp> cd ..
553 Requested action not taken. Illegal file name.

ftp> dir /
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Jun 10 22:13 VIDEO
drw------- 1 user group 0 Jun 10 21:54 JPG
drw------- 1 user group 0 Jan 7 17:18 SYSTEM~1
-rw------- 1 user group 4 Jan 7 17:28 _disk_id.pod
226 Closing data connection. Requested file action successful.

ftp> dir SYSTEM~1
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Jan 7 17:18 .
drw------- 1 user group 0 Jan 7 17:18 ..
-rw------- 1 user group 12 Jan 7 17:18 WPSETT~1.DAT
-rw------- 1 user group 76 Jan 7 17:18 INDEXE~1
226 Closing data connection. Requested file action successful.

ftp> get SYSTEM~1/INDEXE~1 rollei-ac-420-conf
local: rollei-ac-420-conf remote: SYSTEM~1/INDEXE~1
200 Command okay.
150 File status okay, about to open data connection.
226 Closing data connection. Requested file action successful.
76 bytes received in 0.01 secs (11.6 kB/s)

ftp> get SYSTEM~1/WPSETT~1.DAT rollei-ac-420-conf
local: rollei-ac-420-conf remote: SYSTEM~1/WPSETT~1.DAT
200 Command okay.
150 File status okay, about to open data connection.
226 Closing data connection. Requested file action successful.
12 bytes received in 0.00 secs (39.1 kB/s)

ftp> dir VIDEO
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Dec 19 21:51 .
drw------- 1 user group 0 Dec 19 21:51 ..
-rw------- 1 user group 288028567 Jul 6 07:57 201707~1.MOV
-rw------- 1 user group 1294282334 Jul 6 09:40 201707~2.MOV
-rw------- 1 user group 128498552 Jul 6 10:42 201707~3.MOV
-rw------- 1 user group 31259876 Jul 6 10:45 201707~4.MOV
-rw------- 1 user group 23714753 Jul 8 08:52 201707~5.MOV
-rw------- 1 user group 1199789953 Jul 8 09:47 201707~6.MOV
-rw------- 1 user group 28268656 Jul 9 10:48 201707~7.MOV
-rw------- 1 user group 29296196 Jul 9 10:49 201707~8.MOV
-rw------- 1 user group 42177487 Jul 9 10:49 201707~9.MOV
-rw------- 1 user group 28810232 Jul 9 10:50 20170~10.MOV
-rw------- 1 user group 1089026492 Jul 9 10:54 20170~11.MOV
-rw------- 1 user group 579015866 Jul 9 11:02 20170~12.MOV
-rw------- 1 user group 42500467 Jul 9 16:00 20170~13.MOV
-rw------- 1 user group 25413442 Jul 9 16:01 20170~14.MOV
-rw------- 1 user group 32251406 Jul 9 16:03 20170~15.MOV
-rw------- 1 user group 53108217 Jul 9 16:07 20170~16.MOV
-rw------- 1 user group 108504889 Jul 9 16:07 20170~17.MOV
-rw------- 1 user group 54372308 Jul 9 16:08 20170~18.MOV
-rw------- 1 user group 77415762 Jul 10 16:52 20170~19.MOV
-rw------- 1 user group 375038113 Jul 12 12:19 20170~20.MOV
-rw------- 1 user group 532701980 Jul 12 12:23 20170~21.MOV
-rw------- 1 user group 134693378 Jul 12 12:40 20170~22.MOV
-rw------- 1 user group 471903227 Jul 12 12:41 20170~23.MOV
-rw------- 1 user group 193173909 Jul 12 12:45 20170~24.MOV
-rw------- 1 user group 261373362 Jul 12 12:47 20170~25.MOV
-rw------- 1 user group 725680174 Jul 12 12:50 20170~26.MOV
-rw------- 1 user group 7896492 Jul 12 13:14 20170~27.MOV
-rw------- 1 user group 26640698 Jul 12 13:15 20170~28.MOV
-rw------- 1 user group 1615563 Sep 15 19:33 20170~29.MOV
-rw------- 1 user group 133522649 Sep 15 19:33 20170~30.MOV
-rw------- 1 user group 1896899 Oct 3 11:32 20171~31.MOV
-rw------- 1 user group 2887887 Oct 3 11:38 20171~32.MOV
-rw------- 1 user group 1768987 Oct 3 11:38 20171~33.MOV
-rw------- 1 user group 1836615 Oct 3 11:41 20171~34.MOV
-rw------- 1 user group 4667673 Oct 3 11:50 20171~35.MOV
-rw------- 1 user group 1954425 Oct 3 11:50 20171~36.MOV
-rw------- 1 user group 2227515 Oct 3 11:52 20171~37.MOV
-rw------- 1 user group 3881179 Oct 3 11:53 20171~38.MOV
-rw------- 1 user group 2656633 Oct 3 11:56 20171~39.MOV
-rw------- 1 user group 1806321 Oct 3 11:57 20171~40.MOV
-rw------- 1 user group 2181519 Oct 3 12:01 20171~41.MOV
-rw------- 1 user group 2178501 Oct 3 12:01 20171~42.MOV
-rw------- 1 user group 2337317 Oct 3 12:02 20171~43.MOV
-rw------- 1 user group 2347097 Oct 3 12:03 20171~44.MOV
-rw------- 1 user group 2619839 Oct 3 12:03 20171~45.MOV
-rw------- 1 user group 13523739 Oct 3 12:03 20171~46.MOV
-rw------- 1 user group 3162253 Oct 3 12:08 20171~47.MOV
-rw------- 1 user group 4231281 Oct 3 12:18 20171~48.MOV
-rw------- 1 user group 3404543 Oct 3 12:19 20171~49.MOV
-rw------- 1 user group 7263007 Oct 3 12:23 20171~50.MOV
-rw------- 1 user group 524288 Oct 3 12:25 20171~51.MOV
-rw------- 1 user group 2460275 Oct 3 12:51 20171~52.MOV
-rw------- 1 user group 53154637 Oct 3 13:39 20171~54.MOV
-rw------- 1 user group 92012720 Oct 6 12:23 20171~55.MOV
-rw------- 1 user group 28740714 Oct 6 12:24 20171~56.MOV
-rw------- 1 user group 29999744 Oct 6 12:25 20171~57.MOV
-rw------- 1 user group 26433487 Oct 6 12:26 20171~58.MOV
-rw------- 1 user group 36563568 Oct 6 12:47 20171~59.MOV
-rw------- 1 user group 19486193 Oct 6 12:52 20171~60.MOV
-rw------- 1 user group 36590486 Oct 6 12:52 20171~61.MOV
-rw------- 1 user group 19727915 Oct 6 12:55 20171~62.MOV
-rw------- 1 user group 61499754 Oct 8 17:38 20171~63.MOV
-rw------- 1 user group 10882522 Oct 12 16:12 20171~64.MOV
226 Closing data connection. Requested file action successful.

ftp> get VIDEO/20171~64.MOV rollei-ac-420-20171-64.MOV
local: rollei-ac-420-20171-64.MOV remote: VIDEO/20171~64.MOV
200 Command okay.
150 File status okay, about to open data connection.
WARNING! 43917 bare linefeeds received in ASCII mode
File may not have transferred correctly.
226 Closing data connection. Requested file action successful.
10882522 bytes received in 5.02 secs (2117.5 kB/s)

ftp> binary
200 Command okay.

ftp> get VIDEO/20171~64.MOV rollei-ac-420-20171-64.MOV
local: rollei-ac-420-20171-64.MOV remote: VIDEO/20171~64.MOV
200 Command okay.
150 File status okay, about to open data connection.
226 Closing data connection. Requested file action successful.
10882522 bytes received in 5.22 secs (2037.4 kB/s)

ftp> get VIDEO/20170~13.MOV rollei-ac-420-20170-13.MOV
local: rollei-ac-420-20170-13.MOV remote: VIDEO/20170~13.MOV
200 Command okay.
150 File status okay, about to open data connection.
226 Closing data connection. Requested file action successful.
42500467 bytes received in 17.29 secs (2400.7 kB/s)

ftp> dir JPG
Command not implemented for that parameter.
200 Command okay.
150 File status okay, about to open data connection.
drw------- 1 user group 0 Dec 19 21:51 .
drw------- 1 user group 0 Dec 19 21:51 ..
-rw------- 1 user group 5038913 Jun 29 13:25 201706~1.JPG
-rw------- 1 user group 5247720 Aug 26 14:09 201708~2.JPG
-rw------- 1 user group 5323913 Aug 26 14:09 201708~3.JPG
-rw------- 1 user group 4932391 Sep 15 19:56 201709~4.JPG
-rw------- 1 user group 5025340 Oct 12 16:16 201710~5.JPG
-rw------- 1 user group 5104763 Oct 12 16:17 201710~6.JPG
-rw------- 1 user group 5071430 Oct 12 16:17 201710~7.JPG
226 Closing data connection. Requested file action successful.

ftp> exit
221 Good bye.
```

### Using Firefox
Open the IP adress in Firefox as FTP:
```
ftp://192.168.1.1
```


### Filesystem
```
/
|- VIDEO
|  |- All videos (Naming see below)
|- JPG
|  |- All photos (Naming see below)
|- SYSTEM~1
   |- INDEXE~1
   |- WPSETT~1.DAT
```

#### Naminig of videos and photos

It is a combination of the date and an id. The name is 8 characters long. On the right is the id, left of it a ~ character and the remaining space is filled by the date of capturing in Format YYYYMM. It is shorten at the last character of date first, so following patterns should be possible:

* YYYYMM~X
* YYYYM~XX
* YYYY~XXX
* YYY~XXXX
* YY~XXXXX
* Y~XXXXXX
* ~XXXXXXX ??? I'am not sure, how the camera handle this, but i think nobody does so many shots
* XXXXXXXX ???

The Filename is followed by this extensions:
* Video: .MOV
* Photo: .JPG

```
201706~1.JPG
201708~2.JPG
201708~3.JPG
201709~4.JPG
201710~5.JPG
201710~6.JPG
201710~7.JPG
201710~8.JPG
201710~9.JPG
20171~10.JPG
20171~11.JPG
...
```

## 192.168.1.1:17540 (PTP)
```
?
```

## Sources
* https://plan.leipzig.freifunk.net/issues/269

## Other
Only some notes (guesses), not important or correct.
```
rtsp://192.168.1.1
rtsp://192.168.1.1/0
rtsp://192.168.1.1/SYSTEM~1/WPSETT~1.DAT
stsp://192.168.1.1/SYSTEM~1/WPSETT~1.DAT
ftp://192.168.1.1/SYSTEM~1/WPSETT~1.DAT
rtsp://192.168.1.1/videostream.asf
rtsp://192.168.1.1/videostream.sdp
rtsp://192.168.1.1/live.sdp
```
