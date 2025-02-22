# U-Boot

# Kernel
```bash
# 步骤1: 清理环境
killall weston 2 >/dev/null
ps | grep -n demo | awk '{print $2}' | xargs kill

mkdir -p -m 0700 /var/lib/xdg
echo "output:all:freeze" > /tmp/.weston_drm.conf

export WESTON_DRM_MIRROR=1
export WESTON_DRM_KEEP_RATIO=1
export PIXMAN_USE_RGA=1
export GST_VIDEO_CONVERT_USE_RGA=1
export GST_V4L2_PREFERRED_FOURCC=NV12
export XDG_RUNTIME_DIR="/var/lib/xdg"
export WAYLAND_DISPLAY="wayland-1"

echo $WAYLAND_DISPLAY
echo $XDG_RUNTIME_DIR

ls -ld $XDG_RUNTIME_DIR
ls -la $XDG_RUNTIME_DIR

ls -l /dev/dri/card0
cat /etc/xdg/weston/weston.ini

# 步骤2: 启动Weston（嵌入式平台推荐参数）
weston \
  --drm-device=card0 \
  --backend=drm \
  --idle-time=0 \
  --renderer=pixman &

ps | grep weston

# 步骤3: 视频抓取
GST_DEBUG=3 gst-launch-1.0 v4l2src device="/dev/video0" ! video/x-raw, width=1280, height=720 ! waylandsink
```

