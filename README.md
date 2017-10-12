# Rollei AC 420
Reverse Engineering of the Rollei AC 420 WiFi connection

## Play Livestream
```
rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000
rtsp://192.168.1.1/MJPG?W=720&H=400&Q=50&BR=5000000/track1
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
But you can use every username and password.

### Test
Linux terminal:
* cd doesn't work
* File download needs to be binary
```

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
