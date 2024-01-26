mp3chop
=======

A natty little utility for extracting bits of ADTS MP3 and AAC
files. It does this by walking the frames of the file knowing how much
audio they will generate and enables/disables output as appropriate
depending on the timecode associated with the frame. It doesn't
actually understand the contents of the frames. MP3 frames can contain
references to other frames and often do. This means that mp3chop can
generate output that contains references to frames that do not exist
in the output. Because the MP3 is a streaming format decoders should
deal with this correctly and glean what information they can from the
frames before discarding them if they cannot produce audio from
them. Some decoders may output a diagnostic such as "frame discarded"
when this happens. AAC ADTS frames are always self contained so this
problem does not occur.

Alternatives
============
Since I started developing mp3chop over twenty years ago, other tools have
been developed that handle far more file formats and might be a better
alternative. The one I now use is [ffmpeg](https://ffmpeg.org/) which can
be used to chop arbitrary files with commands like:

```shell
ffmpeg -loglevel warning -y -i input_file.mp3 -ss mm:ss -to mm:ss -c copy output_file.mp3
```

The existence of alternatives means that mp3chop isn't really developed any
more.

Licensing
=========
mp3chop is licensed under the GNU General Public License version 2 or, at
your option, later. See the COPYING file for a copy of this licence.

Installation on POSIX-like Systems
==================================
mp3chop uses the Meson build system. You should first install Meson,
either from your Linux distribution using something like:

    apt install meson

or PIP:

    pip3 install --user meson

then run:

    meson --buildtype=release build
    ninja -C build

You can run the mp3chop tool from the build directory, or copy it
elsewhere. Alternatively, to install the program and its manual page
on your system run:

    sudo ninja -C build install

More information about Meson can be found at https://mesonbuild.com/

Contributing
============
mp3chop is maintained at [GitLab](https://gitlab.com/mp3chop/mp3chop). Please submit issues and merge requests there.

Also see:

- https://www.mcrowe.com/~mac/mp3chop/

Mike Crowe
2022/05/08
