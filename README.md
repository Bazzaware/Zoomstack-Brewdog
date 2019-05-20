# Zoomstack-Brewdog
OS Open Zoomstack Masterclass

In this workshop we are going to explore the OS Open Zoomstack vector tiles.  You will be creating a custom style to make your very own map using Maputnik, an open source vector tile style editor. By the end of the session you will have deployed your very own custom web map using Maputnik and Github. All you need to get started is a Github account, Wi-Fi and a modern browser (we recommend Google Chrome.)

Let’s get started!

Part one: Opening Maputnik with the OS Night Style

■ Open the Maputnik web application in a web browser: http://maputnik.github.io/editor/
   We are going to start by using the OS Night Style.

■ Press the Open Button in the top left:


■ Copy & Paste in the following URL: https://s3-eu-west-1.amazonaws.com/tiles.os.uk/v2/styles/open-zoomstack-night/style.json

■ Then press the ‘Open URL’ button:


You should now have the OS Night style loaded in Maputnik.

Awesome! That’s how easy it is to start using OS Open Zoomstack - now we’re going to customise the map…









Part two: Loading Brewdog locations

Similar to how search engines read websites, it is easy to create a script to read the location data from the Brewdog website.  We created a script to extract their UK locations and save them as GeoJSON, which can then be converted to vector tiles using Mapbox tippecanoe.

Whilst you can make the data yourself, we have saved you the trouble.

■ Click Data Sources


■ Now add the Brewdog vector tile data.  Under the Add New Source section (at the bottom) set the name of the ‘Source ID’ to brewdog (see image below).

■ Then Copy and Paste in this Brewdog vector tile data configuration as ‘TileJSON URL’: https://snodnipper.github.io/os-zoomstack-masterclass/data/brewdog/config.json



The config file simply tells the vector tile software where to load the data, what fields to expect, the zoom levels of the data etc.

■ Finally, click Add Source and close the window.

Note: we now have the Brewdog data source configured but you won’t see anything on the map just yet, in fact it won’t appear like anything has changed.  One vector tile source can contain different types of data, where each set of data is called a layer.  For instance, OS Open Zoomstack has ‘building’ and ‘road’ layers in addition to many more.  We must add the Brewdog layer to the map in order to see it.

■ Before we add the layer, we will add a sprite sheet with the Brewdog symbology.  We are doing this so we can use the Brewdog logo instead of a simple point.

■ Click the Style Settings button at the top:

■ Then change the Sprite URL by Copy & Pasting the following: https://snodnipper.github.io/os-zoomstack-masterclass/style/brewdog/sprites



Then close the ‘Style Settings’ panel.
This ensures that the map has access to these symbols:



Now we’re going to add the brewdog layer to the map.

■ Click the Add Layer button from the top left:


■ Then set the ID, Source and Source Layer values to brewdog and set the type to Symbol.
   Make sure brewdog is lowercase.



You should now see the new brewdog layer on the bottom left of the layer panel - you may need to scroll down to see it.



■ Now we want to style the layer.  Click on the brewdog layer and scroll to the bottom of the second panel.

■ You can set the text, icon, style stops etc. yourself but to save you time you can paste the following code into the JSON editor:

{
  "id": "brewdog",
  "type": "symbol",
  "source": "brewdog",
  "source-layer": "brewdog",
  "paint": {
    "text-color": "rgba(6, 211, 247, 1)",
    "text-opacity": {
      "stops": [
        [
          7,
          0
        ],
        [
          12,
          1
        ]
      ]
    }
  },
  "layout": {
    "text-font": [
      "Source Sans Pro Bold"
    ],
    "text-field": "{name}",
    "icon-image": "brewdog",
    "text-offset": [
      0,
      1.7
    ],
    "icon-size": {
      "stops": [
        [
          5,
          0.3
        ],
        [
          12,
          0.7
        ]
      ]
    },
    "text-size": 14,
    "text-max-width": 20
  }
}


You should now have a map that looks something like this:

You will notice that as you zoom in, the labels appear.

Part three: let’s add a bit of brand flair to the map

■ Select the ‘sea’ layer from the left-hand panel and change the colour to #224952

■ Do the same for the ‘surfacewater’ layer.







■ Add some glow to the buildings by first duplicating the ‘buildings’ layer:



■ Now rename the layer ID to buildings-glow and move it just below ‘buildings’ in the list.  You must click and drag on the left of the layer name.



■ Now you can set the line width, caps and opacity.  To save you time, you can paste the following text into the ‘building-glow’ JSON Editor:

