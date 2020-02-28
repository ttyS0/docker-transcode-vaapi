# docker-transcode-vaapi

## About


[Don Melton](http://donmelton.com/) of [Video Transcoding](https://github.com/donmelton/video_transcoding) renown
has brought hardware acceleration to bear in the form of [Other Video Transcoding](https://github.com/donmelton/other_video_transcoding). While Don and his troupe of video transcoding flagellants have well
covered the installation and usage of `other-transcode` for all normal desktop platforms, Linux remains a bit
of a challenge thanks to a diaspora of package management and custom software compilation. My approach to the
problem is to wrap up `other-transcode` and its accouterments into a Docker container.

I have leaned _heavily_ on [Julien Rottenberg's](https://github.com/jrottenberg) [ffmpeg](https://github.com/jrottenberg/ffmpeg) Dockerfiles. Basically I've taken them, pulled out some of the extraneous `ffmpeg`
compile options, added in the `other-transcode` dependencies, and finally bundled in `other-transcode` itself.


Because hardware acceleration is, well, hardware dependent, I again stole an idea from [Julien Rottenberg's](https://github.com/jrottenberg)
repository, and narrowed the scope of each container to a specific hardware target. This container is for Intel
GPUs, and leverages [VAAPI](https://en.wikipedia.org/wiki/Video_Acceleration_API). Other hardware support, such
as `NVidia`, will be done in separate repositories.

Since this container compiles `FFmpeg`, its dependent libraries, and `mpv`, building can take a while to complete.
The image itself weighs in at ~200MB. While it can be built on any platform, the image is only intended to be run
on bare metal Linux systems. Virtualized Linux systems further complicate the direct hardware access that
`other-transcode` relies on, and are not supported. I am using GitHub's `package` facility, so you can
pull an already built image directly from [here](https://github.com/ttyS0/docker-transcode-vaapi/packages/104690).

The container image version is hard aligned to `other-transcode`, with numerical suffixes representing container
modifications not directly related to `other-transcode` such as changes in `FFmpeg` compile options, for example.


## Usage

In its current form, this container is intended to be used to directly run `other-transcode` as a container.
As such, the host's `/dev/dri` must be passed in, as well as the current directory bind mounted. In order to
keep `other-transcode` from attempting to overwrite the source file that's being transcoded, it's suggested
that the current working directory be considered the `output` directory, and source file(s) be placed in a
child directory. In short something like this:

```
output_dir
\_source_dir
  \_source_file.mkv
```

where `output_dir` is `$(pwd)`. Using that context, to transcode `source_file.mkv`:

```
cd output_dir
docker run --device /dev/dri:/dev/dri -v $(pwd):$(pwd) -w $(pwd) \ 
  ttys0/other-transcode-vaapi \
  source_dir/source_file.mkv
```

If you want to pass `other-transcode` options, do so just as if `other-transcode` was being run outside of the
container. For example, to target a bitrate of 2000 the above example would look like this:

```
cd output_dir
docker run --device /dev/dri:/dev/dri -v $(pwd):$(pwd) -w $(pwd) \
  ttys0/other-transcode-vaapi \
  --target=2000 source_dir/source_file.mkv
```

## Potential Problems

* As I try to shave down the required FFmpeg compile options to only those that get used in video transcoding, I  may inadvertently remove some that are actually needed. If you find a codec missing that is "normally" available with `FFmpeg` please make an issue, and I'll add it back in.
