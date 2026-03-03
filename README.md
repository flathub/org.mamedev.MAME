# README

## Table of Contents

- [Flatpak](#flatpak)
- [Security](#security)
- [Building](#building)
    - [Manifest](#manifest)
- [Configuration](#configuration)
- [MAME Tools](#mame-tools)

## Flatpak

This is a method for distributing MAME using the Flatpak format. The main reason for this is on locked down systems that only allow sandboxed user applications.

## Security

The use of Flatpak here is not intended to secure MAME or provide any isolation from the rest of your system. MAME requires access to many of your systems resources including, but not limited to, devices for controllers, X11 sessions, filesystems writable for configuration and readable for data files, networking, and audio. The Flatpak provided sandbox must be opened up in many areas for MAME to function properly.

## Building

`build_icons.sh` will download the logo from the [https://www.mamedev.org](https://www.mamedev.org) site and resize it to various sizes to use as an icon.

`build_local.sh` will invoke the `flatpak-builder` application to compile MAME and generate a Flatpak package.

`org.mamedev.MAME.yaml` is the main Flatpak manifest that includes build and install information.

### Manifest

An explanation for some of the options included in the `org.mamedev.MAME.yaml' manifest file.

`org.kde.Platform` is used for its Qt support, required to build the MAME debugger.

`--device=all` added to allow various input devices, controllers, etc.

`--persist=.mame` writable location used for cfg, diff, nvram, etc.

`--filesystem=host:ro` primarily to be used for external data, roms, samples, etc.

## Configuration

The default ini paths include the home `~/.mame` directory and two relative paths '.' and 'ini'.  The relative paths are not helpful inside the sandbox, and the `~/.mame` contents shouldn't be overwritten with each new MAME release.  Instead the default ini search path is patched to `$HOME/.mame;/app/share/mame/ini`.  This will allow a default base ini in `/app/share/mame/ini/mame.ini` and can be overridden in `~/.mame/mame.ini`.

## MAME Tools

The various MAME tools, [https://docs.mamedev.org/tools/index.html](https://docs.mamedev.org/tools/index.html), can be called using the Flatpak. For a list of the commands to use the various tools, see below.

* [chdman](https://docs.mamedev.org/tools/chdman.html)
    * **Base Command**
        * `flatpak run --command=chdman org.mamedev.MAME`
    * **Example**
        * `flatpak run --command=chdman org.mamedev.MAME verify -i foobar.chd`
* [Imgtool](https://docs.mamedev.org/tools/imgtool.html#imgtool-format-info)
    * **Base Command**
        * `flatpak run --command=imgtool org.mamedev.MAME`
    * **Example**
        * `flatpak run --command=imgtool org.mamedev.MAME getall coco_jvc_rsdos myimage.dsk`
* [Castool](https://docs.mamedev.org/tools/castool.html)
    * **Base Command**
        * `flatpak run --command=castool org.mamedev.MAME`
    * **Example**
        * `flatpak run --command=castool org.mamedev.MAME convert coco zaxxon.cas zaxxon.wav`
* [Floptool](https://docs.mamedev.org/tools/floptool.html)
    * **Base Command**
        * `flatpak run --command=floptool org.mamedev.MAME`
    * **Example Command**
        * `flatpak run --command=floptool org.mamedev.MAME convert ddp mybasicprogram.ddp mybasicprogram.wav`
