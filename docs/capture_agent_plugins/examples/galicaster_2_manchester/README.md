Peakaboo Plugins for Galicaster
===================================

Two plugins that enables two-way communication between An Opencast compatible capture agent running Galicaster and a Peakaboo instance. This plugin leverages the Distributed Data Protocol (DDP) to communicate with the Meteor framework. The Audiostream plugin sets up a small audio server on the capture agent using gstreamer.

Things to note
--------------
* See https://github.com/hharnisc/python-ddp on more information for developing using ddp
* When using SSL with peakaboo the websocket url must use 'wss' rather than 'ws'
* When using a self signed SSL certificate on peakaboo images will not be Posted
* This version of the peakaboo plugin does not have `audiofaders` due to compatibility issues with how sound cards have various `alsa` audio settings. Please look at the sussex plugin if you want to implement audio faders.
* Figuring out the alsa/pulse audio sources can be difficult depending on your audio hardware


# Requirements

With the same user that runs Galicaster (Normally: "galicaster"), you need to install these dependencies:

* python-Meteor
* pyscreenshot
* image
* requests

```
$ pip install python-meteor pyscreenshot pillow requests
```

# ddp.py
Loading
-------

To activate the plugin, add the line in the `plugins` section of your configuration file

    [plugins]
    ddp = True

True: Enables plugin.
False: Disables plugin.

Plugin Options
--------------

    [ddp]
    meteor = ws://localhost/websocket
    room_name = local-room
    cam_available = 0
    cam_labels = local-camera-1,local-camera-2
    user = galicaster@example.com
    password = galicaster
    http_host = http://localhost
    take_screenshot = True
    hq_snapshot = False
    existing_stream_host =
    existing_stream_port =
    existing_stream_key =
    extra_params =
    existing_screenshot =
    token =


| Option               | Type    | Parameter                 | Description                                                                          |
|----------------------|---------|---------------------------|--------------------------------------------------------------------------------------|
| meteor               | string  | [ws, wss]://uri/websocket | the meteor ddp websocket interface (wss when using SSL)                              |
| room_name            | string  | any                       | the room name                                                                        |
| cam_available        | integer | 0                         | if cameras feeds are available in the room                                           |
| cam_labels           | string  | list: string,string,...   | list of camera names                                                                 |
| user                 | string  | any                       | username for sending image POST requests to peakaboo                                 |
| password             | string  | any                       | password for sending image POST requests to peakaboo                                 |
| http_host            | string  | [http, https]://uri       | URL for sending image POST requests to peakaboo                                      |
| take_screenshot      | boolean | True, False               | Have the plugin take the screenshot                                                  |
| hq_snapshot          | boolean | True,False                | Use a high quality screenshot                                                        |
| existing_stream_host | string  |                           | The host for an existing audio stream service e.g. icecast server                    |
| existing_stream_port | integer |                           | The port for an existing audio stream service e.g. icecast server                    |
| existing_stream_key  | string  |                           | The URI for an existing audio stream service e.g. icecast server                     |
| extra_params         | string  | list:of;extra:parameters  | Extra params you want to send to peakaboo that will make it into the database to use |
| existing_screenshot  | boolean |                           | Path to an existing screenshot on the filesystem /path/to/screenshot.jpg             |
| token                | string  |                           | The token that will be obtained automatically from peakaboo                          |

# audiostream.py
Loading
-------

To activate the plugin, add the line in the `plugins` section of your configuration file


    [plugins]
    audiostream = True

True: Enables plugin.
False: Disables plugin.

Plugin Options
--------------

    [audiostream]
    port = 31337
    src = alsasrc
    device =

| Option | Type    | Parameter           | Description                                                                                                                                                                                            |
|--------|---------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| port   | integer | 31337               | The port to set up the audio stream from. Can be any non-secure port                                                                                                                                   |
| src    | string  | [alsasrc, pulsesrc] | The audio source to select from, alsasrc by default but can also use pulsesrc                                                                                                                          |
| device | string  | any                 | This is the device you will stream from the `src` parameter. you may not need this if using alsasrc alone. `pactl list sources` and `arecord -l` may help if you need to specify a custom audio source |
