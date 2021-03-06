offline-editor-js
=================

A prototype JavaScript toolkit for using the ArcGIS API for JavaScript offline. It manages both editing and tiles in an offline mode. It's still a work-in-progress so if you have suggestions open an issue or if you want to make a pull request we welcome your proposed modifications. 

*IMPORTANT:* If you want a full, robust offline solution then you should be using our ArcGIS Runtime SDKs for .NET, WPF, Java, iOS, Android and Qt.

This repo contains the following libraries:

- `/edit`: handles vector features and stores adds, updates and deletes while offline. Resync's edits with server once connection is reestablished
   * `offlineFeaturesManager` - Extends and overrides a feature layer.
   * `editsStore` - Provides static helper methods for working with the offline data store.
- `/tiles`: stores portions of tiled maps client-side and uses the cached tiles when device is offline
   * `offlineTilesEnabler` Extends and overrides a tiled map service from ArcGIS Online or for partial offline use.
   * `OfflineTilesEnablerLayer` Extends any Esri tiled basemap service for a web app that has a requirement for browser reload and/or restart. This library should be used in conjunction with an application cache coding pattern.
- `/tpk`: lets you work with TPK files.
   * `TPKLayer` - parses a TPK file and displays it as a tiled map layer.
- `/utils`: contains various helper libraries.
- `/samples`: sample apps to show how to use different aspects of the offline library capabilities.

#Workflows Supported (v1)
The following workflow is currently supported for both both features and tiles:

1) Load web application while online.
 
2) Once all tiles and features are loaded then programmatically take application offline. 

3) Make edits while offline.

4) Return online when you want to resync edits.

Using an [application manifest](https://developer.mozilla.org/en-US/docs/HTML/Using_the_application_cache) allows you to reload and restart the application while offline. The application manifest let's you store .html, .js, .css and image files locally.

__Attachment Support__: Attachments are supported with some limitations. See documentation [here](./doc/attachments.md)


#API Doc

##offlineFeaturesManager
Extends and overrides a feature layer. This library allows you to extend esri.layers.FeatureLayer objects with offline capability and manage the resync process.

* __Click [here](doc/offlinefeaturesmanager.md) to see the full API doc for `offlineFeaturesManager`__

 
##offlineTilesEnabler
Extends and overrides a tiled map service. Provides the ability to customize the extent used to cut the tiles. See the detailed description of basemap.prepareForOffline() in the "How To Use" section to learn different options.

* __Click [here](doc/offlinetilesenabler.md) to see the full API doc for `offlineTilesEnabler`__ 

##TPKLayer

Extends TileMapServiceLayer. You can display TPK files with this library. TPK's are binary tile package files. Go [here](http://resources.arcgis.com/en/help/main/10.1/index.html#//00170000017w000000) for more information on how to create a TPK file.

* __Click [here](doc/tpklayer.md) to see the full API doc for `TPKLayer`__ 

#How to use

* [Learn more about using the `tile` library](doc/howtousetiles.md)
* [Learn more about using the `edit` library](doc/howtouseeditlibrary.md)
* [Learn more about using the `tpk` library](doc/howtousetpklibrary.md)
* [Learn more about using an application cache with this library](doc/howtouseappcache.md)


##Setup Instructions

1. [Fork and clone the repo.](https://help.github.com/articles/fork-a-repo)
2. After cloning from github, `cd` into the `offline-editor-js` folder
3. Run `git submodule init` and `git submodule update`
4. Try out the apps in the `/samples` folder.


##Samples
* `appcache-features.html` - shows how to work with the application manifest, tiles and features.
* `appcache-tiles.html` - shows how to work with the application manifest and map tiles.
* `attachments-editor.html` - demonstrates how to work with this library and feature attachments.
* ~~`military-offline.html`~~ - renamed `draw-pointlinepoly-offline.html` shows working with points, lines and polygons locally.
* `tpklayer.html` - shows how to work with TPK files.
* `tiles-indexed-db.html` - shows how to work with storing tiles locally.
* `Gruntfile.js` - a node.js app and its associated `package.json` file to help with creating an application manifest file.


##Dependencies

* ArcGIS API for JavaScript (v3.8+)
* NOTE: browser limitations and technical dependencies. The offline capabilities in this toolkit depend on certain HTML5 capabilities being present in the browser. Go [here](doc/dependencies.md) for a detailed breakdown of the information.
	* We offer browser support for Chrome and Safari only, at this time. Some of the capabilities in the repository will not work on Internet Explorer. We continue to evaluate IE's capabilities as new releases become available to try and identify a point where we might be able to support it.  	

* Sub-modules (see `/vendor` directory)

   * [offline.js](https://github.com/hubspot/offline) - it allows detection of the online/offline condition and provides events to hook callbacks on when this condition changes
   * [IndexedDBShim](https://github.com/axemclion/IndexedDBShim) - polyfill to simulate indexedDB functionality in browsers/platforms where it is not supported (notably desktop Safari and iOS Safari)
   		- IMPORTANT: There is a known [issue](https://github.com/axemclion/IndexedDBShim/issues/115) with IndexedDBShim on Safari. The workaround is to switch from using /dist/IndexedDBShim.min.js to just using IndexedDBShim.js and then search for and modify the line that defines the value for `DEFAULT_DB_SIZE`. Set this to more appropriate size that will meet all your storage needs, for example: ```var DEFAULT_DB_SIZE = 24 * 1024 * 1024```
   * [jasmine.async](https://github.com/derickbailey/jasmine.async.git) - library to help implementing tests of async functionality (used in tests)

* Non sub-module based libraries
	* [FileSaver.js](https://github.com/Esri/offline-editor-js/blob/master/lib/tiles/README.md) - library to assist with uploading and downloading of files containing tile information.
	* [grunt-manifest](https://github.com/gunta/grunt-manifest) node.js library to assist with the creation of manifest files.
	* [zip](http://gildas-lormeau.github.io/zip.js/) A library for zipping and unzipping files. 
	* [xml2json](https://code.google.com/p/x2js/) A library for converting XML to JSON. Seems to handle complex XML. 

## Resources

* [ArcGIS Developers](http://developers.arcgis.com)
* [ArcGIS REST Services](http://resources.arcgis.com/en/help/arcgis-rest-api/)
* [twitter@esri](http://twitter.com/esri)

## Issues

Find a bug or want to request a new feature?  Please let us know by submitting an [issue](https://github.com/Esri/offline-editor-js/issues?state=open).

## Contributing

Anyone and everyone is welcome to contribute. Please see our [guidelines for contributing](https://github.com/esri/contributing).


## Licensing
Copyright 2014 Esri

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

A copy of the license is available in the repository's [license.txt]( license.txt) file.

[](Esri Tags: ArcGIS Web Mapping Editing FeatureServices Offline)
[](Esri Language: JavaScript)


