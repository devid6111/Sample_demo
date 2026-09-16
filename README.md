# Sample_demo
my project
This is my first project
<br>
devid6111
git and github is main tool.
<br>
Author:cdac
<br>
<br>
<br>
<br>
<br>
Offline Guide
<br>
By default, CesiumJS uses several external data sources which require internet access at runtime, though none of these dependencies are required. This guide lists these external sources and how to configure CesiumJS to work in a fully offline (no internet access) environment.
<br>
Imagery
The default imagery provider in CesiumJS is Cesium ion global imagery through Bing Maps. This provider loads data from api.cesium.com and dev.virtualearth.net as well as several other tile servers that are subdomains of virtualearth.net. To use another provider, pass it into the constructor for the Viewer widget.
<br>
If you have an imagery server on your local network (e.g. WMS, ArcGIS, Google Earth Enterprise), you can configure CesiumJS to use that. Otherwise, CesiumJS ships with a low-resolution set of images from Natural Earth II in Assets/Textures/NaturalEarthII.
<br>
By default, the BaseLayerPicker includes options for several sample online imagery and terrain sources. In an offline application, you should either disable that widget completely, by passing baseLayerPicker : false to the Viewer widget's constructor, or use the imageryProviderViewModels and terrainProviderViewModels options to configure the sources that will be available in your offline application.
<br>
Geocoder
The Geocoder widget, which allows flying to addresses and landmarks, uses the Cesium ion API at api.cesium.com. In your offline application, you should disable this functionality by passing geocoder : false to the Viewer constructor.
<br>
Example
This example shows how to configure CesiumJS to avoid use of online data sources.
<br>
const viewer = new Cesium.Viewer("cesiumContainer", {
  baseLayer: Cesium.ImageryLayer.fromProviderAsync(
    Cesium.TileMapServiceImageryProvider.fromUrl(
      Cesium.buildModuleUrl("Assets/Textures/NaturalEarthII"),
    ),
  ),
  baseLayerPicker: false,
  geocoder: false,
});
3D Tiles, glTF, and other static files
Most other files loaded in CesiumJS, such as 3D Tiles or glTF, are static assets that do not require any server-side operations to load. However, since browsers commonly treat requests to load resources using the file:// schema as cross-origin requests, it's recommended that you set up a local server.
<br>
Download and install Node.js
<br>
At the command line, run
<br>
npm install http-server -g
This will install the 'http-server' app from https://github.com/http-party/http-server globally
<br>
In the directory that contains the data, run
<br>
http-server -a localhost -p 8003 --cors=http://localhost:8080/
This will start the server, under the address localhost, using port 8003. The cors parameter will allow the a CesiumJS app running at port 8080 to access the data from this locally running server.
<br>
Load files in a CesiumJS app at the served url.
<br>
For example, a local tileset in an example directory can now be loaded with the following url:
<br>
try {
  const tileset = await Cesium.Cesium3DTileset.fromUrl(
    "http://localhost:8003/example/tileset.json",
  );
  viewer.scene.primitives.add(tileset);
} catch (error) {
  console.log(`Error loading tileset: ${error}`);
}