{
  "id": "buildings-glow",
  "type": "line",
  "source": "composite",
  "source-layer": "buildings",
  "minzoom": 13,
  "layout": {
    "line-cap": "round",
    "line-join": "round"
  },
  "paint": {
    "line-color": "rgba(10, 211, 236, 0.6)",
    "line-width": 0.8,
    "line-opacity": {
      "stops": [
        [
          13,
          0
        ],
        [
          16,
          1
        ]
      ]
    }
  }
}


You will need to zoom in to see the building glow effect.

Feel free to unleash your own creativity and make further style changes to your map. You can use the colour palette that we have provided, if you wish.

Equally, if you’re happy with your map then move straight onto the next step.

Part four: save your style on Github

■ Press the Export button:


■ And then click Download:

■ Head over to github.com and sign in. Create a new repository by clicking ‘New’ or ‘Start a project’:
  


■ Set the name to brewdog-map and enter a description.  Then be sure to select Initialize this repository with a README and click ‘Create repository’ (see image below):



■ Rename the file that we downloaded from Maputnik to style.json

■ Then upload it to your new repo by dragging it onto the Github page and clicking ‘Commit changes’ near the bottom of the page:

■ Now we have the style on Github, we can create an HTML page.  This will load the Mapbox Vector Tile software to display the OS Open Zoomstack data.

■ From the repository page, click ‘Create new file’ and create an HTML file named ‘index.html’:



■ Paste in the following text, replacing the snodnipper handle with your own username:

<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8' />
    <title>Where is your Brewdog? 🍺🐕 using OS Open Zoomstack</title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.1/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.1/mapbox-gl.css' rel='stylesheet' />
    <style>
        body { margin:0; padding:0; }
        #map { position:absolute; top:0; bottom:0; width:100%; }
    </style>
</head>
<body>

<div id='map'></div>
<script>
mapboxgl.accessToken = '';
var map = new mapboxgl.Map({
    container: 'map', // container id
    style: 'https://snodnipper.github.io/brewdog-map/style.json', // stylesheet location
    center: [-1.25, 52.2], // starting position [lng, lat]
    zoom: 6 // starting zoom
});
</script>

</body>
</html>

■ Commit the file by pressing the green ‘Commit new file’ button near the bottom of the page:

■ When you are done, we need to serve your repository.  This can be done from the ‘Settings’ panel near the top of the page:


■ Scroll down to the ‘GitHub Pages’ section and change the drop-down to ‘master branch’ before clicking the ‘Save’ button!

■ Now open a new browser tab and go to your map. The url will be: yourusername.github.io/brewdog-map/

It may take a couple of minutes before it works so try refreshing the page.
Assuming it all went to plan it should look something like this:

Now we’re going to create a story map (an animated scrolling map) using our new style.
This is a technique employed by data journalists to tell stories that have an inherent location element.

Again, we have already created the template code based on a Mapbox example.

■ On Github, edit your index.html and overwrite it with the following code (apologies for how long it is, Copy & Paste will be your friend again!)

■ Once pasted in, you will also need to change the style url on line 94:

<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8' />
    <title></title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.1/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.53.1/mapbox-gl.css' rel='stylesheet' />
    <style>
        body { margin:0; padding:0; }
        #map { position:absolute; top:0; bottom:0; width:100%; }
    </style>
</head>
<body>

<style>
#map {
    position: fixed;
    width:65%;
}
#features {
    width: 35%;
    margin-left: 65%;
    font-family: Arial;
    overflow-y: scroll;
    background-color: #0e1b29;
}
section {
    padding:  25px 50px;
    line-height: 25px;
    border-bottom: 1px solid #23a8d3;
    opacity: 0.25;
    font-size: 20px;
}
section.active {
    opacity: 0.75;
}
section:last-child {
    border-bottom: none;
    margin-bottom: 1000px;
}
    .mapboxgl-popup {
        max-width: 400px;
        font: 12px/20px 'Helvetica Neue', Arial, Helvetica, sans-serif;
    }
</style>

