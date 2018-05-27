# leaflet-painting
Recipe for tiling large images, and code to use them

The "zoomify" feature is quite well-known among the community working in this sector, but there's lots of variations, old scripts don't work now etc. There are many museums out there that don't know that this is possible. I hammered out a recipe to get it done quickly and at efficient size. And code to use it. So sharing it here.

[See a live implementation here.](http://nikhilvj.cu.cc.cp-48.bigrockservers.com/files/leaflet-painting/leaflet-painting.html)

Artwork Source: an Indian painting from the Kishengarh school of miniature paintings from Rajasthan, hosted at Nation Museum, New Delhi. Uploaded on Wikimedia Commons in high-res: [Radha and Krishna in the boat of love](https://commons.wikimedia.org/wiki/Template:Radha_and_Krishna_in_the_boat_of_love_-_Google_Art_Project), from 

Note: These instructions are for Ubuntu machines. Inviting inputs for adapting to Windows systems.

### Feature: Permalinks
- You can save a permalink to what you're seeing by copying the addressbar URL or click a location and right-click > copy the link.
- Share that with others to show them the exact portion of the painting you're talking about. Example: who're those [people on the hill](http://nikhilvj.cu.cc.cp-48.bigrockservers.com/files/leaflet-painting/leaflet-painting.html#6/61.449/-42.462)?; is she carrying a [small blackboard](http://nikhilvj.cu.cc.cp-48.bigrockservers.com/files/leaflet-painting/leaflet-painting.html#6/14.115/-15.721)?
- Thanks to the [leaflet-hash](https://github.com/mlevans/leaflet-hash) plugin.

### Steps
- Download this repo, unzip it to a folder and open the folder in Terminal (Ctrl + Alt + T).
- Download or obtain your image into this folder. Let's called it 'bigimage.jpg'.
- Run these commands in Terminal one by one:
```
python gdal2tiles-multiprocess.py -l -p raster bigimage.jpg tiles
cd tiles
find -name '*.png' -print0 | xargs -0 mogrify -format jpg
find -name '*.png' -print0 | xargs -0 rm
cd ..
```
- A folder 'tiles' will be created. (or a different name if you changed the command)
- Edit the `config.js` file to update the variables. Explained below. 
- After saving `config.js`, open `leaflet-painting.html` in your web browser. You should see your painting / large image in the same way as the example linked above.

### Explanation
- `find -name '*.png' -print0 | xargs -0 mogrify -format jpg` : This creates low-size .jpg versions of all the tiled images.
- `find -name '*.png' -print0 | xargs -0 rm` : This deletes the .png tiles.
- `gdal2tiles-multiprocess.py` : From the [gdal2times-leaflet](https://github.com/commenthol/gdal2tiles-leaflet) repo.

#### Variables in `config.js`:
- `tilePath` : name of the tiles folder
- `maxZoom` : see what is the highest number folder in the tiles folder
- `attributionHTML` : This HTML-encoded line will come at the bottom right of the webpage. Please cite the origins of the image you have tiled, link back to the source.
- `extension` : whether the tiles are in '.jpg' format or something else. (if you didn't apply the bulk png-to-jpg conversion commands above, they will likely be .png)

### What more can we do with this?
Check out [StoryMapJS for Images by Knightlab](https://storymap.knightlab.com/gigapixel/)
