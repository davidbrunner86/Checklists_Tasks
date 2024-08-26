# FFMPEG - download from official site or it is already included in various free tools  

## Convert VideoFileTypes without Re-Encoding  

### convert mkv to mp4 with FFMPEG without Re-encoding  

ffmpeg -i input.mp4 -vcodec copy -acodec copy output.mkv  
Example:  
ffmpeg.exe -i 'H:\Ice.Age.3.Dawn.of.the.Dinosaurs.2009.720p.mkv' -vcodec copy -acodec copy H:\IceAge3_DawnoftheDinosaurs.2009.720p.mp4  


### Convert Video to x256  
ffmpeg -i input.mp4 -c:v libx265 -vtag hvc1 c:\temp\output.mp4  

to use NVENC use -vcodec hevc_nvenc  
for AMD GPU use -c:v hevc_amf  

### convert 4k to 1080 (no change in codec)  
ffmpeg -i input4kvid.mp4 -vf scale=1920:1080 -c:a copy output1080vid.mp4  

### convert h.264 to h.265 (no change in resolution)  
ffmpeg -i input.mp4 -c:v libx265 -vtag hvc1 -c:a copy output.mp4

