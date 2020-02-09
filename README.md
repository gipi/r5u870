# R5U870 linux driver module

This is the porting of the module to the 4.x kernel; right now is not working.

```
$ fakeroot make \
    KDIR=/hack/buildroot/output/build/linux-4.19.16/ \
    DESTDIR=/hack/buildroot/output/target/ \
    FWDIR=/hack/buildroot/output/target/lib/firmware \
    KVER=4.19.16  \
    M=$PWD \
    install
```

## Where we are now?

The module compiles fine on 4.14 if ``CONFIG_VIDEOBUF_DMA_SG`` is enabled (and that
on rpi0 is all but easy!, probably you need to manually modify the option in ``KConfig`` and
add a ``prompt`` value otherwise is not selectable); also check that ``/dev/video0``
is present and the following is the message from the kernel:

```
Oct 28 19:11:04 raspberrypi0 user.info kernel: [24423.247993] usbcam: registering driver r5u870 0.11.3
Oct 28 19:11:04 raspberrypi0 user.info kernel: [24423.256927] r5u870: No DMI model found
Oct 28 19:11:04 raspberrypi0 user.info kernel: [24423.264503] r5u870-0: Detected HP Pavilion Webcam (WDM)
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.273507] r5u870-0: requesting microcode state
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.274913] r5u870-0: camera reports negative microcode state
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.274931] r5u870-0: loading microcode file "r5u870_1870.fw"
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.340478] r5u870-0: enabling microcode
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.340696] r5u870-0: requesting microcode version
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.340929] r5u870-0: camera reports version 0112
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341162] r5u870-0: Added 18 WDM controls
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341180] r5u870-0: control Brightness <= 63 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341190] r5u870-0: control Contrast <= 63 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341197] r5u870-0: control Hue <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341203] r5u870-0: control Saturation <= 63 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341209] r5u870-0: control Sharpness <= 63 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341216] r5u870-0: control Gamma <= 100 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341224] r5u870-0: control Auto White Balance <= 1 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341232] r5u870-0: control White Balance Red <= 127 [auto suppress]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341238] r5u870-0: control White Balance Blue <= 127 [auto suppress]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341244] r5u870-0: control Auto Exposure Control <= 1 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341253] r5u870-0: control Exposure <= 255 [auto suppress]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341259] r5u870-0: control Auto Gain Control <= 1 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341265] r5u870-0: control Gain <= 63 [auto suppress]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341270] r5u870-0: control Power Line Frequency <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341276] r5u870-0: control V-Flip <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341283] r5u870-0: control H-Flip <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341289] r5u870-0: control Privacy <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.debug kernel: [24423.341295] r5u870-0: control Night Mode <= 0 [capture off]
Oct 28 19:11:04 raspberrypi0 user.info kernel: [24423.351119] r5u870-0: registered as video0
Oct 28 19:11:04 raspberrypi0 user.info kernel: [24423.365257] usbcore: registered new interface driver r5u870
Oct 28 19:11:04 raspberrypi0 user.notice kernel: [24423.380562] **3PEI: usbcam_v4lopen(...) enter
Oct 28 19:11:05 raspberrypi0 user.notice kernel: [24423.403247] **3PEI: usbcam_v4lopen(...) exit with 0
Oct 28 19:11:05 raspberrypi0 user.debug kernel: [24423.403327] r5u870-0: received V4L ioctl: -2140645888
Oct 28 19:11:05 raspberrypi0 user.debug kernel: [24423.403327]
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.412180] ------------[ cut here ]------------
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.420779] WARNING: CPU: 0 PID: 467 at /usr/src/kernel/drivers/media/v4l2-core/v4l2-ioctl.c:1017 v4l_querycap+0x9c/0xd0 [videodev]
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.439802] Bad caps for driver r5u870, 5200001 0
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.439818] Modules linked in: r5u870(O) usbcam(O) videobuf_dma_sg videobuf_vmalloc videobuf_core v4l2_common videodev media ipv6 ctr ccm arc4 rtl8192cu rtl_usb rtl8192c_common rtlwifi mac80211 cfg80211 rfkill uio_pdrv_genir
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.485635] CPU: 0 PID: 467 Comm: v4l_id Tainted: G           O    4.14.68 #1
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.496681] Hardware name: BCM2835
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.503929] [<c00165b8>] (unwind_backtrace) from [<c0013f00>] (show_stack+0x20/0x24)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.519091] [<c0013f00>] (show_stack) from [<c0636c74>] (dump_stack+0x20/0x28)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.530406] [<c0636c74>] (dump_stack) from [<c002227c>] (__warn+0xe4/0x10c)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.541292] [<c002227c>] (__warn) from [<c00222e8>] (warn_slowpath_fmt+0x44/0x4c)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.556739] [<c00222e8>] (warn_slowpath_fmt) from [<bf59edf0>] (v4l_querycap+0x9c/0xd0 [videodev])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.574120] [<bf59edf0>] (v4l_querycap [videodev]) from [<bf59f6c8>] (__video_do_ioctl+0x2f4/0x318 [videodev])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.592834] [<bf59f6c8>] (__video_do_ioctl [videodev]) from [<bf59ee9c>] (video_usercopy+0x78/0x58c [videodev])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.611839] [<bf59ee9c>] (video_usercopy [videodev]) from [<bf59f3cc>] (video_ioctl2+0x1c/0x24 [videodev])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.630793] [<bf59f3cc>] (video_ioctl2 [videodev]) from [<bf6a08bc>] (usbcam_v4l_ioctl+0x44/0x60 [usbcam])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.650273] [<bf6a08bc>] (usbcam_v4l_ioctl [usbcam]) from [<bf59a66c>] (v4l2_ioctl+0xc8/0xec [videodev])
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.669786] [<bf59a66c>] (v4l2_ioctl [videodev]) from [<c0170290>] (do_vfs_ioctl+0xa0/0x774)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.688151] [<c0170290>] (do_vfs_ioctl) from [<c01709a8>] (SyS_ioctl+0x44/0x68)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.700559] [<c01709a8>] (SyS_ioctl) from [<c000fe80>] (ret_fast_syscall+0x0/0x28)
Oct 28 19:11:05 raspberrypi0 user.warn kernel: [24423.717975] ---[ end trace 3b34fea18310076d ]---
```

