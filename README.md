# Griode

Griode lets you play music using a LaunchPad or a similar controller.


There is a basic (and incomplete) [user manual](MANUAL.md)


## Quick start

Here are some quick instructions to get you started, assuming that you
have a LaunchPad connected to an Debian/Ubuntu system.

```
git clone git://github.com/worikgh/griode
cd griode
sudo apt-get install python3-pip python3-dev libasound2-dev libjack-dev fluidsynth
pip3 install --user -r requirements.txt
( cd soundfonts; ./download-soundfonts.sh; )
./griode.sh
```

Your LaunchPad should light up with a red and white pattern, and pressing
pads should make piano sounds.


## Detailed setup instructions

You need:

- Python 3
- Jackd audio
- FluidSynth (to generate sounds)
- at least one SoundFont (instrument bank used by FluidSynth)
- a LaunchPad or similar MIDI controller
- ladspa-sdk
- lilv-utils
- mod-host

- avw.lv2 drumkv1-lv2 guitarix-lv2 invada-studio-plugins-lv2 ir.lv2
  lv2fil mda-lv2 modep-lv2-amsynth modep-lv2-calf modep-l v2-caps
  modep-lv2-fluidplug modep-lv2-fomp modep-lv2-freaked
  modep-lv2-guitarix modep-lv2-gx-slowgear modep-lv2-gx-switchless-wah
  modep-lv2-infamousplugins modep-lv2-invada-studio modep-lv2-mda
  modep-lv2-mod-distortion modep-lv2-rkrlv2 modep-lv2-setbfree
  modep-lv2-shiroplugins modep-lv2-tap modep-lv2-tinyg

## Using `mod-host`

`mod-host -i` starts a interactive session.  Commands can be entered from the command line
Otherwise send commands to socket: 5555 (`-p N` sets socket to `N`)

`lv2ls` lists all `lv2` plugins on your system

E.g:
```
http://avwlv2.sourceforge.net/plugins/avw/absolute
http://avwlv2.sourceforge.net/plugins/avw/ad
http://avwlv2.sourceforge.net/plugins/avw/cvs
http://avwlv2.sourceforge.net/plugins/avw/cvtocontrol
http://avwlv2.sourceforge.net/plugins/avw/delay
```

In `mod-host` use command: `add <URI> <N>` where `URI` is found using
`lv2ls` and `N` is a number that will be used to identify the plugin

To find what a plugin's properties are, use `lv2info <URI>`

E.g:

```
$ lv2info http://avwlv2.sourceforge.net/plugins/avw/amp
http://avwlv2.sourceforge.net/plugins/avw/amp

	Name:              AMS Amp
	Class:             Amplifier
	Author:            Aurélien Leblond
	Author Email:      mailto:blablack@gmail.com
	Has latency:       no
	Bundle:            file:///usr/lib/lv2/avw.lv2/
	Binary:            file:///usr/lib/lv2/avw.lv2/amp.so
	UIs:
		http://avwlv2.sourceforge.net/plugins/avw/amp/gui
			Class:  http://lv2plug.in/ns/extensions/ui#GtkUI
			Binary: file:///usr/lib/lv2/avw.lv2/amp_gui.so
			Bundle: file:///usr/lib/lv2/avw.lv2/
	Data URIs:         file:///usr/lib/lv2/avw.lv2/manifest.ttl
	                   file:///usr/lib/lv2/avw.lv2/amp.ttl
	Optional Features: http://lv2plug.in/ns/lv2core#hardRTCapable
	Presets: 

	Port 0:
		Type:        http://lv2plug.in/ns/lv2core#ControlPort
		             http://lv2plug.in/ns/lv2core#InputPort
		Symbol:      gain
		Name:        Gain
		Minimum:     -10.000000
		Maximum:     10.000000
		Default:     1.000000

	Port 1:
		Type:        http://lv2plug.in/ns/lv2core#CVPort
		             http://lv2plug.in/ns/lv2core#InputPort
		Symbol:      input
		Name:        Input

	Port 2:
		Type:        http://lv2plug.in/ns/lv2core#CVPort
		             http://lv2plug.in/ns/lv2core#OutputPort
		Symbol:      output
		Name:        Output
```

See https://github.com/moddevices/mod-host

When a plugin is added with `add` a new `jack` input and output is created for it.


### Installing Python dependencies

On Debian/Ubuntu systems, you will need the following system packages,
so that the `python-rtmidi` Python package can be installed correctly:

