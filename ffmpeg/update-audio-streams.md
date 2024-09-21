# README #

Updating audio tracks of video files

### Extract audio track ###

Extract audio track from video file:
```
ffmpeg -i VIDEO.mkv -q:a 0 -map a AUDIO.mp3
```

Extract audio track from video file while skipping the first 4 seconds:
```
ffmpeg -i VIDEO.mkv -ss 4 -q:a 0 -map a AUDIO.mp3
```

### Add audio track ###

Add an `AUDIO.mp3` to a `VIDEO.mkv` input and copy the updated file to `VIDEO_NEW.mkv`
```
ffmpeg -i VIDEO.mkv -i AUDIO.mp3 -map 0 -map 1 -c copy VIDEO_NEW.mkv
```