# 测试结果
```bash
root@AM62x:~# killall weston 2 >/dev/null
killall: weston: no process killed
killall: 2: no process killed
root@AM62x:~# ps | grep -n demo | awk '{print $2}' | xargs kill
kill: sending signal to 972 failed: No such process
root@AM62x:~# mkdir -p -m 0700 /var/lib/xdg
[1]-  Terminated              ${awtk_file} > /dev/null 2>&1
root@AM62x:~# echo "output:all:freeze" > /tmp/.weston_drm.conf


root@AM62x:~# export WESTON_DRM_MIRROR=1
root@AM62x:~# export WESTON_DRM_KEEP_RATIO=1
root@AM62x:~# export PIXMAN_USE_RGA=1
root@AM62x:~# export GST_VIDEO_CONVERT_USE_RGA=1
root@AM62x:~# export GST_V4L2_PREFERRED_FOURCC=NV12
root@AM62x:~# export XDG_RUNTIME_DIR="/var/lib/xdg"
root@AM62x:~# export WAYLAND_DISPLAY="wayland-1"


root@AM62x:~# echo $WAYLAND_DISPLAY
wayland-1
root@AM62x:~# echo $XDG_RUNTIME_DIR
/var/lib/xdg
root@AM62x:~# ls -ld $XDG_RUNTIME_DIR
drwx------ 2 root root 4096 10月 21 23:39 /var/lib/xdg
root@AM62x:~# ls -la $XDG_RUNTIME_DIR
total 12
drwx------ 2 root root 4096 10月 21 23:39 .
drwxr-xr-x 1 root root 4096 10月 21 23:23 ..
root@AM62x:~#
root@AM62x:~# ls -l /dev/dri/card0
crw-rw---- 1 root video 226, 0 10月 21 23:42 /dev/dri/card0
root@AM62x:~# cat /etc/xdg/weston/weston.ini
[core]
require-input=false

[shell]
locking=false
animation=zoom
panel-position=top
startup-animation=fade

[screensaver]
# Uncomment path to disable screensaver
#path=@libexecdir@/weston-screensaver


root@AM62x:~# weston \
>   --drm-device=card0 \
>   --backend=drm \
>   --idle-time=0 \
>   --renderer=pixman &
[3] 981


root@AM62x:~# Date: 2023-10-21 CST
[23:43:01.668] weston 13.0.1
               https://wayland.freedesktop.org
               Bug reports to: https://gitlab.freedesktop.org/wayland/weston/issues/
               Build: 13.0.1
[23:43:01.669] Command line: weston --drm-device=card0 --backend=drm --idle-time=0 --renderer=pixman
[23:43:01.669] OS: Linux, 6.6.58-rt45-gc79d7ef3a56f-dirty, #1 SMP PREEMPT_RT Fri Feb 21 08:47:44 UTC 2025, aarch64
[23:43:01.669] Flight recorder: enabled
[23:43:01.670] Using config file '/etc/xdg/weston/weston.ini'
[23:43:01.671] Output repaint window is 7 ms maximum.
[23:43:01.675] Loading module '/usr/lib/libweston-13/drm-backend.so'
[23:43:01.686] initializing drm backend
[23:43:01.687] Trying libseat launcher...
[23:43:01.689] [seatd/seat.c:39] Created VT-bound seat seat0
[23:43:01.689] [seatd/server.c:145] New client connected (pid: 981, uid: 0, gid: 0)
[23:43:01.689] [libseat/backend/seatd.c:633] Started embedded seatd
[23:43:01.689] [seatd/seat.c:170] Added client 2 to seat0
[23:43:01.689] [seatd/seat.c:480] Opened client 2 on seat0
[23:43:01.689] [libseat/libseat.c:73] Seat opened with backend 'builtin'
[23:43:01.690] [libseat/backend/seatd.c:212] Enabling seat
[23:43:01.690] libseat: session control granted
[23:43:01.692] using /dev/dri/card0
[23:43:01.692] DRM: supports atomic modesetting
[23:43:01.692] DRM: supports GBM modifiers
[23:43:01.692] DRM: does not support async page flipping
[23:43:01.692] DRM: supports picture aspect ratio
[23:43:01.692] Using Pixman renderer
[23:43:01.716] event3  - nvp6188_input_event: not tagged as supported input device
[23:43:01.743] event3  - not using input device '/dev/input/event3'
[23:43:01.745] event0  - pwm-beeper: not tagged as supported input device
[23:43:01.762] event0  - not using input device '/dev/input/event0'
[23:43:01.763] event1  - goodix-ts: is tagged by udev as: Touchscreen
[23:43:01.764] event1  - goodix-ts: device is a touch device
[23:43:01.766] event2  - goodix-ts: is tagged by udev as: Touchscreen
[23:43:01.766] event2  - goodix-ts: device is a touch device
[23:43:01.767] Touchscreen - goodix-ts - /sys/devices/virtual/input/input1/event1
[23:43:01.767] libinput: configuring device "goodix-ts".
[23:43:01.767] input device event1 has no enabled output associated (none named), skipping calibration for now.
[23:43:01.767] Touchscreen - goodix-ts - /sys/devices/virtual/input/input2/event2
[23:43:01.767] libinput: configuring device "goodix-ts".
[23:43:01.767] input device event2 has no enabled output associated (none named), skipping calibration for now.
[23:43:01.768] DRM: head 'LVDS-1' found, connector 40 is connected, EDID make 'unknown', model 'unknown', serial ''
               Supported EOTF modes: SDR
[23:43:01.768] Registered plugin API 'weston_drm_output_api_v1' of size 40
[23:43:01.768] Color manager: no-op
[23:43:01.768] Output 'LVDS-1' attempts EOTF mode: SDR
[23:43:01.768] Output 'LVDS-1' using color profile: stock sRGB colo[   66.772204] weston-keyboard[983]: memfd_create() called without MFD_EXEC or MFD_NOEXEC_SEAL set
[   66.772204] weston-desktop-[984]: memfd_create() called without MFD_EXEC or MFD_NOEXEC_SEAL set
r profile
[23:43:01.805] DRM: output LVDS-1 uses shadow framebuffer.
[23:43:01.805] Initialized backlight for head 'LVDS-1', device /sys/class/backlight/backlight
[23:43:01.805] Output LVDS-1 (crtc 38) video modes:
               1280x800@59.1, preferred, current, 70.0 MHz
[23:43:01.805] associating input device event1 with output LVDS-1 (none by udev)
[23:43:01.806] associating input device event2 with output LVDS-1 (none by udev)
[23:43:01.806] Output 'LVDS-1' enabled with head(s) LVDS-1
[23:43:01.806] Compositor capabilities:
               arbitrary surface rotation: yes
               screen capture uses y-flip: no
               cursor planes: yes
               arbitrary resolutions: no
               view mask clipping: yes
               explicit sync: no
               color operations: no
               presentation clock: CLOCK_MONOTONIC, id 1
               presentation clock resolution: 0.000000001 s
[23:43:01.812] Loading module '/usr/lib/weston/desktop-shell.so'
[23:43:01.814] launching '/usr/libexec/weston-keyboard'
[23:43:01.817] launching '/usr/libexec/weston-desktop-shell'
could not load cursor 'dnd-move'
could not load cursor 'dnd-move'
could not load cursor 'dnd-copy'
could not load cursor 'dnd-copy'
could not load cursor 'dnd-none'
could not load cursor 'dnd-none'
Fontconfig warning: "/usr/share/fontconfig/conf.avail/44-wqy-zenhei.conf", line 11: Fontconfig warning: "/usr/share/fontconfig/conf.avail/44-wqy-zenhei.conf", line 11: Having multiple values in <test> isn't supported and may not work as expectedHaving multiple values in <test> isn't supported and may not work as expected

root@AM62x:~# ps | grep weston
  981 root     weston --drm-device=card0 --backend=drm --idle-time=0 --renderer=pixman
  982 root     weston --drm-device=card0 --backend=drm --idle-time=0 --renderer=pixman
  983 root     /usr/libexec/weston-keyboard
  984 root     /usr/libexec/weston-desktop-shell
  986 root     grep weston


root@AM62x:~# GST_DEBUG=3 gst-launch-1.0 v4l2src device="/dev/video0" ! video/x-raw, width=1280, height=720 ! waylandsink
[   95.911853] nvp6188 1-0030: nvp6188_open
[   96.090557] nvp6188 1-0030: nvp6188_open
设置暂停管道 ...

(gst-launch-1.0:987): GStreamer-Wayland-WARNING **: 23:43:31.277: Could not bind to zwp_linux_dmabuf_v1
管道正在使用且不需要 PREROLL ...
0:00:00.389924610   987 0xffffa8000b70 WARN                    v4l2 gstv4l2object.c:4534:gst_v4l2_object_get_crop_rect:<v4l2src0:src> VIDIOC_CROPCAP failed
管道被 PREROLLED ...
设置播放管道 ...
New clock: GstSystemClock
0:00:00.391786925   987 0xffffa8000b70 WARN                    v4l2 gstv4l2object.c:4534:gst_v4l2_object_get_crop_rect:<v4l2src0:src> VIDIOC_CROPCAP failed
[   96.175021] j721e-csi2rx 30102000.ticsi2rx: Width does not match (source 640, sink 1280)
0:00:00.438381180   987 0xffffa8000b70 ERROR         v4l2bufferpool gstv4l2bufferpool.c:714:gst_v4l2_buffer_pool_streamon:<v4l2src0:pool0:src> error with STREAMON 32 (Broken pipe)
0:00:00.450492965   987 0xffffa8000b70 ERROR             bufferpool gstbufferpool.c:572:gst_buffer_pool_set_active:<v4l2src0:pool0:src> start failed
0:00:00.452119805   987 0xffffa8000b70 WARN                 v4l2src gstv4l2src.c:949:gst_v4l2src_decide_allocation:<v4l2src0> error: 分配请求的内存失败。
0:00:00.452216425   987 0xffffa8000b70 WARN                 v4l2src gstv4l2src.c:949:gst_v4l2src_decide_allocation:<v4l2src0> error: Buffer pool activation failed
错误：来自组件 /GstPipeline:pipeline0/GstV4l2Src:v4l2src0：分配请求的内存失败。
额外的调试信息：
../sys/v4l2/gstv4l2src.c(949): gst_v4l2src_decide_allocation (): /GstPipeline:pipeline0/GstV4l2Src:v4l2src0:
Buffer pool activation failed
0:00:00.452630535   987 0xffffa8000b70 WARN                 basesrc gstbasesrc.c:3352:gst_base_src_prepare_allocation:<v4l2src0> Subclass failed to decide allocation
0:00:00.452723170   987 0xffffa8000b70 WARN                 basesrc gstbasesrc.c:3132:gst_base_src_loop:<v4l2src0> error: Internal data stream error.
Execution ended after 0:00:00.062199045
0:00:00.452757900   987 0xffffa8000b70 WARN                 basesrc gstbasesrc.c:3132:gst_base_src_loop:<v4l2src0> error: streaming stopped, reason not-negotiated (-4)
设置 NULL 管道 ...
错误：来自组件 /GstPipeline:pipeline0/GstV4l2Src:v4l2src0：Internal data stream error.
额外的调试信息：
../libs/gst/base/gstbasesrc.c(3132): gst_base_src_loop (): /GstPipeline:pipeline0/GstV4l2Src:v4l2src0:
streaming stopped, reason not-negotiated (-4)
释放管道资源 ...
```