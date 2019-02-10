---
title: "How to get QGIS3 with Homebrew with all the bells and whistles"
# date: 2018-12-01 00:00 +00:00
# last_modified_at: 2018-12-11 20:45 +00:00
header: 
 overlay_image: /assets/images/blog/2018/QGIS_logo_2017.svg.png
 teaser: /assets/images/blog/2018/QGIS_logo_2017.svg.png
 caption: QGIS Logo
#  overlay_filter: 0.3
categories: [Technology, GIS]
tags: [QGIS, macOS, homebrew]
twitter: 
  image: /assets/images/blog/2018/QGIS_logo_2017.svg.png
#   hashtags: [tag1, tag2] # Only for Twitter, they go before tags
# toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
# toc_sticky: true
---

Not long ago, I wrote a post about [how to install QGIS with Homebrew](/blog/2018/03/12/qgis-on-macos-with-homebrew/). At that moment, although the development of QGIS 3 for macOS was already finish the the options to install QGIS 3 on macOS were quite limited and if you wanted to get it mostly the only option was to use the developers formula in Homebrew. 

Luckily, this has changed a lot. A couple of days ago, the maintainers of the Homebrew's tap that hold the QGIS formula gave to it the final touches and now even have more options than if you install QGIS using the binary from the [official website](https://www.qgis.org/).

If you want to install QGIS 3 on macOS with Homebrew, you just need: 

1. To have or [install Homebrew](/blog/2017/11/21/homebrew/). 

2. Tap the [osgeo/osgeo4mac](osgeo/osgeo4mac) tap.   

    ```shell
    $ brew tap osgeo/osgeo4mac
    ```

3. Install it:   

    ```shell
    $ brew install qgis
    ```
{: style="display: block;"}

That's it. You don't need anything more if you just want QGIS 3. If you are lucky[^2] you are probably going to get a bottled version and you aren't going even to need to build. 

When the install finish, you just need to make an alias to `/Applications` if you want to have QGIS there. I prefer the alias to the symbolic link because an alias shows up in Spotlight. You can make the alias running the following script: 

```sh
#!/usr/bin/env bash  
trash /Applications/QGIS.app 
qgis_location=$(find $(brew --prefix)/Cellar/qgis/ -name "3.*" -print -quit)/QGIS.app
osascript -e 'tell application "Finder"' -e 'make new alias to file (posix file "'$qgis_location'") at (posix file "/Applications")' -e 'end tell'
```

\* If you don't have `trash` installed in your system —`brew install trash`— you can use `rm -fr`, but be careful, `rm` will delete **forever** your `QGIS.app` if you have one in `/Applications`. I recommend you to install `trash` and use it often when you are deleting things from the shell. It just moves to trash files and folders.   
{: .small}

If you want to know a little more about the installation process, I can tell you that now the QGIS 3 formula is split into two formulas, `qgis` and `qgis-res`. The latter one give support to the former and make the building faster, even when you want to install everything. QGIS has a lot of Python dependencies and they don't need to be installed every time QGIS is installed or updated. This is the logic behind `qgis-res`. 

## I want more! 

If you want to have QIGS with all the additional capabilities and plugins it's shipped on the official version for macOS, it's easy. You just need to add flags to the install command. If my memory doesn't fail QGIS 3 is shipped with, [Grass 7](https://grass.osgeo.org), [Saga GIS](http://www.saga-gis.org/en/index.html), [R](https://www.r-project.org)[^1] and [GDAL](https://www.gdal.org). GDAL is installed with the basic installation of QGIS 3, but all the other capabilities and plugins need a flag. 

```shell
$ brew install qgis --with-grass --with-saga --with-r 
```

However, and here are the wonderful news, you can install QGIS with much more using Homebrew now. These are the additional flags I'm using: 

- `--with-gpsbabel`: Build with [GPSBabel](https://www.gpsbabel.org). Read, write and manipulate GPS waypoints in a variety of formats. 
- `--with-hdf5`: Build with [HDF5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format) support. 
- `--with-lastools`: Build with [LAStools](https://rapidlasso.com/lastools/): Efficient tools for LiDAR processing
- `--with-liblas`: Build with [liblas](https://en.wikipedia.org/wiki/LibLAS) support. 
- `--with-mssql`: Build with Microsoft ODBC Driver for [SQL Server](https://en.wikipedia.org/wiki/Microsoft_SQL_Server).
- `--with-orfeo`: Build extra [Orfeo Toolbox](https://www.orfeo-toolbox.org/) for Processing plugin.
- `--with-pdal`: Build with [pdal](https://pdal.io/) support.
- `--with-qspatialite`: Build [QSpatialite](https://en.wikipedia.org/wiki/SpatiaLite) Qt database driver.
- `--with-szip`: Build with [szip](https://support.hdfgroup.org/doc_resource/SZIP/) support.
- `--with-taudem`: Build with [TauDEM](http://hydrology.usu.edu/taudem/taudem5/index.html): Terrain Analysis Using Digital Elevation Models for hydrology.
- `--with-whitebox`: Build with [Whitebox Tools](http://www.uoguelph.ca/~hydrogeo/Whitebox/): An advanced geospatial data analysis platform.

And you can even build with more additional capabilities, like: 

- `--with-3d`: Build with 3D Map View panel.
- `--with-api-docs`: Build the API documentation with Doxygen and Graphviz.
- `--with-isolation`: Isolate .app's environment to HOMEBREW_PREFIX, to coexist with other QGIS installs.
- `--with-netcdf`: Build with [NetCDF](https://en.wikipedia.org/wiki/NetCDF) support.
- `--with-openvpn`: Build with [OpenVPN](https://openvpn.net) support.
- `--with-oracle`: Build extra [Oracle](https://en.wikipedia.org/wiki/Oracle_Corporation) geospatial database and raster support.
- `--with-postgresql10`: Build with [PostgreSQL](https://www.postgresql.org) 10.
- `--with-server`: Build with QGIS Server (qgis_mapserv.fcgi).

For me the building time is around 40', but I have an old Mac (MBP late 2011). In new Macs the building process can be around just 20'-30'. 

This is the best QGIS we have had in a really long time. We have to thank to people like [@jfperini](https://github.com/fjperini) and [@nickrobison](https://github.com/nickrobison) who have been working really hard to reach this level. I've been lending them a small hand and I'm been testing builds too. 

## Looking Forward

In the near future are more improvements to come, you can be sure of that. One of the thinks we are thinking about is to create a bottle with all the capabilities —all the flags— so people don't need to build QGIS in their machines if they wanted to have a complete build of QGIS. 

I hope we can keep working on improving it, or at least maintain what we already have right now. I really encourage you to install QGIS using Homebrew because **it's great now**. 









[^1]: It's shipped with a plugin to connect to R and run R scripts. As far as I know, you need to have R installed.
[^2]: I say "if you're lucky" because the tap has had some problems with the bottles lately. But they are workin on it and soon it will be solved. 

