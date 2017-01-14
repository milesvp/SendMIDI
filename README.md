# SendMIDI

SendMIDI is a multi-platform command-line tool makes it very easy to quickly send MIDI messages to MIDI devices from your computer.

All the heavy lifting is done by the wonderful JUCE library.

The project website is https://github.com/gbevin/SendMIDI

## Purpose
This tool is mainly intended for configuration or setup through Continuous Control, RPN and NRPN messages, but many other MIDI messages can be sent

## Download

You can download pre-built binaries from the release section:
https://github.com/gbevin/SendMIDI/releases

Since SendMIDI is free and open-source, you can also easily build it yourself. Just take a look into the Builds directory when you download the sources.

## Usage
To use it, simply type "sendmidi" or "sendmidi.exe" on the command line and follow it with a series of commands that you want to execute. These commands have purposefully been chosen to be concise and easy to remember, so that it's extremely fast and intuitive to quickly shoot out a few MIDI messages.

These are all the supported commands:
```
  dev   name           Set the name of the MIDI output port (REQUIRED)
  list                 Lists the MIDI output ports
  panic                Sends all possible Note Offs and relevant panic CCs
  file  path           Loads commands from the specified program file
  ch    number         Set MIDI channel for the commands (1-16), defaults to 1
  on    note velocity  Send Note On with note (0-127) and velocity (0-127)
  off   note velocity  Send Note Off with note (0-127) and velocity (0-127)
  pp    note value     Send Poly Pressure with note (0-127) and pressure (0-127)
  cc    number value   Send Continuous Controller (0-127) with value (0-127)
  pc    number         Send Program Change number (0-127)
  cp    value          Send Channel Pressure value (0-127)
  pb    value          Send Pitch Bend value (0-16383)
  rpn   number value   Send RPN number (0-16383) with value (0-16383)
  nrpn  number value   Send NRPN number (0-16383) with value (0-16383)
  start                Start the current sequence playing
  stop                 Stop the current sequence
  cont                 Continue the current sequence
  clock bpm            Send 2 beats of MIDI Timing Clock for a BPM (1-999)
  spp   beats          Send Song Position Pointer with beat (0-16383)
  ss    number         Send Song Select with song number (0-127)
  --                   Read commands from standard input until it's closed
```

Alternatively, you can use the following long versions of the commands:
```
  device channel note-on note-off poly-pressure continuous-controller
  program-change channel-pressure pitch-bend continue song-position song-select
```

The MIDI device name doesn't have to be an exact match.
If SendMIDI can't find the exact name that was specified, it will pick the first MIDI output port that contains the provided text, irrespective of case.

## Examples
  
Here are a few examples to get you started:

List all the available MIDI output ports on your system

```
sendmidi list
```

Switch the LinnStrument to User Firmware Mode by setting NRPN 245 to the value 1:

```
sendmidi dev "LinnStrument MIDI" nrpn 245 1
```

Light up LinnStrument column 5 on row 0 in red by setting CCs 20, 21, and 22 to the column, row and color:
  
```
sendmidi dev "LinnStrument MIDI" cc 20 5 cc 21 0 cc 22 1
```

Load the commands from a text file on your system and execute them, afterwards switch to the "Network Session 1" port and send it program change number 10:
  
```
sendmidi file path/to/some/text/file dev "Network Session 1" pc 10
```

## Text File Format

The text file that can be read through the "file" command can contain a list of commands and options, just like when you would have written them manually on the console (without the "sendmidi" executable). You can insert new lines instead of spaces and any line that starts with a dash (#) character is a comment.

For instance, this is a text file for one of the examples above:
```
dev "LinnStrument MIDI"
# set column 5 on row 0 to the red color
cc 20 5
cc 21 0
cc 22 1
```
