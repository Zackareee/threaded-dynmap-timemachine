# Minecraft Dynmap Time Machine

CLI script in Python 2.7 that downloads tiles from a Minecraft's Dynmap plugin HTTP server and composes one image in extremely large resolution suitable for print.

![Scaled down image](https://raw.githubusercontent.com/martinsik/minecraft-dynmap-timemachine/master/doc/majncraft.3320.thumb.png)

This is a scaled down image form original `20736 x 13824`px. You can also [download full size 389 MB image](https://www.dropbox.com/s/hhq6jbuxyu6fmr0/majncraft.20736.full.png?dl=0). See example bellow.

List of all parameter is as follows:

    $ python2.7 main.py -h
    usage: main.py [-h] [--list-worlds] [--list-maps] [-q] [-v] [-vv]
                   base_url [world] [map] [center] [boundary_size] [zoom] [dest]
    
    positional arguments:
      base_url              Dynamp server URL
      world                 world name, use --list-worlds to list available worlds
      map                   map name, use --list-maps to list available maps
      center                minecraft cooridnates, use format: [x,y,z]
      boundary_size         size in tiles, use format: [h,v]
      zoom                  zoom level, 0 = maximum zoom
      dest                  output file name
    
    optional arguments:
      -h, --help            show this help message and exit
      --list-worlds         list available worlds from this Dynmap server and exit
      --list-maps           list available maps for this world and exit

## 1. Example usage - capture one large image

Let's say we want to download a `20736x13824`px map (286 Mpx image) from [map.majncraft.cz](http://map.majncraft.cz/) at Minecraft position `[3300,65,-2630]`.


1. **First see what worlds are available and what's the name of the world that we want**

   ```
   $ python2.7 main.py --list-worlds http://map.majncraft.cz/
   world - Svět Majncraft | Overworld
   world_space - Vesmír | Space
   world_nether - Nether Reloaded
   ```

   We want the first world on the list called simply `world`.

2. **Then list all maps avaialble for this world**

   ```
   $ python2.7 main.py --list-worlds http://map.majncraft.cz/ world
   surface - Prostorová - Den
   surface_night - Prostorová - Noc
   populated - Osídlení světa - prostorové
   flat - Plochá - Den
   populated_flat - Osídlení světa - ploché
   ```
    
   This lists flat, isometric, cave and all other types of maps together. Map names depend on Dynmap's configuration. We want the first one called `surface` which is an isometric map.
   
3. **Make a test image with Minecraft's coordinates**

   Check your coordinates on Dynmap or simply walk in Minecraft at the position that you want to capture and press F3 to see what are your Minecraft's coordinates. Then make a test image to make sure that the position captured by `minecraft-dynmap-timelapse` is correct:
   
   ```
   python2.7 main.py http://map.majncraft.cz/ world surface \
       [3300,65,-2630] [3,2] 0 majncraft.test.png
   ```
   
   Used parameters:
   
   - `http://map.majncraft.cz/` - Dynmap's HTTP server URL
   - `world` - World name
   - `surface` - Map name
   - `[3300,65,-2630]` - Minecraft coordiantes that will be automatically converted to tile names
   - `[3,2]` - Number of tiles I want to download in each direction from specified coordinates. That's two to the left and right, two to the top and bottom. This will actually download 6x4 grid where each tile is 128x128 pixels. In total this image will be 768x512 pixels
   - `0` - Zoom level. 0 means maximum zoom in. Number of zoom levels depend's on Dynamp's configuration
   - `majncraft.test.png` - Output file name
   
   This should generate a 768x512 image:
   
   ![Preview from 6x4 grid](https://raw.githubusercontent.com/martinsik/minecraft-dynmap-timemachine/master/doc/majncraft.3320.test.png)
   
4. **Make a full size image in 20736x13824 resolution.**
   
   Finally, we can make the full size image:
   
   ```
   python2.7 main.py -v http://map.majncraft.cz/ world surface \
       [3300,65,-2630] [81,54] 0 majncraft.3320.full.png
   ```
   
   This takes a while because in total it needs to download `81 * 2 * 54 * 2 = 17496` tiles. The final image has 389 MB.
   
   ![The final image scaled down to 728px width](https://raw.githubusercontent.com/martinsik/minecraft-dynmap-timemachine/master/doc/majncraft.3320.thumb.png)
   
   You can download the [full 20736x13824 size image for this example (389 MB)](https://www.dropbox.com/s/hhq6jbuxyu6fmr0/majncraft.20736.full.png?dl=0) or a different, [smaller 16384x10240 image (168 MB)](https://www.dropbox.com/s/c6zzpv2cd26x76g/majncraft.16384.png?dl=0) if you just want to see what it looks like in full resolution.