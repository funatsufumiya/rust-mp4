MP4
=======

Chinese Version README: `README_cn.rst <README_cn.rst>`_

:Date: 09/16 2018

.. contents::

Running Example
----------------

.. code:: bash

    git clone https://github.com/LuoZijun/rust-mp4
    cd rust-mp4

    # Make sure there is a file named `a.mp4` in the current directory.
    cargo run --example mp4_to_h264

MP4 Track
--------------
Supported media types:

*   Audio
*   Video

Audio media supported formats:

*   MP3
*   AAC
*   FLAC
*   Opus
*   LPCM
*   ALAC
*   EncryptedAudio

Video media supported formats:

*   H264
*   MP4V
*   VP10
*   VP9
*   VP8
*   EncryptedVideo

H.264 Stream in MP4 Sample
----------------------------

When the `Track` media format in the `MP4` container is `H264`,
The `Sample` data in the MP4 is composed of continuous `AU` (Access Unit).

H.264 Stream Format
----------------------

*    Annex-B Byte Stream
*    AVC

In MP4, the H264 video stream uses the `AVC` encoding method.

*Annex-B Byte Stream*:

When encoding `H.264`, add a start code `0x00_00_01` before each `NAL`, and the decoder detects the start code in the bitstream, and the current `NAL` ends.
To prevent the occurrence of `0x00_00_01` in the `NAL`, `H.264` proposes an "emulation prevention" mechanism. After encoding a `NAL`, if two consecutive `0x00` bytes are detected, insert a `0x03` at the end.
When the decoder detects `0x00_00_03` in the `NAL`, discard the `0x03` and restore the original data.

::

    0x000000  >>>>>>  0x00000300
    0x000001  >>>>>>  0x00000301
    0x000002  >>>>>>  0x00000302
    0x000003  >>>>>>  0x00000303

H.264 Format Encoder
----------------------

*   `Cisco OpenH264 <https://github.com/cisco/openh264>`_ ,
*   `FFMPEG H264 <https://github.com/FFmpeg/FFmpeg/blob/master/libavcodec/h264.h>`_ ,

Or you can use `libx264 <https://git.videolan.org/?p=x264.git>`_ to write your own format.

`H.264` Data Analysis Tool: `h264bitstream <https://h264bitstream.sourceforge.io/>`_

On the `macOS` system, you can install it by `brew install h264bitstream`.

Terms Introduction
-------------------

*   `H264` is a video format, it is a series of `AU` elements arranged in decoding order (Coded video sequence).
*   `AU` is the full name of `Access Unit`, which is composed of a series of `NAL`.
*   `Sample` is the smallest media resource in the MP4 format, which is almost equivalent to what we usually call `video frame` or `Access Unit`.

Easily Confused Points
-----------------------

*   `NALUs` do not carry time information, time information is on `AUs`.
*   One MP4 H264 Video Sample = One AU(Access Unit) = one or more NALU

Reference
------------
*   `H.264 Specs <http://www.itu.int/rec/T-REC-H.264/en>`_ , Advanced video coding for generic audiovisual services
*   `Overview of the H.264/AVC Video Coding Standard <http://ip.hhi.de/imagecom_G1/assets/pdfs/csvt_overview_0305.pdf>`_
*   `StackoverFlow: h264 inside AVI, MP4 and “Raw” h264 streams. Different format of NAL units (or ffmpeg bug) <https://stackoverflow.com/questions/46601724/h264-inside-avi-mp4-and-raw-h264-streams-different-format-of-nal-units-or-f>`_
*   `Blog: Introduction to H.264: (1) NAL Unit <https://yumichan.net/video-processing/video-compression/introduction-to-h264-nal-unit/>`_
