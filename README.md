# ffmpeg-examples




## Installation

```
# install ffmpeg
brew install ffmpeg

```


### Convert a file

```

# convert a video container (format) from FLV to MP4, leaving the codec the same
ffmpeg -i input.flv -codec copy output.mp4

# convert video codec to H.264 and bitrate using Constant Rate Factor (lowers the average bit rate, but retains better quality. Vary the CRF between around 18 and 24 — the lower, the higher the bitrate)
ffmpeg -i input.mp4 -vcodec libx265 -crf 20 output.mp4

# convert video bitrate using CRF
ffmpeg -i input.mp4 -crf 20 output.mp4



# convert MOV to MP4
ffmpeg -i input.mov -vcodec h264 -acodec aac -strict -2 output.mp4

# convert MOV to WEBM
ffmpeg -i input.mov -vcodec libvpx -qmin 0 -qmax 50 -crf 10 -b:v 1M -acodec libvorbis output.webm

```


## Convert multiple files

```

# convert the containers (format) for all videos in a directory from FLV to MP4, leaving the codec the same
for i in *.flv; do ffmpeg -i "$i" -codec copy  "${i%.*}.mp4"; done

# convert all MP4s in a directory using CRF and copy to new directory
for i in *.mp4; do ffmpeg -y -i "$i" -crf 20  "../new/${i%.*}.mp4"; done

```



### References

- (Encoding Video for the web)[https://gist.github.com/Vestride/278e13915894821e1d6f]
