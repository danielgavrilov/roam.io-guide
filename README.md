Contents
========

1. [Mac mini setup](#mac-mini-setup)
2. [Installing the software](#installing-the-software)
3. [Wiring](#wiring)
4. [Starting Enquir.io](#starting-enquirio)
5. [Editing questions](#editing-questions)



Mac mini setup
==============

These need to be done **only once** for a fresh Mac mini setup.

#### Auto login on startup (skip password prompt)

Go to **System Preferences** → **Users & Groups** → **Login Options**. Set **Automatic Login** to the default user.

#### Disable iTunes Play

Instructions [here](http://www.thebitguru.com/projects/iTunesPatch).

#### Remove Chrome toolbar in full screen

Open Chrome, then **View** → **Always Show Toolbar in Fullscreen**.

#### Remap backspace

Install [Karabiner](https://pqrs.org/osx/karabiner/). Edit the custom XML to:

```xml
<?xml version="1.0"?>
<root>
  <item>
    <name>Use ] as Backspace</name>
    <identifier>private.use_bracket_right_as_backspace</identifier>
    <autogen>__KeyToKey__ KeyCode::BRACKET_RIGHT, KeyCode::DELETE</autogen>
  </item>
</root>
```

Then reload the custom XML. This will then add a `Use ] as Backspace` option in the list, which you need to enable.

#### Disable screen brightness and volume up/down overlays

[Turn off bezels](http://apple.stackexchange.com/a/212694) indefinitely for this user:

```
launchctl unload -wF /System/Library/LaunchAgents/com.apple.BezelUI.plist
```

#### Auto-hide mouse cursor

Install [Cursorcerer](http://doomlaser.com/cursorcerer-hide-your-cursor-at-will/) and configure it to automatically hide cursor after a few seconds of no movement.

#### Disable notifications

Disable banners for each application (System Preferences → Notifications, set "mini alert style" to `None` for each) and turn on **Do Not Disturb** mode from `00:00` to `23:59`.

#### Display resolution

Resolution needs to be **1024x768** on both displays (to set resolution System Preferences → Displays). If this specific resolution not shown in the list, **option click** on "Scaled", which should show more resolutions in the list.



Installing the software
=======================

This needs to be done **only once** for a fresh setup.

1. Install [`node-pixel` Firmata](https://github.com/ajfisher/node-pixel/blob/master/firmware/build/node_pixel_firmata/node_pixel_firmata.ino) on the Arduino, using the Arduino IDE.

1. Install [brew](http://brew.sh) on the Mac.

1. Install [Node.js](https://nodejs.org) on the host computer. Preferably using:

  ```
  brew install node
  ```

1. Install MongoDB as a service

  ```
  brew install mongodb
  brew services start mongodb
  ```

1. Clone this repo

  ```
  git clone https://github.com/danielgavrilov/roam.io
  ```

1. `cd` to repo folder to install all Node.js & Bower dependencies:

  ```
  npm install
  ```

1. Make program auto-start on login

  Go to **System Preferences** → **Users & Groups** → **Login Items**, press `+` icon and add `scripts/boot.app` to the list, then enable it by ticking the checkbox.



Wiring
======

#### Arduino

There is a [shield diagram](https://docs.google.com/spreadsheets/d/1edrtmq1ul43iYVfsFOV3o18ER8bILbH1SxsYPFVHQ8U/) in the Google Drive folder showing the pins connected to the Arduino and the connectors coming out of it.

#### Display ports

These need to be in the right order, but there are no labels. Connect randomly then swap if the monitors are upside down.



Starting Enquir.io
==================

The software for Enquir.io should auto-start when the Mac mini is turned on. Below are some things that might go wrong.

#### Nothing happens

If Google Chrome does not open at all, the program probably terminated and the error message is at the end of `err.log`. Try to fix it or contact me. Then restart by running:

```
npm stop; npm start
```

#### Chrome windows incorrectly set up

If the Chrome windows are not full screen, or one is and the other isn't, or something similar, then use a mouse and keyboard to set them up manually. Drag each window to the right screen and enter/exit full screen with `⌘+CTRL+F`.

If they _often_ fail to automatically set up, make sure the screens are side-by-side in **System Preferences** → **Displays** → **Arrangement**. Drag to rearrange until they snap side by side.

#### Answers do not match panel cutouts, or text too big/small

The resolution is likely incorrect, both monitors need to be **1024x768**. Set resolution in **System Preferences** → **Displays**. If 1024x768 does not appear in the list, **option click** on **Scaled** and it should appear.

#### Displays are upside down

Swap the display ports at the back of the Mac mini.

#### Eyes not in sync

If Enquir.io's eyes point in different directions (appearing dizzy), run:

```
node scripts/eye-offset
```

Use the arrow keys to move the white light to the topmost pixel. Then copy the offsets into `app/arduino/logic/setup.js`.

#### Not logging to flash drive

I disabled logging to flash drive after the Madeira deployment. Sometimes you don't have permissions to write to flash drive, so you need to start the script as superuser and fiddle with the config (let me know if you want me to write how to do this).



Editing questions
=================

All questions are in the `questions` directory. Each question is a separate file. The contents of the files is written in [YAML](https://en.wikipedia.org/wiki/YAML#Basic_components) and the fields are explained in the template question in `questions/template.yml`.

The word before the first `-` (dash) in the filename specify the category of the question, e.g. the filename `factual-the-airport-is-named-after` belongs to the **factual** category.

The whole **filename** of the question is its **unique identifier**, and can be used to identify the question in the logs.

When editing more than typos, change the filename if you want to make it distinct in the logs. Note that this will also reset the response counts in the visualisation.
