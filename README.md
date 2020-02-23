[![Build ffmpeg](https://github.com/oznu/ffmpeg-for-homebridge/workflows/Build%20ffmpeg/badge.svg)](https://github.com/oznu/ffmpeg-for-homebridge/actions)

# ffmpeg for homebridge

This project provides static `ffmpeg` binaries for multiple platforms and architectures for use with [Homebridge](https://github.com/nfarina/homebridge).

* Audio support using `libfdk-aac`
* Hardware decoding on the Raspberry Pi using `h264_omx`

## Supported Platforms

| OS                  | Supported Architectures |
|---------------------|-------------------------|
| Raspbian Linux      | armv6l (armv7l)         |
| Debian/Ubuntu Linux | x86_64, armv7l, aarch64 |
| Alpine Linux        | x86_64, armv6l, aarch64 |
| macOS               | x86_64                  |
| Windows             | x86_64                  |

## Install

#### Raspbian Linux:

```
curl -Lf# https://github.com/oznu/ffmpeg-for-homebridge/releases/latest/download/ffmpeg-raspbian-armv6l.tar.gz | sudo tar xzf - -C / --no-same-owner
```

#### Debian / Ubuntu Linux:

```
curl -Lf# https://github.com/oznu/ffmpeg-for-homebridge/releases/latest/download/ffmpeg-debian-$(uname -m).tar.gz | sudo tar xzf - -C / --no-same-owner
```

#### macOS:

```
curl -Lf# https://github.com/oznu/ffmpeg-for-homebridge/releases/latest/download/ffmpeg-darwin-x86_64.tar.gz | sudo tar xzfm - -C / --no-same-owner
```

#### Windows:

Download the `ffmpeg.exe` file from the [releases page](https://github.com/oznu/ffmpeg-for-homebridge/releases/latest).

## Build Flags

The `ffmpeg` binary is built with the following options enabled:

```bash
  --enable-static
  --disable-debug
  --disable-shared
  --disable-ffplay
  --disable-doc
  --enable-openssl
  --enable-gpl
  --enable-version3
  --enable-nonfree
  --enable-pthreads
  --enable-libvpx
  --enable-libmp3lame
  --enable-libopus
  --enable-libtheora
  --enable-libvorbis
  --enable-libx264
  --enable-runtime-cpudetect
  --enable-libfdk-aac
  --enable-avfilter
  --enable-libopencore_amrwb
  --enable-libopencore_amrnb
  --enable-filters
  --enable-decoder=h264
  --enable-network
  --enable-protocol=tcp
  --enable-demuxer=rtsp
  --enable-omx-rpi              # Raspbian Linux builds only
  --enable-mmal                 # Raspbian Linux builds only
  ```

## Issues

Issues related to Homebridge, any camera plugins, or your config.json, should be raised on the corresponding project page or community support forums.

Issues strictly related to the compatibility or installation of the resulting binary may be raised [here](https://github.com/oznu/ffmpeg-for-homebridge/issues).

## Use as a plugin dependency

This section is for Homebridge Plugin developers, if you need to install ffmpeg see the instructions above.

You can include this package as a dependency in your Homebridge camera plugins.

```
npm install --save ffmpeg-for-homebridge
```

```js
var pathToFfmpeg = require('ffmpeg-for-homebridge');

if (pathToFfmpeg) {
  child_process.spawn(pathToFfmpeg, [])
} else {
  // fallback, load from PATH
  child_process.spawn('ffmpeg', [])
}
```

If `ffmpeg` is not supported on the user's platform, or this package failed to download the `ffmpeg` binary, the package will return `undefined`, you should check for this and and try and use `ffmpeg` from the user's `PATH` instead.

You will need to update your plugin's README installation command to include the `--unsafe-perm` flag. For example:

```bash
# example 
sudo npm install -g --unsafe-perm homebridge-fake-camera-plugin
```

## Credits

* Linux and macOS build script: [markus-perl/ffmpeg-build-script](https://github.com/markus-perl/ffmpeg-build-script)
* Windows build script: [rdp/ffmpeg-windows-build-helpers](https://github.com/rdp/ffmpeg-windows-build-helpers)