<div id='map'></div>
<div id='features'>
    <section id='welcome' class='active'>
        <font color="#01cfff"><h3>Brewdog locations</h3><br>
        <img src="https://github.com/egil1978/egil1978.github.io/blob/master/brewdog2.png?raw=true" style="width:40%;height:40%;"><br>
    </section>
  
    <section id='section1'>
        <font color="#01cfff"><h4>1. Aberdeen</h4></font>
        <img src="https://www.brewdog.com/media/aheadworks/store_locator/28/image/Flagship_outside_2.jpg" style="width:70%;height:70%;"><br>
        <h6>In 2010, BrewDog opened their first bar in the owners home town of Aberdeen, 3 years after starting the company.</h6>
    </section>
  
  <section id='section2'>
      <font color="#01cfff"><h4>2. Glasgow & Edinburgh</h4></font>
        <h6>In 2011 BrewDog opened bars in Glasgow, Edinburgh, and a flagship London venue in Camden. In this year BrewDog also welcomed 5,000 new shareholders and raised £2.2m to continue growing their business.</h6>
    </section>
    
    <section id='section3'>
        <font color="#01cfff"><h4>3. Ellon</h4></font>
        <img src="https://www.brewdog.com/media/gene-cms/e/l/ellon_1.jpg" style="width:70%;height:70%;"><br>
        <h6>In 2012 they opened up a world class eco-brewery in Ellon which allowed them to grow revenues by 95%. In this year they also opened 6 new UK bars and launched Dead Pony Club.</h6>
    </section>
    
    <section id='section4'>
        <font color="#01cfff"><h4>4. York</h4></font>
        <img src="https://www.brewdog.com/media/aheadworks/store_locator/103/image/brewdog-york.jpg" style="width:70%;height:70%;"><br>
        <h6>In 2016, Elvis Juice was born! BrewDog also opened 9 new bars from Berlin to York.</h6>
    </section>
    
    <section id='section5'>
        <font color="#01cfff"><h4>5. Clerkenwell</h4></font>
        <img src="https://www.brewdog.com/media/aheadworks/store_locator/118/image/Hero_Clerkenwell_compressed_.jpg" style="width:70%;height:70%;"><br>
        <h6>BrewDog now have 9 bars in London, including one in Clerkenwell, just around the corner from the Geovation Hub. Once you’re thirsty from map making, it’s a great place to grab a nice pint!</h6>
    </section>
        
        <section id='section6'>
        <img src="https://snodnipper.github.io/os-zoomstack-masterclass/brand/ordnance_survey_white.png" style="width:40%;height:40%;"><br>
         <font size="1">Contains OS data © Crown copyright and database rights 2018</font>
    </section>
    
  </div>
<script>
mapboxgl.accessToken = '';
var map = new mapboxgl.Map({
    container: 'map',
    style: 'https://snodnipper.github.io/brewdog-map/style.json',
        maxZoom: 16,
        bearing: 0,
        center: [-3.276136, 54.831588],
        zoom: 6,
        speed: 0.5,
        pitch: 0
    });
    
  var chapters = {
    'welcome': {
        bearing: 0,
        center: [-3.276136, 54.831588],
        zoom: 5.93,
        speed: 0.5,
        pitch: 0
    },
    'section1': {
        bearing: 0,
        center: [-2.091948, 57.147483],
        speed: 0.5,
        zoom: 13,
        pitch: 0
    },
    'section2': {
        center: [-3.769885, 55.915518],
        bearing: 0,
        zoom: 8,
        speed: 0.5,
        pitch: 0
    },
    'section3': {
        bearing: 0,
        center: [-2.054593, 57.370377],
        zoom: 13,
        speed: 0.5,
        pitch: 0
    },
    'section4': {
        bearing: 0,
        center: [-1.091226, 53.955371],
        zoom: 13,
        speed: 0.5,
        pitch: 0
    },
    'section5': {
        bearing: 0,
        center: [-0.103039, 51.522837],
        zoom: 14,
        pitch: 60,
        speed: 0.5
    },
};
// On every scroll event, check which element is on screen
window.onscroll = function() {
    var chapterNames = Object.keys(chapters);
    for (var i = 0; i < chapterNames.length; i++) {
        var chapterName = chapterNames[i];
        if (isElementOnScreen(chapterName)) {
            setActiveChapter(chapterName);
            break;
        }
    }
};
var activeChapterName = 'baker';
function setActiveChapter(chapterName) {
    if (chapterName === activeChapterName) return;
    map.flyTo(chapters[chapterName]);
    document.getElementById(chapterName).setAttribute('class', 'active');
    if (document.getElementById(activeChapterName) != null) {
      document.getElementById(activeChapterName).setAttribute('class', '');
    }
    activeChapterName = chapterName;
}
function isElementOnScreen(id) {
    var element = document.getElementById(id);
    var bounds = element.getBoundingClientRect();
    return bounds.top < window.innerHeight && bounds.bottom > 0;
}
</script>
</body>
</html>


■ Commit the change.

When you view your map in a browser now you should see something like this (again, it may take a moment to update):



You can scroll through the story map by scrolling down the right-hand panel - the map will fly to different locations. You can also navigate the map using your mouse or trackpad. 

Can you think of a story map that you would like to create? If so, you can use this code as your template and add your own map style and your own content.

Problems?
We are here to help you guys!

Followed along but missing something? There are no smoke and mirrors here - we have uploaded the scripts we used to make the vector tiles here:
https://github.com/snodnipper/os-zoomstack-masterclass

Want more info about OS Open Zoomstack?
Head over to the webpage at os.uk/zoomstack
Brewdog Inspired Map Colour Palette






Need more colours?  Pop along to the Google Material UI page: https://material-ui.com/style/color/.








