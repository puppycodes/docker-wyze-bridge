[![Docker](https://github.com/mrlt8/docker-wyze-bridge/actions/workflows/docker-image.yml/badge.svg)](https://github.com/mrlt8/docker-wyze-bridge/actions/workflows/docker-image.yml)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/mrlt8/docker-wyze-bridge?logo=github)](https://github.com/mrlt8/docker-wyze-bridge/releases/latest)
[![Docker Image Size (latest semver)](https://img.shields.io/docker/image-size/mrlt8/wyze-bridge?sort=semver&logo=docker&logoColor=white)](https://hub.docker.com/r/mrlt8/wyze-bridge)
[![Docker Pulls](https://img.shields.io/docker/pulls/mrlt8/wyze-bridge?logo=docker&logoColor=white)](https://hub.docker.com/r/mrlt8/wyze-bridge)

# WebRTC/RTSP/RTMP/HLS Bridge for Wyze Cam

![479shots_so](https://user-images.githubusercontent.com/67088095/224595527-05242f98-c4ab-4295-b9f5-07051ced1008.png)


Create a local WebRTC, RTSP, RTMP, or HLS/Low-Latency HLS stream for most of your Wyze cameras including the outdoor, doorbell, and 2K cams. 

No third-party or special firmware required.

It just works!

Streams direct from camera without additional bandwidth or subscriptions.

Based on [@noelhibbard's script](https://gist.github.com/noelhibbard/03703f551298c6460f2fd0bfdbc328bd#file-readme-md) with [kroo/wyzecam](https://github.com/kroo/wyzecam) and [aler9/mediamtx](https://github.com/aler9/mediamtx).


Please consider ⭐️ starring or [☕️ sponsoring](https://ko-fi.com/mrlt8) this project if you found it useful, or use the [affiliate link](https://amzn.to/3NLnbvt) when shopping on amazon!

## Quick Start

Install [docker](https://docs.docker.com/get-docker/) and use your Wyze credentials to run:

(If your credentials have special characters, you must escape them)

```bash
docker run \
  -e WYZE_EMAIL=you@email.com \
  -e WYZE_PASSWORD=yourpassw0rd \
  -p 1935:1935 -p 8554:8554 -p 8888:8888 -p 5000:5000 \
  mrlt8/wyze-bridge:latest
```

You can then use the web interface at `http://localhost:5000` where localhost is the hostname or ip of the machine running the bridge.

See [basic usage](#basic-usage) for additional information or visit the [wiki page](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant) for additional information on using the bridge as a Home Assistant Add-on.

## What's Changed in v2.1.5

* FIX: set_alarm_on/set_alarm_off was inverted #795. Thanks @iferlive!
* NEW: `URI_MAC=true` to append last 4 characters of the MAC address to the URI to avoid conflicting URIs when multiple cameras share the same name. #760
* Home Assistant: Add RECORD_FILE_NAME option #791
* UPDATE: base image to bullseye.

## What's Changed in v2.1.4

* FIX: Record option would not auto-connect. #784 Thanks @JA16122000!
* 

## What's Changed in v2.1.2/3

* Increase close on-demand time to 60s to prevent reconnect messages. #643 #750 #764
* Disable default LL-HLS for compatibility with apple. LL-HLS can still be enabled with `LLHLS=true` which will generate the necessary SSL certificates to work on Apple devices.
* Disable MQTT if connection refused.
* UPDATED: MediaMTX to [v0.22.2](https://github.com/aler9/mediamtx/releases/tag/v0.22.2)


## What's Changed in v2.1.1

* FIXED: WebRTC on UDP Port #772
* UPDATED: MediaMTX to [v0.22.1](https://github.com/aler9/mediamtx/releases/tag/v0.22.1)
* ENV Options: Re-enable `ON_DEMAND` to toggle connection mode. #643 #750 #764

## What's Changed in v2.1.0

⚠️ This version updates the backend rtsp-simple-server to [MediaMTX](https://github.com/aler9/mediamtx) which may cause some issues if you're using custom rtsp-simple-server related configs.

* CHANGED: rtsp-simple-server to MediaMTX.
* ENV Options:
  * New: `SUB_QUALITY` - Specify the quality to be used for the substream. #755
  * New: `SNAPSHOT_FORMAT` - Specify the output file format when using `SNAPSHOT` which can be used to create a timelapse/save multiple snapshots. e.g., `SNAPSHOT_FORMAT={cam_name}/%Y-%m-%d/%H-%M.jpg` #757:
* Home Assistant/MQTT:
  * Fixed: MQTT auto-discovery error #751
  * New: Additional entities for each of the cameras.
  * Changed: Default IMG_DIR to `media/wyze/img/` #660


Known Issues/Bugs:

* WebUI Audio: Limited audio compatibility between HLS and WebRTC. May need to force the audio codec to `AUDIO_CODEC=aac` for HLS and `AUDIO_CODEC=libopus` for WebRTC.
* Substream: SD stream seems to have a stuttering issue due to the hardware encoding on the camera. May need to force a re-encode of the video using `FORCE_ENCODE=true`.


[View previous changes](https://github.com/mrlt8/docker-wyze-bridge/releases)


## Supported Cameras

![Wyze Cam V1](https://img.shields.io/badge/wyze_v1-yes-success.svg)
![Wyze Cam V2](https://img.shields.io/badge/wyze_v2-yes-success.svg)
![Wyze Cam V3](https://img.shields.io/badge/wyze_v3-yes-success.svg)
![Wyze Cam V3 Pro](https://img.shields.io/badge/wyze_v3_pro-yes-success.svg)
![Wyze Cam Floodlight](https://img.shields.io/badge/wyze_floodlight-yes-success.svg)
![Wyze Cam Pan](https://img.shields.io/badge/wyze_pan-yes-success.svg)
![Wyze Cam Pan V2](https://img.shields.io/badge/wyze_pan_v2-yes-success.svg)
![Wyze Cam Pan V3](https://img.shields.io/badge/wyze_pan_v3-yes-success.svg)
![Wyze Cam Pan Pro](https://img.shields.io/badge/wyze_pan_pro-yes-success.svg)
![Wyze Cam Outdoor](https://img.shields.io/badge/wyze_outdoor-yes-success.svg)
![Wyze Cam Outdoor V2](https://img.shields.io/badge/wyze_outdoor_v2-yes-success.svg)
![Wyze Cam Doorbell](https://img.shields.io/badge/wyze_doorbell-yes-success.svg)

![Wyze Cam Doorbell Pro](https://img.shields.io/badge/wyze_doorbell_pro-no-inactive.svg)
![Wyze Cam OG](https://img.shields.io/badge/wyze_og-no-inactive.svg)
![Wyze Cam OG 3x](https://img.shields.io/badge/wyze_og_3x-no-inactive.svg)

| Camera                        | Model          | Supported                                                   |
| ----------------------------- | -------------- | ----------------------------------------------------------- |
| Wyze Cam v1 [HD only]         | WYZEC1         | ✅                                                           |
| Wyze Cam V2                   | WYZEC1-JZ      | ✅                                                           |
| Wyze Cam V3                   | WYZE_CAKP2JFUS | ✅                                                           |
| Wyze Cam V3 Pro [2K]          | HL_CAM3P       | ✅                                                           |
| Wyze Cam Floodlight           | WYZE_CAKP2JFUS | ✅                                                           |
| Wyze Cam Pan                  | WYZECP1_JEF    | ✅                                                           |
| Wyze Cam Pan v2               | HL_PAN2        | ✅                                                           |
| Wyze Cam Pan v3               | HL_PAN3        | ✅                                                           |
| Wyze Cam Pan Pro [2K]         | HL_PANP        | ✅                                                           |
| Wyze Cam Outdoor              | WVOD1          | ✅                                                           |
| Wyze Cam Outdoor v2           | HL_WCO2        | ✅                                                           |
| Wyze Cam Doorbell             | WYZEDB3        | ✅                                                           |
| Wyze Battery Cam Pro          | AN_RSCW        | ❓                                                           |
| Wyze Cam Doorbell Pro 2       | AN_RDB1        | ❓                                                           |
| Wyze Cam Flood Light Pro [2K] | LD_CFP         | ❓                                                           |
| Wyze Cam Doorbell Pro         | GW_BE1         | [⚠️](https://github.com/mrlt8/docker-wyze-bridge/issues/276) |
| Wyze Cam OG                   | GW_GC1         | [⚠️](https://github.com/mrlt8/docker-wyze-bridge/issues/677) |
| Wyze Cam OG Telephoto 3x      | GW_GC2         | [⚠️](https://github.com/mrlt8/docker-wyze-bridge/issues/677) |


## Requirements

![Supports armv7 Architecture](https://img.shields.io/badge/armv7-yes-success.svg)
![Supports aarch64 Architecture](https://img.shields.io/badge/aarch64-yes-success.svg)
![Supports amd64 Architecture](https://img.shields.io/badge/amd64-yes-success.svg)

[![Home Assistant Add-on](https://img.shields.io/badge/home_assistant-add--on-blue.svg?logo=homeassistant&logoColor=white)](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant)
[![Homebridge](https://img.shields.io/badge/homebridge-camera--ffmpeg-blue.svg?logo=homebridge&logoColor=white)](https://sunoo.github.io/homebridge-camera-ffmpeg/configs/WyzeCam.html)
[![Portainer stack](https://img.shields.io/badge/portainer-stack-blue.svg?logo=portainer&logoColor=white)](https://github.com/mrlt8/docker-wyze-bridge/wiki/Portainer)
[![Unraid Community App](https://img.shields.io/badge/unraid-community--app-blue.svg?logo=unraid&logoColor=white)](https://github.com/mrlt8/docker-wyze-bridge/issues/236)

Should work on most x64 systems as well as on some arm-based systems like the Raspberry Pi.

The container can be run on its own, in [Portainer](https://github.com/mrlt8/docker-wyze-bridge/wiki/Portainer), [Unraid](https://github.com/mrlt8/docker-wyze-bridge/issues/236), as a [Home Assistant Add-on](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant), locally or remotely in the cloud.

## Basic Usage

### docker-compose (recommended)

This is similar to the docker run command, but will save all your options in a yaml file.

1. Install [Docker Compose](https://docs.docker.com/compose/install/).
2. Use the [sample](https://raw.githubusercontent.com/mrlt8/docker-wyze-bridge/main/docker-compose.sample.yml) as a guide to create a `docker-compose.yml` file with your wyze credentials.
3. Run `docker-compose up`.

Once you're happy with your config you can use `docker-compose up -d` to run it in detached mode.

NOTE: You may need to [update the WebUI links](https://github.com/mrlt8/docker-wyze-bridge/wiki/WebUI#custom-ports) if you're changing the ports or using a reverse proxy.

#### Updating your container

To update your container, `cd` into the directory where your `docker-compose.yml` is located and run:

```bash
docker-compose pull # Pull new image
docker-compose up -d # Restart container in detached mode
docker image prune # Remove old images
```

### 🏠 Home Assistant

Visit the [wiki page](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant) for additional information on Home Assistant.

[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fmrlt8%2Fdocker-wyze-bridge)


## Additional Info

* [Two-Factor Authentication (2FA/MFA)](https://github.com/mrlt8/docker-wyze-bridge/wiki/Two-Factor-Authentication)
* [ARM/Raspberry Pi](https://github.com/mrlt8/docker-wyze-bridge/wiki/Raspberry-Pi-(armv7-and-arm64))
* [Network Connection Modes](https://github.com/mrlt8/docker-wyze-bridge/wiki/Network-Connection-Modes)
* [Portainer](https://github.com/mrlt8/docker-wyze-bridge/wiki/Portainer)
* [Unraid](https://github.com/mrlt8/docker-wyze-bridge/issues/236)
* [Home Assistant](https://github.com/mrlt8/docker-wyze-bridge/wiki/Home-Assistant)
* [Homebridge Camera FFmpeg](https://sunoo.github.io/homebridge-camera-ffmpeg/configs/WyzeCam.html)
* [HomeKit Secure Video](https://github.com/mrlt8/docker-wyze-bridge/wiki/HomeKit-Secure-Video)
* [WebUI API](https://github.com/mrlt8/docker-wyze-bridge/wiki/WebUI-API)


### Special Characters

If your email or password contains a `%` or `$` character, you may need to escape them with an extra character. e.g., `pa$$word` should be entered as `pa$$$$word`

## Web-UI

The bridge features a basic Web-UI which can display a preview of all your cameras as well as direct links to all the video streams.

The web-ui can be accessed on the default port `5000`:

```text
http://localhost:5000/
```

See also: 
* [WebUI page](https://github.com/mrlt8/docker-wyze-bridge/wiki/WebUI)
* [WebUI API page](https://github.com/mrlt8/docker-wyze-bridge/wiki/WebUI-API)


## WebRTC

WebRTC should work automatically in Home Assistant mode, however, some additional configuration is required to get WebRTC working in the standard docker mode.

- WebRTC requires two additional ports to be configured in docker:
  ```yaml
    ports:
      ...
      - 8889:8889 #WebRTC
      - 8189:8189/udp # WebRTC/ICE
    ```

- In addition, the `WB_IP` env needs to be set with the IP address of the server running the bridge.
  ```yaml
    environment:
      - WB_IP=192.168.1.116
  ```
- See [documentation](https://github.com/aler9/rtsp-simple-server#usage-inside-a-container-or-behind-a-nat) for additional information/options.

## Advanced Options

**WYZE_EMAIL** and **WYZE_PASSWORD** are the only two required environment variables.

* [Audio](https://github.com/mrlt8/docker-wyze-bridge/wiki/Camera-Audio)
* [Bitrate and Resolution](https://github.com/mrlt8/docker-wyze-bridge/wiki/Camera-Bitrate-and-Resolution)
* [Camera Substreams](https://github.com/mrlt8/docker-wyze-bridge/wiki/Camera-Substreams)
* [MQTT Configuration](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#mqtt-config)
* [Filtering Cameras](https://github.com/mrlt8/docker-wyze-bridge/wiki/Camera-Filtering)
* [Doorbell/Camera Rotation](https://github.com/mrlt8/docker-wyze-bridge/wiki/Doorbell-and-Camera-Rotation)
* [Custom FFmpeg Commands](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#custom-ffmpeg-commands)
* [Interval Snapshots](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#snapshotstill-images)
* [Stream Recording and Livestreaming](https://github.com/mrlt8/docker-wyze-bridge/wiki/Stream-Recording-and-Livestreaming)
* [rtsp-simple-server/MediaMTX Config](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#mediamtx)
* [Offline/IFTTT Webhook](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#offline-camera-ifttt-webhook)
* [Proxy Stream from RTSP Firmware](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#proxy-stream-from-rtsp-firmware)
* [BOA HTTP Server/Motion Alerts](https://github.com/mrlt8/docker-wyze-bridge/wiki/Boa-HTTP-Server)
* [Debugging Options](https://github.com/mrlt8/docker-wyze-bridge/wiki/Advanced-Option#debugging-options)
