# 2.2.1 Cycle 1

## Design

### Objectives

In this cycle my aim was to create my basic HTML5 webpage using JavaScript. I then went on to get a background loaded and for sprites to be loaded onto the page, and respond to player on the keyboard. I then installed Phaser.js which is the game engine library that I will be using, which provides a physics engine called Matter.js and it also is efficient at drawing and receiving inputs from the user using functions.&#x20;

* [x] Get website to load and work on localhost:3000
* [x] Basic working controls
* [x] Getting the page to fit the screen
* [x] Get keyboard inputs to move the sprite

### Usability Features

### Key Variables

| Variable Name               | Use                                                                                                |
| --------------------------- | -------------------------------------------------------------------------------------------------- |
| player                      | This is the variable that stores the properties of the spaceship sprite.                           |
| update                      | This function is the function that updates each frame to update the position of the sprite.        |
| W/A/S/D, up/left/down/right | These are the variable that store the key bindings for movement of the character.                  |
| create                      | This function is called at the start of the game to initialise all the variables and sprites etc.  |
| velocity(.X)(.Y)            | This is the speed at which the player starts to move in a direction when a key is pressed.         |

### Pseudocode

```
procedure create
    load map
    load player
    load keys
end procedure

procedure update
    if keys.A is pressed
        velocityX = -300
    else if keys.D is pressed
        velocityX = 300
        
    player.rotation = player.angle + π / 2
end procedure
```

## Development

### Outcome

The PlayScene.ts file is where the main portion of the game is and it is where the different planets will be added later on, and is where I have imported the controls from my components folder and added it to the update function so that they update each frame. I've made it so that the controls are in a separate file in my components folder so that I'm modularising my program which makes for easier to debug code, and also makes it more readable. This means that each function is decomposed into smaller tasks and makes it more structured.

&#x20;In the controls file I have added a simple function that detects when my arrow keys are pressed and what that should do to the sprite as a result of them pressing that key. Pressing the keys will move the sprite around the screen, at a comfortable speed as to be easy to control the sprite.&#x20;

{% tabs %}
{% tab title="PlayScene.ts" %}
{% code title="playScene.ts" %}
```typescript
class TestScene extends Phaser.Scene {
 	player: Phaser.GameObjects.Sprite;
 	cursors: any;

 	constructor() {
     super({
 	key: 'TestScene'
         });
     }

 	preload() {
 		this.load.tilemapTiledJSON('map', '/assets/tilemaps/desert.json');
 		this.load.image('Desert', '/assets/tilemaps/tmw_desert_spacing.png');
 		this.load.image('player', '/assets/sprites/mushroom.png');
 	}

 	create() {
 		var map:Phaser.Tilemaps.Tilemap = this.make.tilemap({ key: 'map' });
 		var tileset:Phaser.Tilemaps.Tileset = map.addTilesetImage('Desert');
 		var layer:Phaser.Tilemaps.StaticTilemapLayer = map.createStaticLayer(0, tileset, 0, 0);

 		this.player = this.add.sprite(100, 100, 'player');
 		this.cursors = this.input.keyboard.createCursorKeys();

 		this.cameras.main.setBounds(0, 0, map.widthInPixels, map.heightInPixels);
                this.cameras.main.startFollow(this.player, false);
 	}

 	update(time: number, delta:number) {
 		this.player.angle += 1;
 		if (this.cursors.left.isDown) {
 			this.player.x -= 5;
 		}
 		if (this.cursors.right.isDown) {
 			this.player.x += 5;
 		}
 		if (this.cursors.down.isDown) {
 			this.player.y += 5;
 		}
 		if (this.cursors.up.isDown) {
 			this.player.y -= 5;
 		}
 	}
 }

 export default TestScene; 
```
{% endcode %}
{% endtab %}

{% tab title="Main.ts" %}
{% code title="main.ts" %}
```typescript
import 'phaser';

// game scenes
import Planet_1 from './scenes/PlayScene';

const config:GameConfig = {
    type: Phaser.AUTO,
    parent: 'game',
    width: window.innerWidth, // the width and height is scaled to the window
    height: window.innerHeight,
    resolution: 1, 
    backgroundColor: "#fcfff2", // this is a cream background
    physics: {
        default: 'matter',
        matter: {
            gravity: {
                y: 0
            },
            debug: true // this shows bounding boxes around sprites and bodie
        }
    },
    scene: [
      Planet_1
    ]
};

export class Game extends Phaser.Game {
    constructor(config: GameConfig) {
        super(config);
    }
}

export const game = new Phaser.Game(config);

```
{% endcode %}
{% endtab %}

