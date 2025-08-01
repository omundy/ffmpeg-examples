# ffmpeg-examples




## Install FFmpeg

```bash
brew install ffmpeg
```




## Convert audio files

```bash

# convert M4A to WAV
ffmpeg -i input.m4a output.wav

# convert M4A to MP3 ([using variable bit rate](https://superuser.com/a/704535/132422) `-q:a 4`)
ffmpeg -i input.m4a -c:v copy -c:a libmp3lame -q:a 4 output.mp3
```

## Convert video files

```bash
# convert a video container (format) from FLV to MP4, leaving the codec the same
ffmpeg -i input.flv -codec copy output.mp4

# convert video codec to H.264 and bitrate using Constant Rate Factor (lowers the average bit rate, but retains better quality. Vary the CRF between around 18 and 24 — the lower, the higher the bitrate)
ffmpeg -i input.mp4 -vcodec libx265 -crf 20 output.mp4

# convert video bitrate using CRF
ffmpeg -i input.mp4 -crf 20 output.mp4

# convert MOV to MP4
ffmpeg -i input.mov -vcodec h264 -acodec aac output.mp4
# same as ^ + strictly following aac codec (experimental https://stackoverflow.com/a/35247468/441878 )
ffmpeg -i input.mov -vcodec h264 -acodec aac -strict -2 output.mp4

# convert MOV to WEBM
ffmpeg -i input.mov -vcodec libvpx -qmin 0 -qmax 50 -crf 10 -b:v 1M -acodec libvorbis output.webm
```


## Crop a video

[Examples](https://video.stackexchange.com/a/4571/25602)

```bash
# filter automatically centers the crop if x and y are omitted
# ffmpeg -i input.mp4 -filter:v "crop=out_w:out_h:x:y" output.mp4
# example crops from 1920x1080 to 1920x886
ffmpeg -i input-1920x1080.mp4 -filter:v "crop=1920:886" output-1920x886.mp4
```


## Convert multiple files

```bash
# list files found in containing folders 
for i in ./**/*.m4a; do echo "$i"; done

# for each PATH in containing folders; store and create new DIR_NAME w/o the preceding "./"; and file name, replacing the extension w/mp3; convert to mp3
for PATH in ./**/*.m4a; \
  do \
    DIR_NAME="$(dirname "../music-drive-converted/${PATH:2}")"; \
    mkdir -p "$DIR_NAME";
    FILE_NAME=$(basename "$PATH"); \
    NEW_PATH="$DIR_NAME/${FILE_NAME/m4a/mp3}";  \
    ffmpeg -i "$FILE" -codec:v copy -codec:a libmp3lame -q:a 4 "$NEW_PATH";  \
done


# convert multiple audio files
for i in *.m4a; do ffmpeg -i "$i" "${i%.*}.wav"; done

# convert the containers (format) for all videos in a directory from FLV to MP4, leaving the codec the same
for i in *.flv; do ffmpeg -i "$i" -codec copy  "${i%.*}.mp4"; done

# convert all MP4s in a directory using CRF and copy to new directory
for i in *.mp4; do ffmpeg -y -i "$i" -crf 20  "../new/${i%.*}.mp4"; done

```



## References

1. [Encoding Video for the web](https://gist.github.com/Vestride/278e13915894821e1d6f)
