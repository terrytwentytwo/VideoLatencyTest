# VideoLatencyTest

## Overview
The machine used has VAAPI media stack enabled. Take note of the display refresh rate. Ideal to set display refresh rate to 120Hz so to synchronize with the framerate.
Do a check to ensure the machine playing the H264 video is not CPU bottlenecked before using the setup to benchmark latencies. This can be done with the help of a handheld stopwatch. (Use hardware accelerated decoders for playbacks whenever possible.)
- Start the handheld stopwatch and play the H264 video simultanously. Your actions are not perfect, therefore, there is a reasonably small time gap (not noticeable to the naked eye) between the playing of H264 file and the handheld stopwatch.
- The H264 video duration is 60 seconds. Stop the handheld stopwatch at the 60 second mark of the H264 video. The time difference on the handheld stopwatch should be within a reasonable number (e.g. human reaction <100ms) near 60 second mark.
- Repeat several times to weed out reaction time variations.
- Now you are good to go!

## Creating the H264 video for your own use.
> **Create a 1280x360@120Hz YUV file of 60 seconds:** gst-launch-1.0 videotestsrc pattern=white num-buffers=7200 ! video/x-raw,width=1280,height=360,framerate=120/1 ! timeoverlay valignment=5 font-desc="Sans, 144" ypad=0 time-mode=0 auto-resize=false scale-mode=2 color=4278190080 draw-shadow=0 draw-outline=0 ! filesink location=timestamped_1280x360_120HZ_NV12.yuv

> **Create the H264 file from YUV:** gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.yuv ! rawvideoparse width=1280 height=360 framerate=120/1 format=GST_VIDEO_FORMAT_NV12 ! x264enc ! filesink location=timestamped_1280x360_120HZ_NV12.h264

> **Play the H264 video:** gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.h264 ! h264parse ! vaapih264dec ! vaapipostproc ! vaapisink sync=1
