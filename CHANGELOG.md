## [0.2.0 (2020-01-14)](https://github.com/ttyS0/docker-transcode-vaapi/packages/104690)
---

* started with a copy of [jrottenberg/ffmpeg/docker-images/4.2/vaapi/Dockerfile](https://github.com/jrottenberg/ffmpeg/blob/master/docker-images/4.2/vaapi/Dockerfile)
* removed various FFmpeg compile targets, such as X11 components
* added `mpv`, `mkvpropedit`, and `other-transcode`
* changed the `ENTRYPOINT` to be `other-transcode`


