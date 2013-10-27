wallx
=====

Create wallpapers for multiple monitors and machines to form a single image.

Overview
--------

"wallx" is a small utility to create wallpapers targeted at multiple
screens, possibly of different dimension, pixel density (dpi), or even
misaligned (one monitor not lined up with the next one).

The screens must be specified on the command line from left to right.
There are no support for stacked monitors (e.g. grid arrangement) but
it would not be hard to handle if someone was interested.

Requirements
------------

 - You need either Python 2.7 ou Python 3.x. (The script works on both
   major version.)

 - You need ImageMagick (its "convert" command)

 - A ruler :)

Usage
-----

    Usage: wallx [OPTIONS] INPUT OUTPUT

     -h, --help            Print this help.
     -s, --screen=SPEC     Define a screen. See below.
     -t, --then            Start a new set of screens.
     -C, --crop=CROP_SPEC  Limit the action on a subpart of the input image.
     -x, --xcenter=FLOAT   The gravity in the X direction. Default to 0.5.
     -y, --ycenter=FLOAT   The gravity in the Y direction. Default to 0.5.
     -S, --scale=FLOAT     Scale the image. Default to 1.0. Must be >= 1.0.
     -p, --pretend         Just show what command would be run, but do nothing.
     -v, --verbose         Verbose output.

    At least one screen should be specified.

    Screen specification:

      The format is <width>x<height>/<ppm>[@<top>][+<gap>], where:

       * `width' is the screen width in pixel

       * `height' is the screen height in pixel

       * `ppm' is the screen density (pixel per meter). It is possible to
         specify a density in DPI by using the "dpi" suffix. See examples
         below. Alternatively, you can specify the physical *width* of the
         panel with a value suffixed by either "m", "mm", "cm" or "in".

       * `top' (optional) is the offset from the top of all
         screens. Default unit is meter. You can suffix one of the
         following unit: "mm", "cm" or "in".

       * `gap' (optional) is the distance between two consecutive screens
         (gap between panels). Default unit is meter. You can suffix one
         of the following unit: "mm", "cm" or "in".

    Crop specification:

      The format is <width>x<height>(-|+)<x_offset>(-|+)<y_offset>.

      The format follow the geometry syntax used by X11. A negative offset
      (including -0) refers to the right or the bottom of the image. For
      example: 2400x1200-0-0 will work on the image zone of size 2400x1200
      on the lower right corner.

    Gravity

      With the --xcenter and --ycenter option, you can choose how to
      center the screens on the source image. The default is to take the
      center of the image if the screens doesn't cover it all.

      A gravity of 0 means either top or left alignment in the image.
      A gravity of 1 means either right or bottom alignment in the image.
      A gravity of 0.5 means the center of the image.

    Examples:

      If you have 2 monitors with different resolutions and you don't care
      about the gap between them nor the top alignement, you can use
      something like this:

        wallx --screen 2560x1440/59.8cm --screen 1680x1050/46.4cm landscape.png wallpaper.png

      A more complex example, if you have 3 machines, with 4 monitors
      total (1 on the first, 2 on the middle one, and 1 on the last one),
      and if you measure how they are aligned vertically and the gap
      between them (from panel to panel), you can use something like this:

        wallx --screen 1920x1200/20.5in \
            --then --screen 2560x1440/59.8cm@1.5cm+15cm --screen 1680x1050/46.4cm@5.5cm+4.5cm \
            --then --screen 1920x1080/93.7dpi+8cm \
            landscape.png wallpaper.png

      The above command will produce 3 images:

         1_wallpaper.png, 2_wallpaper.png, 3_wallpaper.png.

      You can then setup each wallpaper on each machine, and the source
      image should be nicely spread accross all your monitors.
