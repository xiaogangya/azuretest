<properties 
	pageTitle="Media Encoder Standard formats and codecs" 
	description="This topic gives an overview of Azure Media Encoder Standard formats and codecs." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/11/2015"
	ms.author="juliako"/>

#Media Encoder Standard Formats and Codecs


This document contains a list of the most common import and export file formats that you can use with Media Encoder Standard.


##Input Container/File Formats

File formats (file extensions)|Supported
---|---|---|---
FLV (with H.264 and AAC codecs) (.flv)			|Yes 
MXF	(.mxf)					|Yes 
GXF	(.gxf)					|Yes 
MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp)	|Yes 
Windows Media Video (WMV)/ASF (.wmv, .asf) |Yes 
AVI (Uncompressed 8bit/10bit) (.avi)|Yes 
MP4/ISMV (.ismv)|Yes 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Yes 
Matroska/WebM (.mkv)		|Yes 
WAVE/WAV (.wav)	|Yes 
 

##Input Video Codecs

Input Video Codecs|Supported
---|---|---|---
AVC 8-bit/10-bit, up to 4:2:2, including AVCIntra	|8 bit 4:2:0 and 4:2:2 
Avid DNxHD (in MXF)									|Yes 
DVCPro/DVCProHD (in MXF)							|Yes 
JPEG 2000											|Yes 
MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)|Up to 422 Profile 
MPEG-1												|Yes 
VC-1/WMV9											|Yes 
Canopus HQ/HQX										|No 
MPEG-4 Part 2										|Yes 
[Theora](https://en.wikipedia.org/wiki/Theora)		|Yes 
YUV420 uncompressed, or mezzanine					|Yes


##Input Audio Codecs

Input Audio Codecs|Supported
---|---|---|---
AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)|Yes 
MPEG Layer 2|Yes 
MP3 (MPEG-1 Audio Layer 3)|Yes 
Windows Media Audio|Yes 
WAV/PCM|Yes 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Yes 
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format) |Yes 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Yes 
AMR (adaptive multi-rate)|Yes
AES (SMPTE 331M and 302M, AES3-2003)		|No 
Dolby® E									|No 
Dolby® Digital (AC3)						|No 
Dolby® Digital Plus (E-AC3)					|No 


##Output Formats and codecs

The following table lists the codecs and file formats that are supported for export.


File Format|Video Codec|Audio Codec
---|---|---
MP4 (* .mp4)<br/><br/>(including multi-bitrate MP4 containers) |H.264 (High, Main, and Baseline Profiles)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2-TS |H.264 (High, Main, and Baseline Profiles)|AAC-LC, HE-AAC v1, HE-AAC v2 

##See also

[Encoding On-Demand Content with Azure Media Services](media-services-encode-asset.md)

[How to encode with Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

test
