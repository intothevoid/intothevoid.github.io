---
title: "Organise Videos By Resolution"
date: 2024-05-18T21:40:39+09:30
description: "Sort video files using a quick and dirty script in Python"
tags: ["programming", "code", "python", "scripts"]
featured_image: "/images/python.png"
---

Recently I was clearing out some old movie rips that I had created from VideoCDs and DVDs many years ago. I wanted a quick and dirty way to organise these i.e. get rid of the videos of resolutions lower than 720p (1280x720)

With LLMs all the rage at the moment, all it took is a few prompts and corrections and in 15 minutes I had this script ready to go -


```python
import os
import shutil
from moviepy.editor import VideoFileClip

def analyze_and_move_folders(parent_folder):
    # Create the 'inferior' folder if it doesn't exist
    inferior_folder = os.path.join(parent_folder, 'inferior')
    if not os.path.exists(inferior_folder):
        os.makedirs(inferior_folder)

    # Walk through all subdirectories
    for root, dirs, files in os.walk(parent_folder):
        # Check if the current directory contains any video files
        video_files_found = False
        for file in files:
            if file.endswith(('.mkv', '.avi', '.mp4')):
                video_files_found = True
                break
        
        if video_files_found:
            # Load the first video file to check its resolution
            first_video_file = next((f for f in files if f.endswith(('.mkv', '.avi', '.mp4'))), None)
            if first_video_file:
                file_path = os.path.join(root, first_video_file)
                try:
                    clip = VideoFileClip(file_path)
                    
                    # Check if the video resolution is less than 720p
                    if clip.size[0] < 1280 or clip.size[1] < 720:
                        # Use shutil.move() instead of os.rename()
                        shutil.move(root, os.path.join(inferior_folder, os.path.basename(root)))
                        print(f'Moved folder {root} to inferior due to low resolution.')
                except UnicodeDecodeError as e:
                    print(f"Skipping {file_path} due to decoding error: {e}")
                    continue  # Skip this file and continue with the next one

# Specify the path to the parent folder
parent_folder = '/path/to/your/folder'
analyze_and_move_folders(parent_folder)

```

The script uses the moviepy library to analyse movie files. The script works in the following manner -

1. Scan all sub-folders within the root folder for movie files i.e. mkv, avi, and mp4 files
2. Once a movie file is encountered, analyse this file using moviepy
3. Find  out the resolution of the movie
4. If the resolution of the movie is < 1280p horizontally or < 720p vertically, move the file to a folder called 'inferior'
5. Repeat above steps until all sub-folders within the parent folder have been scanned

That's it. The script saved me time and effort from having to load each movie in a video player before deleting it.
