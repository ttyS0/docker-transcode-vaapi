# docker-transcode-vaapi

# THIS REPOSITORY HAS MOVED!!!!
## Please update links to point to https://github.com/ttyS0/docker-other-transcode

## About


[Don Melton](http://donmelton.com/) of [Video Transcoding](https://github.com/donmelton/video_transcoding) fame has brought his expertise to hardware accelerated transcoding in the form of [Other Video Transcoding](https://github.com/donmelton/other_video_transcoding). While Don covers the installation and usage of `other-transcode` for all normal desktop platforms, this repository's focus is using `other-transcode` as a Docker container.

I have leaned _heavily_ on [Julien Rottenberg's](https://github.com/jrottenberg) [ffmpeg](https://github.com/jrottenberg/ffmpeg) Dockerfiles. Basically I've taken them, pulled out some of the extraneous `ffmpeg` compile options, added in the `other-transcode` dependencies, and finally bundled in `other-transcode` itself.

Hardware acceleration being hardware dependent, I again stole an idea from [Julien Rottenberg's](https://github.com/jrottenberg) repositories, and narrowed the scope of each container to a specific hardware target. *This* container is for **Intel** GPUs, and uses [VAAPI](https://en.wikipedia.org/wiki/Video_Acceleration_API).

The container image version is hard aligned to `other-transcode`, with numerical suffixes representing container modifications not directly related to `other-transcode` such as changes in `FFmpeg` compile options.

## Requirements

* An Intel GPU. Kaby Lake or better is recommended.

* Exclusive use of the `/dev/dri` device.

## Usage

In its current form, this container is setup to directly run `other-transcode`. In order to keep `other-transcode` from attempting to overwrite the source file that's being transcoded, the current working directory is considered the `output` directory, and source file(s) are placed in a child directory. In short something like this:

```
working_dir
\_src
  \_source_file.mkv
```

where `working_dir` is `$(pwd)`.

Using that setup, to transcode `source_file.mkv` with the default H264 encoding, the Docker command looks like this when run from `working_dir`:

```
docker run --rm --device /dev/dri:/dev/dri -v $(pwd):$(pwd) -w $(pwd) \ 
  ttys0/other-transcode-vaapi \
  src/source_file.mkv
```

If you want to pass `other-transcode` options, do so just as if `other-transcode` was being run outside of the container.

For example, to change the target bitrate to 6000Kbs the Docker command would be:

```
docker run --rm --device /dev/dri:/dev/dri -v $(pwd):$(pwd) -w $(pwd) \
  ttys0/other-transcode-vaapi \
  --target 6000 src/source_file.mkv
```

## Gotchas

* **HEVC** is _NOT_ well supported by VAAPI, and _SHOULD BE AVOIDED_ when using this container!! If you _really_ want to transcode with HEVC on a Linux system your best bet is to use an NVidia card. In that case, the [NVidia other-transcode container](https://github.com/ttyS0/docker-transcode-nvidia) is a good option.

* As I try to shave down the required FFmpeg compile options to only those that get used in video transcoding, I  may inadvertently remove some that are actually needed. If you find a codec missing that is "normally" available with `FFmpeg` please make an issue, and I'll add it back in.

* This has only been tested with Ubuntu 18.04.
