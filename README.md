This is a fork of mpv-video-splice with an ability to edit online videos from platforms
that youtube-dl (or yt-dlp) support and other improvements and features.

# mpv-yt-splice
An mpv player script that allows you create a video out of cuts made in the
current playing video (local or online). 

**Requires: ffmpeg**
**Requires: yt-dlp or youtube-dl** (optional)


## Description
This script provides the ability to create video slices by grabbing two
timestamps, which generate a slice from timestamp A to timestamp B,
e.g.:
	
	-> 1: 00:10:34.25 -> 00:15:00.00
	-> 2: 00:23:00.84 -> 00:24:10.00
	...
	-> n: 01:44:22.47 -> in progress..
	

Then, all the slices from 1 to n are joined together, creating a new
video.

**The output file will appear at the directory that the mpv command was ran
or at the directory specified in output_location option.**

If **re-encoding** is enabled, the video will be encoded.
Only applies to local files. By default, the script copies the video codec
(do_encode == "no") which makes cutting near instant but imprecise, because the
cut has to start at a keyframe. Toggling re-encoding slows down the compilation
process but allows for frame-perfect cuts.

If **uploading** is enabled, the video will be saved and uploaded to the configured
destination and the link to the uploaded video copied to clipboard.

**Note:** This script prevents the mpv player from closing when the video ends,
so that the slices don't get lost. Keep this in mind if there's the option
`keep-open=no` in the current config file.

**Note:** This script will also silence the terminal, so the script messages
can be seen more clearly.


## Usage
This section correspond to the shortcut keys provided by this script.

### Alt + e (Toggle re-encoding)
For local videos, toggling re-encoding on will slow down the compilation
process and allows for for perfectly precise cuts.

### Alt + u (Toggle upload)
Toggle uploading the edited video to configured destination.
The default in streamable, supply your credentials in the config for it to work.

### Alt + t (Grab timestamp)
In the video screen, press `Alt + T` to grab the first timestamp and then
press `Alt + T` again to get the second timestamp. This process will generate
a time range, which represents a video slice. Repeat this process to create
more slices.

### Alt + p (Print slices)
To see all the slices made, press `Alt + P`. All of the slices will appear
in the terminal in order of creation and on screen,
with their corresponding timestamps.
Incomplete slices will show up as `N: timestamp -> in progress...`.

### Alt + r (Reset unfinished slice)
To reset an incomplete slice, press `Alt + R`. If the first part of a slice
was created at the wrong time, this will reset the current slice.

### Alt + d (Delete slice)
To delete a whole slice, start the slice deletion mode by pressing `Alt + D`.
When in this mode, it's possible to press `Alt + NUM`, where `NUM` is any
number between 0 inclusive and 9 inclusive. For each `Alt + NUM` pressed, a
number will be concatenated to make the final number referring to the slice 
to be removed, then press `Alt + D` again to stop the slicing deletion mode
and delete the slice corresponding to the formed number.

Example 1: Deleting slice number 3
* `Alt + D`	# Start slice deletion mode
* `Alt + 3`	# Concatenate number 3
* `Alt + D`	# Exit slice deletion mode

Example 2: Deleting slice number 76
* `Alt + D`	# Start slice deletion mode
* `Alt + 7`	# Concatenate number 7
* `Alt + 6`	# Concatenate number 6
* `Alt + D`	# Exit slice deletion mode

### Alt + c (Compiling final video)
To fire up ffmpeg, which will slice up the video and concatenate the slices
together, press `Alt + C`. It's important that there are at least one
slice, otherwise no video will be created.

**Note:** No cut will be made unless the user presses `Alt + C`.
Also, the original video file **won't** be affected by the cutting.

## Log Level
Everytime a timestamp is grabbed, a text will appear on the screen showing
the selected time.
When `Alt + P` is pressed, besides showing the slices in the terminal, 
it will also show on the screen the total number of cuts (or slices)
that were made.
When the actual cutting and joining process begins, a message will be shown
on the screen and the terminal telling that it began. When the process ends,
a message will appear on the screen and the terminal displaying the full path
of the generated video. It will also appear a message in the terminal telling
that the process ended.

**Note:** Every message that appears on the terminal has the **log level of 'info'**.

## Environment Variables
This script uses environment variables to allow the user to
set the temporary location of the video cuts and for setting the location for
the resulting video.

To set the temporary directory, set the variable `MPV_SPLICE_TEMP`;

e.g.: `export MPV_SPLICE_TEMP="$HOME/temporary_location"`

To set the video output directory, set the variable `MPV_SPLICE_OUTPUT`;

e.g.: `export MPV_SPLICE_OUTPUT="$HOME/output_location"`

**Make sure the directories set in the variables really exist, or else the
script might fail.**

## Configuration
The script can be configured using the mpv-splice.conf file.
See the file comments for explanation of the configuration options.


# Installation

To install this script, simply add it to your script folder, located at
`$HOME/.config/mpv/scripts`

When the mpv player gets started up, the script will be executed and will be ready to use.
