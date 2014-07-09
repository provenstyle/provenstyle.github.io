---
layout: post
title: "FFMPEG: Analyzing audio and video  files"
date: 2014-07-09 17:00
comments: true
categories: FFMPEG
---

`ffmpeg -i c:\temp\video.mpg`

`-i` is the input argument.  By specifying an input, but no output, FFMPEG will inspect the file and give you lots of information such as:

###Video
- Encoding
- Streams
- Bit Rate
- Aspect Ratio
- Frame Rate

###Audio
- Encoding
- Bit depth
- Sampling Frequency
- Channels

### Example Output
	ffmpeg.exe -i "C:\temp\audio\image\VIDEO_TS\VTS_01_1.VOB"                                                                                                          
	ffmpeg version N-42347-g299387e Copyright (c) 2000-2012 the FFmpeg developers                                          
  	built on Jul  8 2012 15:44:54 with gcc 4.7.1                                                                         
  	configuration: --enable-gpl --enable-version3 --disable-w32threads --enable-runtime-cpudetect --enable-avisynth --enable-bzlib --enable-frei0r -enable libass --enable-libcelt --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libfreetype --enable-libgsm --enable-libmp3lame --enable-libnut --enable-libopenjpeg --enable-librtmp --enable-libschroedinger --enable-libspeex --enable-libtheora --enable-libutvideo --enable-libvo-aacenc --enable-libvo-amrwbenc --enable-libvorbis --enable-libvpx --enable-libx264 --enable-libxavs --enable-libxvid --enable-zlib                         
  		libavutil      51. 64.100 / 51. 64.100                                                                               
  		libavcodec     54. 33.100 / 54. 33.100                                                                               
  		libavformat    54. 15.102 / 54. 15.102                                                                               
  		libavdevice    54.  1.100 / 54.  1.100                                                                               
  		libavfilter     3.  1.100 /  3.  1.100                                                                               
  		libswscale      2.  1.100 /  2.  1.100                                                                               
  		libswresample   0. 15.100 /  0. 15.100                                                                               
  		libpostproc    52.  0.100 / 52.  0.100                                                                               
	[mpeg @ 01edf700] max_analyze_duration 5000000 reached at 5016000                                                      
	Input #0, mpeg, from 'C:\temp\audio\image\VIDEO_TS\VTS_01_1.VOB':                                                      
  	Duration: 00:03:06.80, start: 0.500000, bitrate: 6265 kb/s                                                           
    Stream #0:0[0x1e0]: Video: mpeg2video (Main), yuv420p, 720x480 [SAR 8:9 DAR 4:3], 104857 kb/s, 30 fps, 30 tbr, 90k tbn, 60 tbc                                                                                                            
    Stream #0:1[0x1c0]: Audio: mp2, 48000 Hz, stereo, s16, 128 kb/s                                                    
	At least one output file must be specified                                                                             