{% tab title="Index.html" %}
{% code title="index.html" %}
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Phaser3 - ES6 - Webpack</title>
    <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
    <meta name="HandheldFriendly" content="True">
    <meta name="MobileOptimized" content="320">
    <meta http-equiv="cleartype" content="on">
    <meta name="format-detection" content="telephone=no">
    <style>
        html,
        body {
            margin: 0;
            padding: 0;
        }
    </style>
</head>

<body>
    <div id="content"></div>
<script type="text/javascript" src="./dist/vendor.bundle.js"></script><script type="text/javascript" src="./dist/bundle.js"></script></body>
</html>
```
{% endcode %}
{% endtab %}
{% endtabs %}

Using the package.json you are able to install all libraries and modules, by running **`npm install`**, and then you can run it by **`npm run dev`** .This will use webpack to deploy the webpage to localhost//:3000, where I am able to develop the game, and then when I want to fully bundle the game I can run the command **`npm run deploy`** as this will fully package the game, and make it a smaller file size so that it loads faster and is less demanding on the computer.

### Challenges

Description of challenges

* Getting the webpage to load after installing Phaser.js
* Getting working controls from a different file

## Testing

Evidence for testing

### Tests

| Test | Instructions     | What I expect                                                         | What actually happens                                                                            | Pass/Fail |
| ---- | ---------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------- |
| 1    | Run code         | Page to load and be rendered to fit the screen.                       | Page loads after compiling and all renders, whilst fitting to screen.                            | Pass      |
| 2    | Press arrow keys | Arrow keys allow basic movement of sprite on page.                    | Nothing happens.                                                                                 | Fail      |
| 3    | Press WASD keys  | WASD allows movement of the player in the same fashion to arrow keys. | The WASD keys allow me to move the sprite, but would like to be able to use the arrow keys too.  | Pass      |

I went back to my control.ts file and realised that I didn't implement the cursors variable to be able to enable the cursors, so I went back and fixed this in the cursors.ts file and the create() function in the PlayScene.ts file:

{% tabs %}
{% tab title="Cursors.ts" %}
{% code title="cursors.ts" %}
```typescript
import { cursors } from "../../scenes/PlayScene";

export default function createCursorKeys(keys, player){
    if (keys.A.isDown || cursors.left.isDown) {
        player.setVelocityX(-300);
      } else if (keys.D.isDown || cursors.right.isDown) {
        player.setVelocityX(300);
      }
    
      if (keys.W.isDown || cursors.up.isDown) {
        player.setVelocityY(-300);
      } else if (keys.S.isDown || cursors.down.isDown) {
        player.setVelocityY(300);
      }
}
```
{% endcode %}
{% endtab %}

{% tab title="PlayScene.ts" %}
{% code title="PlayScene.ts" %}
```typescript
create() {
	var tileset:Phaser.Tilemaps.Tileset = map.addTilesetImage('sky');
	var layer:Phaser.Tilemaps.StaticTilemapLayer = map.createStaticLayer(0, tileset, 0, 0);*/

	let map = this.physics.add.image(innerWidth/2, innerHeight/2, 'sky');
	cursors = this.input.keyboard.createCursorKeys()

	player = this.physics.add.sprite(100, 100, 'player');
	keys = this.input.keyboard.addKeys('W,A,S,D');

	player.setScale(0.25);
	map.setScale(2);
	}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Evidence

In this game I'm able to use the arrow keys to move around, and the sprite points in the direction of travel.&#x20;

![Photo of the spacecraft in the sky](<../.gitbook/assets/Screenshot 2022-06-07 at 12.12.25.png>)

### Tests

| Test | Instructions     | What I expect                                                         | What actually happens                                                                            | Pass/Fail |
| ---- | ---------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------- |
| 1    | Run code         | Page to load and be rendered to fit the screen.                       | Page loads after compiling and all renders, whilst fitting to screen.                            | Pass      |
| 2    | Press arrow keys | Arrow keys allow basic movement of sprite on page.                    | The arrow keys allow for the movement of the player.                                             | Pass      |
| 3    | Press WASD keys  | WASD allows movement of the player in the same fashion to arrow keys. | The WASD keys allow me to move the sprite, but would like to be able to use the arrow keys too.  | Pass      |

### What next?

* I'd like it so that the player moves more naturally when you let go of a key, as this will make the game feel more natural to play and more emersive.&#x20;
* Realisitc floating physics. The spacecraft wouldn't just stop dead in real life, it would carry on floating a bit after.&#x20;
