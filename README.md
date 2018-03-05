# Build an IP camera with a Raspberry Pi and a camera module v2

## Build the image for SD card
``` bash
make raspberrypi_camera_rtsp_defconfig
make
```

You will find the image 'output/images/sdcard.img'

## Install the image to an SD card (find the device name of the SD card first!!!)
``` bash
sudo dd if=output/images/sdcard.img of=/dev/sdxx
```

## Commands to try the RTSP server:
- login to the Pi, the password is empty for root
- Copy the 'test-launch' from 'output/build/gst1-rtsp-server-1.12.3/examples/test-launch' in your development machine to the Pi
- Start RTSP server
``` bash
./test-launch "( rpicamsrc preview=false bitrate=4000000 keyframe-interval=30 ! video/x-h264, width=720, height=480, framerate=30/1 ! h264parse ! rtph264pay name=pay0 pt=96 )"
```
- On a Linux desktop, run below command to view the video
``` bash
gst-launch-1.0 rtspsrc location="rtsp://<Pi's IP>:8554/test" latency=0 ! rtph264depay ! decodebin ! videoconvert ! ximagesink
```
