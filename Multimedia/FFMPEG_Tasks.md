# FFMPEG - download from official site or it is already included in various free tools  

## Convert VideoFileTypes without Re-Encoding  

### convert mkv to mp4 with FFMPEG without Re-encoding  

ffmpeg -i input.mp4 -vcodec copy -acodec copy output.mkv  
Example:  
ffmpeg.exe -i 'H:\Ice.Age.3.Dawn.of.the.Dinosaurs.2009.720p.mkv' -vcodec copy -acodec copy H:\IceAge3_DawnoftheDinosaurs.2009.720p.mp4  
