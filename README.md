# VideoLatencyTest

# The machine used has VAAPI media stack enabled. Take note of the display refresh rate. Ideal to set display refresh rate to 120Hz.

# To create a raw 1280x360@120Hz YUV file of 60 seconds.
gst-launch-1.0 videotestsrc pattern=white num-buffers=7200 ! video/x-raw,width=1280,height=360,framerate=120/1 ! timeoverlay valignment=5 font-desc="Sans, 144" ypad=0 time-mode=0 auto-resize=false scale-mode=2 color=4278190080 draw-shadow=0 draw-outline=0 ! filesink location=timestamped_1280x360_120HZ_NV12.yuv

# To create the H264 file from YUV.
gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.yuv ! rawvideoparse width=1280 height=360 framerate=120/1 format=GST_VIDEO_FORMAT_NV12 ! x264enc ! filesink location=timestamped_1280x360_120HZ_NV12.h264

# To read and display the H264 content.
gst-launch-1.0 filesrc location=timestamped_1280x360_120HZ_NV12.h264 ! h264parse ! vaapih264dec ! vaapipostproc ! vaapisink sync=1
