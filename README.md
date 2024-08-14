# VideoLatencyTest

## Overview
This is a simple guide on how to use a video playback for measurement of glass-to-glass latency of video capturing device.

## Ensuring the machine playing H264 video does so at correct speed and framerate
Take note of the display refresh rate. It is ideal to set the machine's display refresh rate to match the H264 video's framerate, this will ensure synchronized display of every frames. For this case, the sample H264 video file has 120Hz framerate, so set the display refresh rate to 120Hz.
Check to ensure the machine playing the H264 video is not CPU bottlenecked before using the setup to benchmark latencies. This can be done with the help of a handheld stopwatch. (Use hardware accelerated decoders for playbacks whenever possible.)
- Start the handheld stopwatch and play the H264 video simultanously. Your actions are not perfect, therefore, there is a reasonably small time gap (not noticeable to the naked eye) between the playing of H264 file and the handheld stopwatch.
- The H264 video duration is 60 seconds. Try to accurately stop the handheld stopwatch at the 60 second mark of the H264 video. The time difference on the handheld stopwatch should be within a reasonable number (e.g. human reaction <±100ms) near 60 second mark.
- Repeat several times to weed out reaction time variations and ensure the results are somewhat consistent.
- Now you are good to go!

> If after several tries and the time on the handheld stopwatch is *way* different, there is some synchronization issue. Possibly, the H264 video is not playing according to *true* time sync.

## Creating the H264 video for your own use (for cases if your wish to have your H264 video framerate to match your display's).
> **Create a 1280x360@120Hz YUV file of 60 seconds:** `gst-launch-1.0 videotestsrc pattern=white num-buffers=7200 ! video/x-raw,width=1280,height=360,framerate=120/1 ! timeoverlay valignment=5 font-desc="Sans, 144" ypad=0 time-mode=0 auto-resize=false scale-mode=2 color=4278190080 draw-shadow=0 draw-outline=0 ! filesink location=timestamped_1280x360_120HZ_NV12.yuv`

> **Create the H264 file from YUV:** `gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.yuv ! rawvideoparse width=1280 height=360 framerate=120/1 format=GST_VIDEO_FORMAT_NV12 ! x264enc ! filesink location=timestamped_1280x360_120HZ_NV12.h264`

> My machine used has VAAPI media stack (hardware accelerated) enabled. Yours will differ.

> **Play the H264 video:** `gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.h264 ! h264parse ! vaapih264dec ! vaapipostproc ! vaapisink sync=1`