```
root@raspberrypi0:~# v4l2-compliance -T --device=/dev/video0
                VIDIOC_QUERYCAP returned 0 (Success)
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                VIDIOC_TRY_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCAP returned 0 (Success)
v4l2-compliance SHA   : not available

Driver Info:
        Driver name   : r5u870
        Card type     : HP Pavilion Webcam (WDM) #1
        Bus info      :
        Driver version: 0.11.3
        Capabilities  : 0x05200001
                Video Capture
                Read/Write
                Streaming
                Extended Pix Format

Compliance test for device /dev/video0 (not using libv4l2):

Required ioctls:
                VIDIOC_QUERYCAP returned 0 (Success)
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-compliance.cpp(314): string empty
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-compliance.cpp(538): check_ustring(vcap.bus_info, sizeof(vcap.bus_info))
        test VIDIOC_QUERYCAP: FAIL

Allow for multiple opens:
                VIDIOC_QUERYCAP returned 0 (Success)
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                VIDIOC_TRY_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
        test second video open: OK
                VIDIOC_QUERYCAP returned 0 (Success)
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-compliance.cpp(314): string empty
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-compliance.cpp(538): check_ustring(vcap.bus_info, sizeof(vcap.bus_info))
        test VIDIOC_QUERYCAP: FAIL
                VIDIOC_G_PRIORITY returned 0 (Success)
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-compliance.cpp(639): prio != match
        test VIDIOC_G/S_PRIORITY: FAIL
        test for unlimited opens: OK

                VIDIOC_G_INPUT returned 0 (Success)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_G_FMT returned 0 (Success)
                VIDIOC_CROPCAP returned -1 (Inappropriate ioctl for device)
                VIDIOC_CROPCAP returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
                VIDIOC_G_CTRL returned 0 (Success)
Debug ioctls:
                VIDIOC_DBG_G_REGISTER returned -1 (Inappropriate ioctl for device)
        test VIDIOC_DBG_G/S_REGISTER: OK (Not Supported)
                VIDIOC_LOG_STATUS returned -1 (Inappropriate ioctl for device)
        test VIDIOC_LOG_STATUS: OK (Not Supported)

Input ioctls:
                VIDIOC_G_STD returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_TUNER returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S_TUNER/ENUM_FREQ_BANDS: OK (Not Supported)
                VIDIOC_G_FREQUENCY returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_FREQUENCY returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S_FREQUENCY: OK (Not Supported)
                VIDIOC_S_HW_FREQ_SEEK returned -1 (Inappropriate ioctl for device)
        test VIDIOC_S_HW_FREQ_SEEK: OK (Not Supported)
                VIDIOC_ENUMAUDIO returned -1 (Inappropriate ioctl for device)
        test VIDIOC_ENUMAUDIO: OK (Not Supported)
                VIDIOC_G_INPUT returned 0 (Success)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUMINPUT returned -1 (Invalid argument)
                VIDIOC_S_INPUT returned -1 (Invalid argument)
                VIDIOC_S_INPUT returned 0 (Success)
        test VIDIOC_G/S/ENUMINPUT: OK
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_G_AUDIO returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_AUDIO returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S_AUDIO: OK (Not Supported)
        Inputs: 1 Audio Inputs: 0 Tuners: 0

Output ioctls:
                VIDIOC_G_MODULATOR returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S_MODULATOR: OK (Not Supported)
                VIDIOC_G_FREQUENCY returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_FREQUENCY returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S_FREQUENCY: OK (Not Supported)
                VIDIOC_ENUMAUDOUT returned -1 (Inappropriate ioctl for device)
        test VIDIOC_ENUMAUDOUT: OK (Not Supported)
                VIDIOC_G_OUTPUT returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUMOUTPUT returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_OUTPUT returned -1 (Inappropriate ioctl for device)
        test VIDIOC_G/S/ENUMOUTPUT: OK (Not Supported)
        test VIDIOC_G/S_AUDOUT: OK (Not Supported)
        Outputs: 0 Audio Outputs: 0 Modulators: 0

Input/Output configuration ioctls:
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_G_STD returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_STD returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_STD returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUMSTD returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYSTD returned -1 (Inappropriate ioctl for device)
        test VIDIOC_ENUM/G/S/QUERY_STD: OK (Not Supported)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_G_DV_TIMINGS returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUM_DV_TIMINGS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERY_DV_TIMINGS returned -1 (Inappropriate ioctl for device)
        test VIDIOC_ENUM/G/S/QUERY_DV_TIMINGS: OK (Not Supported)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_DV_TIMINGS_CAP returned -1 (Inappropriate ioctl for device)
        test VIDIOC_DV_TIMINGS_CAP: OK (Not Supported)
                VIDIOC_ENUMINPUT returned 0 (Success)
                VIDIOC_S_INPUT returned 0 (Success)
        test VIDIOC_G/S_EDID: OK (Not Supported)

Test input 0:

                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_ENUMINPUT returned 0 (Success)
        Control ioctls:
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                test VIDIOC_QUERY_EXT_CTRL/QUERYMENU: OK (Not Supported)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned 0 (Success)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                test VIDIOC_QUERYCTRL: OK
                VIDIOC_G_CTRL returned -1 (Invalid argument)
                VIDIOC_S_CTRL returned -1 (Invalid argument)
                test VIDIOC_G/S_CTRL: OK
                VIDIOC_G_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G/S/TRY_EXT_CTRLS: OK (Not Supported)
                test VIDIOC_(UN)SUBSCRIBE_EVENT/DQEVENT: OK (Not Supported)
                VIDIOC_G_JPEGCOMP returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_JPEGCOMP returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G/S_JPEGCOMP: OK (Not Supported)
                Standard Controls: 0 Private Controls: 0

        Format ioctls:
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned 0 (Success)
                VIDIOC_ENUM_FRAMESIZES returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUM_FMT returned 0 (Success)
                VIDIOC_ENUM_FRAMESIZES returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FRAMESIZES returned -1 (Inappropriate ioctl for device)
                VIDIOC_ENUM_FRAMEINTERVALS returned -1 (Inappropriate ioctl for device)
                test VIDIOC_ENUM_FMT/FRAMESIZES/FRAMEINTERVALS: OK
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_PARM returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_PARM returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G/S_PARM: OK (Not Supported)
                VIDIOC_G_FBUF returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G_FBUF: OK (Not Supported)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned 0 (Success)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned -1 (Invalid argument)
                test VIDIOC_G_FMT: OK
                VIDIOC_G_FMT returned 0 (Success)
                VIDIOC_TRY_FMT returned 0 (Success)
                VIDIOC_TRY_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned 0 (Success)
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(717): TRY_FMT cannot handle an invalid pixelformat.
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(718): This may or may not be a problem. For more information see:
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(719): http://www.mail-archive.com/linux-media@vger.kernel.org/msg56550.html
                VIDIOC_TRY_FMT returned 0 (Success)
                VIDIOC_TRY_FMT returned -1 (Invalid argument)
                test VIDIOC_TRY_FMT: OK
                VIDIOC_G_FMT returned 0 (Success)
                VIDIOC_S_FMT returned -1 (Invalid argument)
                VIDIOC_G_FMT returned 0 (Success)
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(977): S_FMT cannot handle an invalid pixelformat.
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(978): This may or may not be a problem. For more information see:
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(979): http://www.mail-archive.com/linux-media@vger.kernel.org/msg56550.html
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_FMT returned -1 (Invalid argument)
                VIDIOC_ENUM_FMT returned 0 (Success)
                VIDIOC_ENUM_FMT returned 0 (Success)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_FMT returned -1 (Device or resource busy)
                warn: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-formats.cpp(870): Could not set fmt2
                VIDIOC_S_FMT returned 0 (Success)
                test VIDIOC_S_FMT: OK
                VIDIOC_G_SLICED_VBI_CAP returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G_SLICED_VBI_CAP: OK (Not Supported)
                VIDIOC_CROPCAP returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_CROP returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                test Cropping: OK (Not Supported)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                test Composing: OK (Not Supported)
                VIDIOC_G_FMT returned 0 (Success)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_FMT returned 0 (Success)
                test Scaling: OK

        Codec ioctls:
                VIDIOC_ENCODER_CMD returned -1 (Inappropriate ioctl for device)
                test VIDIOC_(TRY_)ENCODER_CMD: OK (Not Supported)
                VIDIOC_G_ENC_INDEX returned -1 (Inappropriate ioctl for device)
                test VIDIOC_G_ENC_INDEX: OK (Not Supported)
                VIDIOC_DECODER_CMD returned -1 (Inappropriate ioctl for device)
                test VIDIOC_(TRY_)DECODER_CMD: OK (Not Supported)

        Buffer ioctls:
                VIDIOC_QUERYCAP returned 0 (Success)
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                VIDIOC_TRY_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_REQBUFS returned -1 (Invalid argument)
                VIDIOC_REQBUFS returned -1 (Invalid argument)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_REQBUFS returned -1 (Invalid argument)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_QUERYBUF returned -1 (Invalid argument)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_QUERYBUF returned -1 (Invalid argument)
                fail: ../../../v4l-utils-1.12.3/utils/v4l2-compliance/v4l2-test-buffers.cpp(510): Expected -1, got 1
                test VIDIOC_REQBUFS/CREATE_BUFS/QUERYBUF: FAIL
                VIDIOC_REQBUFS returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_QUERYBUF returned 0 (Success)
                VIDIOC_EXPBUF returned -1 (Inappropriate ioctl for device)
                VIDIOC_EXPBUF returned -1 (Inappropriate ioctl for device)
                VIDIOC_REQBUFS returned 0 (Success)
                test VIDIOC_EXPBUF: OK (Not Supported)

                VIDIOC_QUERYCAP returned 0 (Success)
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                VIDIOC_TRY_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
Test input 0:

                VIDIOC_S_INPUT returned 0 (Success)

                VIDIOC_QUERYCAP returned 0 (Success)
                VIDIOC_QUERY_EXT_CTRL returned -1 (Inappropriate ioctl for device)
                VIDIOC_TRY_EXT_CTRLS returned -1 (Inappropriate ioctl for device)
                VIDIOC_QUERYCTRL returned -1 (Invalid argument)
                VIDIOC_G_SELECTION returned -1 (Inappropriate ioctl for device)
                VIDIOC_S_INPUT returned 0 (Success)
                VIDIOC_S_FMT returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
                VIDIOC_S_CTRL returned 0 (Success)
Total: 43, Succeeded: 39, Failed: 4, Warnings: 7

```
