Tiled:
Create a tileset
add custom property to tile
save as json

Create a tilemap and import that tileset
Embed that tileset (little tiny button underneath tileset in ui)
Save as json

tileset name: 'test-tileset'
tilemap name: 'test-map'
need to look in the test-map.json file to find this stuff

the tileset file won't actually be imported by phaser, it will be embedded in the tile map and that's how we'll get the data

this.load.image('kenney-platformer-redux-ground', 'assets/tilesets/kenney-platformer-redux-ground.png');
this.load.tilemapTiledJSON('test-map', 'assets/maps/test-map.json');

const tilemap = this.add.tilemap('test-map');
const tileset = tilemap.addTilesetImage('test-tileset', 'kenney-platformer-redux-ground') // this should really be named createTileset since it creates a tileset given the key of the embedded tileset and the phaser key of the associated image
tilemap.createStaticLayer('Tile Layer 1', tileset);  // need to pass in the actual tileset instance