```bash
apt-get install python3-dev libasound2-dev libjack-dev
```

You can then install Griode's requirements with `pip`:

```bash
pip install --user -r requirements.txt
```

If you get compilation errors, you might need extra packages
(libraries or headers).

Note: if you have problems related to the installation of `python-rtmidi`,
you might be tempted to try to install `rtmidi` instead. DO NOT! The two
packages are slightly incompatible; so after installing `rtmidi`, perhaps
Griode will start, but you will get another bizarre error at a later point.


### Installing FluidSynth

Fluidsynth is a software synthesizer. On Debian/Ubuntu systems, you
can install it with:

```
apt-get install fluidsynth
```

### Installing SoundFonts

Griode requires at least one "SoundFont" so that FluidSynth can make
sounds. The easiest way to get started is to go to the `soundfonts/`
subdirectory, and run the script `download-soundfonts.sh`.

Griode will load SoundFonts called `?.sf2` in alphabetical order.
The `download-soundfonts.sh` script will create a symlink `0.sf2`
pointing to the "GeneralUser GS" SoundFont, which contains the
128 instruments of the General MIDI standard, as well as a few
variations, and a few drum kits.

You are welcome to download your own soundfonts, place them in
the `soundfonts/` subdirectory, and create symlinks to these files:
they will be loaded when you start Griode.


#### What are soundfonts?

SoundFonts are instrument banks used by some audio hardware and by FluidSynth
to generate notes of music. The typical extension for SoundFont files is `.sf2`.

There are many SoundFonts available out there.
Some of them are tiny: the Sound Blaster AWE32 (a sound card from the mid-90s)
had 512 KB of RAM to load SoundFonts, and there are SoundFonts of that size
that offer the 100+ instruments of the General Midi standard! And some
SoundFonts are huge: I saw some 1 GB SoundFonts out there with just a couple
of piano instruments in them, but in very high quality (i.e. using different
samples for each note and for different velocity levels.)

Here are a few links to some SF2 files:
- [GeneralUser](http://www.schristiancollins.com/generaluser.php)
- [Fluid SoundFont](https://packages.debian.org/source/sid/fluid-soundfont)
- [Soundfonts4U](https://sites.google.com/site/soundfonts4u/)
- [8bitsf](https://musical-artifacts.com/artifacts/23/8bitsf.SF2)
- [BASSMIDI](https://kode54.net/bassmididrv/BASSMIDI_Driver_Installation_and_Configuration.htm)

### LaunchPad

Griode currently supports the Launchpad Pro, Launchpad X, the
Launchpad MK2 (aka "RGB"), and has partial support for the Launchpad
S. You can plug multiple controllers and use them simultaneously.

Griode relies on the name of the MIDI port reported by the `mido` library
to detect your Launchpad(s). This has been tested on Linux, but the port
names might be different on macOS or Windows.


## Debugging

You can set the `LOG_LEVEL` environment variable to any valid `logging`
level, e.g. `DEBUG` or `INFO`:

```
export LOG_LEVEL=DEBUG
./griode.py
```

Note:
- `DEBUG` level is (and will always be) very verbose.
- You can put the log level in lowercase if you want.
- The default log level is `INFO`.


### Persistence

Griode saves all persistent information to the `state/` subdirectory.
If you want to reset Griode (or some of its subsystems) to factory defaults,
you can wipe out this directory (or some of the files therein).


### Starting automatically on boot

If you are using a Raspberry Pi running the Raspbian distribution,
and want to automatically start Griode on boot, you can use the provided
systemd unit (`griode.service`).

After checking out the code in `/home/pi/griode`, and confirming that
it runs correctly, just run:

```
sudo systemctl enable /home/pi/griode/griode.service
sudo systemctl start griode
```

Griode will start, and it will be automatically restarted when the
Pi reboots.

If it doesn't start, or if you want to see what's going on:

```
sudo systemctl status griode
sudo journalctl -u griode
```

## Controlling `griode` at run time

Write commands to nammed pipe: .command

Commands are formatted:
<Name> [<args>]

Recognised names:

* draw

  Redraw the keypad

* instrument F P B N

  F:Int Font
  P:Int Programme
  B:Int Bank
  N:Str Name


## Bugs

- If you keep a note pressed while switching to another gridget,
  the note will continue to play. This is almost by design.
- If you keep a note pressed while stopping recording, it might
  record a zero-length notes.
- If a sync is triggered while notes are pressed, it might result
  in zero-length notes.

