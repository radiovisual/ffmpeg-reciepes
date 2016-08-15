# ffmpeg-reciepes
> A list of my commonly-used ffmpeg scripts (MacOS)

Below are some of my commonly-used [ffmpeg](https://ffmpeg.org/) snippets used on MacOS. I won't be porting these over to work on Windows, but I will accept a Pull Request for anyone who would like to add Windows support.

**Note:** ffmpeg must be on your `$PATH` for the snippets below to work, otherwise, simply replace each instance of `ffmpeg` with the actual location to the ffmpeg executable, for example: `/Applications/FFMPEG/ffmpeg`

:boom: **DISCLAIMER: Use these snippets at your own risk!** Some of these operations can be destructive, so be sure to have backups of your files before you start running random commands. If any of the commands, presets, parameters or codecs are unclear, then please reference the [ffmpeg documentation](https://ffmpeg.org/documentation.html). I can't offer any promises or guaruntees that these snippets will work the way you intend them to, so use them at your own peril.

## Splice a Video Clip

In the example below, splice a movie clip starting from 15 seconds, and the following 60 seconds
```
ffmpeg -i input.flv -ss 15 -t 60 -acodec copy -vcodec copy output.flv
```

## Strip (Remove) Audio Track from Video

**Single Clip**
```
ffmpeg -i input.mov -an -vcodec noaudio/output.mov
```

**Batch**
```
for file in *.mp4; do ffmpeg -i "$file" -an -vcodec copy noaudio/"$file"; done
```

## Extract Audio from Video

In the examples below, the audio track from `input.mp4` will be saved as `output.mp3`

**Single Clip**
```
ffmpeg -i input.mp4 -ab 160k -ac 2 -ar 44100 -vn output.mp3
```

**Batch**
Target all `*.mp4` in the current directory
```
for file in *.mp4; do ffmpeg -i "$file" -ab 160k -ac 2 -ar 44100 -vn "$file".mp3; done
```

## Convert to WMV
```
ffmpeg -i input.mp4 -qscale 2 -vcodec msmpeg4 -acodec wmav2 output.wmv
```

## Convert FLV to MP4 (and Preserve Quality)
```
for file in *.mov; do /Applications/FFMPEG/ffmpeg -i "$file" -qscale 0 -ar 22050 -vcodec libx264 "$file".mp4; done
```

## Convert AVI to MP4

Use [Handbrake](https://handbrake.fr/) for this.

## Compress to MP4

With audio
```
for file in *.mov; do ffmpeg -i "$file" -vcodec h264 -acodec aac -strict -2 "$file".mp4; done
```
Without audio
```
for file in *.mov; do ffmpeg -i "$file" -acodec mp2 "$file".mp4; done
```

