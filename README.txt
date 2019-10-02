pts_viddl: best-quality video downloader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
pts_viddl is a command-line tool written in Python 2.x for downloading video
files. It calls youtube-dl to do the work, but has improved best-quality
detection (i.e. better than `youtube-dl -f bestvideo+bestaudio/best'), has
better automatic output file naming, and can skip already downloaded videos.

pts_viddl has much fewer features than youtube-dl (i.e. no command-line
flags), and it supports only a few video hosting sites (e.g. youtube.com
and facebook.com).

pts_viddl is free software, GNU GPL >=2.0. There is NO WARRANTY. Use at your
own risk.

To use pts_viddl, you have to install youtube-dl first. Get it from
https://youtube-dl.org/ .

How best-quality detection works in pts_viddl:

* It runs `youtube-dl -F' to query the available audio and video formats.
* It ignores videos with larger than Full HD resolution.
* It considers these videos: mp4 (video-only and multiplexed) and non-mp4
  non-webm multiplexed. It chooses the largest sorted by
  (width * height, int(is_video_only), rate).
* If it has chosen video-only mp4, then it adds the highest-rate audio-only
  m4a format.
* From the above it follows that if video-only mp4 and multiplexed mp4 have the
  same resolution, then it chooses video-only mp4, because that tends to get
  higher-quality audio. Unfortunately it's not possible to query the video rate
  of multiplexed.
* It never chooses webm, because `youtube-dl -F' is lying about the
  resolution of `-f 43', it has 640x360 hardcoded.

How `youtube-dl -f bestvideo+bestaudio/best' can break:

* youtube-dl may choose webm audio with mp4 video, and multiplex it to an
  .mkv file. Most video players can't play such a file. The correct behavior
  would be only chosing compatible codecs for bestvideo+bestaudio. pts_viddl
  fixes it by pairing mp4 video with m4a audio.

  See also https://github.com/rg3/youtube-dl/issues/13176 and
  https://github.com/ytdl-org/youtube-dl/issues/12229 .

* youtube-dl can't detect video resolution for `-f 43' webm video on
  youtube.com, and it has 640x360 hardcoded.

  See also https://github.com/ytdl-org/youtube-dl/issues/14125 .

__END__
