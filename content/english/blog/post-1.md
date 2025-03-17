---
title: "How to add multiple draw points in Mapbox GL JS in Different Styles"
meta_title: "How to add multiple draw points in Mapbox GL JS in Different Styles"
description: "How to add multiple draw points in Mapbox GL JS in Different Styles"
date: 2022-04-04T05:00:00Z
image: "/images/image-placeholder.png"
categories: ["Application", "Data"]
author: "John Doe"
tags: ["nextjs", "tailwind"]
draft: false
---

Mapbox GL is an awesome tool for map drawings. I tried to create a custom toolbox for map drawing. In the documentation of the Mapbox, I couldn’t find a way to replace default draw points of the Mapbox and draw with my own style of draw points. The scenario is like this. First, select a drawing tool from the toolbox and add it to the map, then add it to the several locations. After that select another tool and do the same. Look the following picture to clear your mind.


So I searched through the internet and asked a question on StackOverflow also.

https://stackoverflow.com/questions/55626051/is-there-a-way-to-draw-different-icons-for-draw-points-in-mapbox-gl-js

But couldn’t find a solution. So I tried my own way to accomplish this case and at last, it was succeeded. So I would like to share the way I did it.

In the selection from the toolbox, I here pass the mode to draw function. And assign the selected mode and selected tool to global variables.

First, external images that you are planning use should be added to the map.

map.on(‘load’, function() {
 map.loadImage(‘{{asset(“/images/icons/ic_feature_map_trought@1x.png”)}}’, function(error, image) {
if (error) throw error;
map.addImage(‘trought’, image); // giving a name to image
});
map.loadImage(‘{{asset(“/images/icons/ic_feature_map_windmill@1x.png”)}}’, function(error, image) {
  if (error) throw error;
    map.addImage(‘windmill’, image);
  });
});
Next, I’ll move to the tool selection events. In the selection from the toolbox, Check the following code.

Fixture 2
$(“.dropdown-menu a”).on(‘click’, function(event){
  event.stopPropagation();
  event.stopImmediatePropagation();
  var menu = event.target.innerHTML;
  menu = menu.replace(/\s/g, ‘’).toLowerCase();
  
  switch(menu) {
    case ‘trough’:
    draw.changeMode(‘draw_point’); // 1.
    selected_mode = ‘draw_point’; // 2.
    selectedTool = “trought”; // 3.
    break;
  case ‘windmill’:
    draw.changeMode(‘draw_point’);
    selected_mode = ‘draw_point’;
    selectedTool = “windmill”;
    break;
  default:
  }
  
 map.on(‘draw.modechange’, e => { // 4.
    draw.changeMode(selected_mode);
 });
});
Look on the code and there are commented lines which are numbered. So here I described each number below.

Change the mode to draw point because I need to draw a point.
I have kept the selected mode in a global variable for post usage.
Assigned the selected tool to a global variable for post usage.
For every click event on the map, draw mode is changed to the simple_select mode by default. So it should be reconfigured to the selected mode.
Now I have selected a tool and a draw mode also. So now when I clicked on maps, the thought icon should be drawn on the map. To do that, the map click should be added to the system. Look at the following code.

map.on(‘click’, function (e) {
  var imageId = e.lngLat.lng + e.lngLat.lat + “”;
  map.addLayer({
    “id”: imageId,
    “type”: “symbol”,
    “source”: {
      “type”: “geojson”,
      “data”: {
      “type”: “FeatureCollection”,
      “features”: [{
        “type”: “Feature”,
        “geometry”: {
          “type”: “Point”,
          “coordinates”: [e.lngLat.lng, e.lngLat.lat]
        }
      }]
    }
   },
   “layout”: {
     “icon-image”: selectedTool,
     “icon-size”: 1
   }
   });
});
For each click new layer added to the map and this id key should be unique, otherwise, an error is occurring saying that id is already used, to prevent that you should give unique key here. and give the coordinates to identify where to add this icon in the map. We want to add it to the clicked position so give the clicked position latitude and longitudes. in the layout section, give previously selected tool name to the icon image section and it will load the correct image by name that was given in Fixture 2.

That’s it. If any unclear place, feel free to ask. Thanks in advance.