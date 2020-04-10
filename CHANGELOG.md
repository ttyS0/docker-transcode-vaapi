## 0.3.0-2 (2020-04-09)

* bumped `MPV` to v0.32.0

## 0.3.0 (2020-02-27)

* switched to using [Docker Hub for publishing](https://hub.docker.com/r/ttys0/other-transcode-vaapi)
* bumped `other-transcode` to [v0.3.0](https://github.com/donmelton/other_video_transcoding/releases/tag/0.3.0)
* bumped `FDKAAC` to v2.0.1
* bumped `X264` to v20191217-2245-stable
* bumped `X265` to v3.2.1

---

## [0.2.0 (2020-01-14)](https://github.com/ttyS0/docker-transcode-vaapi/packages/104690)

* started with a copy of [jrottenberg/ffmpeg/docker-images/4.2/vaapi/Dockerfile](https://github.com/jrottenberg/ffmpeg/blob/master/docker-images/4.2/vaapi/Dockerfile)
* removed various FFmpeg compile targets, such as X11 components
* added `mpv`, `mkvpropedit`, and `other-transcode`
* changed the `ENTRYPOINT` to be `other-transcode`

