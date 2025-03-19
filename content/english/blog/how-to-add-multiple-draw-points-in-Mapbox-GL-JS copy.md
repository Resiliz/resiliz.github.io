---
title: "How to Add multiple draw points in Mapbox GL JS in Different Styles"
meta_title: "How to add multiple draw points in Mapbox GL JS in Different Styles"
description: "Learn how to create custom draw points with different styles in Mapbox GL JS. A step-by-step guide to implementing multiple icon styles for map markers."
date: 2019-04-11T05:00:00Z
image: "/images/mapbox.jpg"
categories: ["Application", "Data"]
author: "Viraj"
tags: ["mapbox", "javascript", "mapping", "tutorial"]
draft: false
---

Mapbox GL JS is a powerful tool for creating interactive maps. In this tutorial, I'll show you how to create a custom toolbox for map drawing with different styles of draw points - something that isn't immediately obvious from the official documentation.

## The Challenge

I needed to:
1. Select a drawing tool from a toolbox
2. Add markers to multiple locations
3. Switch to another tool and repeat with different marker styles

After searching through documentation and [asking on StackOverflow](https://stackoverflow.com/questions/55626051/is-there-a-way-to-draw-different-icons-for-draw-points-in-mapbox-gl-js), I developed my own solution.

## Step 1: Loading Custom Images

First, you need to add your custom marker images to the map. Add this code when the map loads:

```javascript
map.on('load', function() {
    // Load trough icon
    map.loadImage('{{asset("/images/icons/ic_feature_map_trought@1x.png")}}', function(error, image) {
        if (error) throw error;
        map.addImage('trought', image); // giving a name to image
    });
    
    // Load windmill icon
    map.loadImage('{{asset("/images/icons/ic_feature_map_windmill@1x.png")}}', function(error, image) {
        if (error) throw error;
        map.addImage('windmill', image);
    });
});
```

## Step 2: Handling Tool Selection

Set up event handlers for your toolbox selections:

```javascript
$(".dropdown-menu a").on('click', function(event) {
    // Prevent event bubbling
    event.stopPropagation();
    event.stopImmediatePropagation();
    
    // Get selected tool name
    var menu = event.target.innerHTML;
    menu = menu.replace(/\s/g, '').toLowerCase();
    
    switch(menu) {
        case 'trough':
            draw.changeMode('draw_point');     // Set draw mode
            selected_mode = 'draw_point';      // Store mode globally
            selectedTool = "trought";          // Store tool type
            break;
        case 'windmill':
            draw.changeMode('draw_point');
            selected_mode = 'draw_point';
            selectedTool = "windmill";
            break;
        default:
    }
    
    // Maintain draw mode on map interactions
    map.on('draw.modechange', e => {
        draw.changeMode(selected_mode);
    });
});
```

### Important Variables:
- `selected_mode`: Stores the current drawing mode
- `selectedTool`: Stores the currently selected marker type

## Step 3: Adding Markers to the Map

Handle map clicks to add markers:

```javascript
map.on('click', function (e) {
    // Create unique ID for each marker
    var imageId = e.lngLat.lng + e.lngLat.lat + "";
    
    // Add new layer with custom marker
    map.addLayer({
        "id": imageId,
        "type": "symbol",
        "source": {
            "type": "geojson",
            "data": {
                "type": "FeatureCollection",
                "features": [{
                    "type": "Feature",
                    "geometry": {
                        "type": "Point",
                        "coordinates": [e.lngLat.lng, e.lngLat.lat]
                    }
                }]
            }
        },
        "layout": {
            "icon-image": selectedTool,
            "icon-size": 1
        }
    });
});
```

## Key Points to Remember

1. Each marker needs a unique ID to prevent conflicts
2. The `icon-image` property must match the name given in `map.addImage()`
3. Coordinates are specified in [longitude, latitude] format
4. The `draw.modechange` event handler is crucial for maintaining the selected draw mode

## Troubleshooting

If you encounter issues:
- Verify that your image paths are correct
- Check that image names match between `addImage()` and `icon-image`
- Ensure unique IDs for each marker layer
- Confirm that your coordinates are in the correct format

## Next Steps

You can enhance this implementation by:
- Adding marker removal functionality
- Implementing marker drag-and-drop
- Adding custom popup information
- Storing marker data for later use

Feel free to ask questions in the comments below!