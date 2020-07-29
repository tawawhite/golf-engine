# GoLF Engine
the GoLang Fantasy Engine (GoLF Engine) is a fantasy engine. It draws inspiration from projects fantasy console projects 
like pico-8 and tic-80. Like those projects it is designed to be a fairly restrictive game creation tool focused on creating
a more managble game creation enviroment the a fully featured game engine would offer. This project differs from those
projects in a few importan ways however. First, while the the golf engine strives to present 'retro' style restrictions, these
restrictions don't apply to the code. The code that is produced by golf is compiled WASM meaning your performance budget is
much larger with the GoLF engine than it would be with other fantasy consoles. It also has no limits on the size of your
code base. GoLF also uses golang, which is a compiled language rather than a scripting language like lua. Finally unlike other
fantasy consoles which have tools like a sprite editor and a music editor build in, GoLF does not yet provide these tools.
This is more because of my limited time as the developer rather than any technical reason, however for these reasons I have
chosen to refere to my GoLF as an fantasy engine, rather than a fantasy console (technically though a fantasy framework might be
more apropriate). Despite these differences I hope you will still enjoy playing and making games with golf.

# Getting Started

# GoLF and WASM
WASM is an exciting new technology and brings a lot of new power and many new programing languages to the web. GoLF was designed
from the groud up to work with WASM. 

# GoLF API

NewEngine(updateFunc func(), draw func()): creates a golf engine instance and returns a pointer to the engine. The golf engine is
the main object used to perform most of the golf functions

engine.Run(): starts the game engine running. Once this is run the update function will be called 60 times a second and the draw function will be called 60 times a second.

engine.Frames(): returns the number of frames that have passed since the game engine was started. This count includes
the startup animation frames. The startup animation is 254 frames meaning the first frame that the update/ draw function 
will be called is frame 255.

engine.DrawMouse(style int): sets the draw style for the mouse indictor.
  * 0 = no mouse cursor is drawn
  * 1 = a mouse arrow is drawn
  * 2 = a hand cursor is drawn
  * 3 = a cross cursoe is drawn

engine.BG(): returns the current clear color of the screen. The returned color is a golf.Col value.

engine.SetBG(color golf.Col): sets the clear color of the screen.

engine.Cls(): fills the screen with the BG color.

engine.Camera(x, y int): Changes the X, Y coordinates of the camera. This value is then subtracted from the
X, Y coordinates of all future drawing calls. This is useful for moving the panning the screen around.

engine.Rect(x, y, w, h float64, col golf.Col): Draw an empty rectangle outline with the specified draw color.

engine.RectFill(x, y, w, h float64, col golf.Col): Draw a filled rectangle with the specified draw color.

engine.Line(x1, y1, x2, y2 float64, col golf.Col): Draw a line from point (x1, y1) to (x2, y2). The line is drawn with
the specified color.

engine.Circ(xc, yx, r float64, col golf.Col): Draw a circle outline with center at point (xc, yc) with radius r.
The outline is drawn with the specified color.

engine.CircFill(xc, yc, r float64, col golf.Col): Draw a filled circle with center at point (xc, yc) with radius r.
The circle is drawn with the specified color

engine.Clip(x, y, w, h int): clips all future draw functions with upper left corner at point (x, y) and with w and heigh h.

engine.RClip(): resets the screen clipping so that no screen pixels are clipped.

engine.Pset(x, y float64, col golf.Col): Sets the pixel on the screen at point (x, y) to the color col.

engine.Pget(x, y float64): Gets the color currently set at screen pixel (x, y).

engine.PalA(pallet golf.Pal): sets the first pallet.

engine.PalB(pallet golf.Pal): sets the second pallet.

engine.PalGet(): returns the first and second pallets that are currently set.

engine.Btn(key golf.Key): returns true if the given key is being held on this frame.

engine.Btnp(key golf.Key): returns true if the given key was first pressed on this frame.

engine.Btnr(key golf.Key): returns true if the given key was released on this frame.

engine.Mbtn(key golf.MouseBtn): returns true if the given mouse key is being held on this frame.

engine.Mbtnp(key golf.MouseBtn): returns true if the given mouse key was first pressed on this frame.

engine.Mbtnr(key golf.MouseBtn): returns true if the given mouse was released ont his frame.

engine.LoadMap(mapData [0x4800]byte): load the map data into memory.

engine.Map(mx, my, mw, mh int, dx, dy float64, opts ...SOp): Draws the map data onto the screen witht he left coordinate 
at screen point dx, dy. mx and my are the map coordinates in tiles and dw and mh are the map size in tiles. opts are optional and change how each individual map tile is drawn.

engine.Mset(x, y, t int): sets the map tile to sprite number t at the map coordinate (x, y)

engine.Mget(x, y int): returns the sprite index of the tile a the map coordinate (x, y)

engine.LoadSprs(sheet [0x3000]byte): load the sprite sheet data into memory.

engine.LoadFlags(flags [0x200]byte): load the sprite flags into memory. Each sprite in the sprite sheet has 1 byte 
(or 8 flags) associated with is that can be set and then later checked. The meaning of each of these flags is 
totally up to the needs of the progarmmer.

engine.Spr(n int, x, y float64, opts ...SOp): draw sprite number n at screen position x, y. opts are optional and change
how the sprite is drawn on screen. the sprite sheet is broken up into 8x8 areas that are then number from the top left 
to the bottom right. Usually the first 8x8 sprite is not used as this sprite is drawn as a transparent tile when used on the map screen.

engine.SSpr(sx, sy, sw, sh int, dx, dy float64, opts ...SOp): a more general version of the spr function. It draws a sprite from an
abitrary spot on the sprite sheet with abitrary size to the screen. sx and sy are the pixel coordiantes of upper left corner of the
sprite on the sprite sheet. sw and sh are the sprites withd and height respectivly. dx and dy are the screen coordinates that
the sprite is drawn to. opts is optional and changes how the sprite is drawn on screen.

engine.Fget(n, f int): returns flag number f associated with sprite number n.

engine.Fset(n, f int, s bool): sets the flag number f for sprite n to the same value as s.

engine.FgetByte(n int): returns the full byte assocated with sprite number n.

engine.FsetByte(n int, b byte): sets the full byte assocated with sprite number n to the value of b.

golf.SOp: this structure represents a list of options that can be passed to a sprite to change how it is drawn.
  * FH: flip the sprite horizontally.
  * FV: flip the sprite vertically.
  * TCol: set the sprites tranparency color.
  * PFrom & PTo: change the sprites pallet. colors number n in PFrom is converted to color number n in PTo.
  * W: width of the sprite in tiles to read from the spritesheet. (e.g. W: 2 is 16 pixels in width).
  * H: height of the sprite in tiles to read from the spritesheet. (e.g. H: 2 is 16 pixels tall).
  * SW: the amount to scale the width of the sprite. default value is 1 or no scaling.
  * SH: the amount to scale the height of the sprite. default value is 1 or no scaling.
  * Fixed: if this is set to true then the sprite ignores the camera x & y when draing. Useful for UI.

engine.Text(x, y float64, text string, opts ...TOp): draws the text on screen at point (x, y), all text is converted to the golf
engines internal font which is all upper case. There are also several sequences that are converted in to golf emojis. escaped sequences are listed bellow. opts are optional and modify how the text is drawn.
  * (<) left button
  * (>) right button
  * (^) up button
  * (v) down button
  * (x) x button
  * (o) o button
  * (l) l shoulder button
  * (r) r shoulder button
  * (+) + button
  * (-) - button
  * :) smily face
  * :( frowny face
  * x( angry face
  * :| meh face
  * =[ boxy face
  * |^ up arrow
  * |v down arrow
  * <- left arrow
  * -> right arrow
  * $$ pound symbol
  * @@ small black dot
  * <| speaker symbole
  * <3 white heart
  * <4 black heart
  * +1 plus one symbole
  * -1 minus one symbole
  * ~~ the pi symbole
  * () tall black dot
  * [] dark square
  * :; dither pattern
  * ** start symbole
  note: if you need to draw one of these patterns without it being drawn as an emoji you can use the '^' symbole to escape.
  the pattern. (e.g. ^** will be drawn as two asterix characters rather than a star)

engine.TextL(text string, opts ...TOp): draws text in the upper left hand corner of the screen. Each time TextL called a new
line is added.

engine.TextR(text string, opts ...TOp): draws text in the upper right hand corner of the screen. Each time TextR is called
a new line is added.

golf.Col: a golf color. there are 8 colors ranging from Col0 to Col7. The first 4 colors map to pallet A and the last
four map to pallet B.

golf.Pal: a golf pallet. There are 16 available pallets (Pal0 to Pal15). These can be used to give you game a unique feel/ look.

# The GoLF memory map
the golf engine is designed to be hackable as was as to maintin a retro feel which developing. In order to achieve this the
golf engine uses a block of memory that can be accessed using the RAM member variable of a golf engine instance. The engine.RAM
contains all the data for the sprite memory, the map memory, the screen buffer and even the keyboard state. If there is anything
that the API does not expose you can probable read or write that data from the engine memory. In order to help you with this 
you may with to look at the memoryMap.go file. This contains a list of all the memory addresses used by golf to run the engine.

# The GoLF color pallet
part of golf's goal as a engine is to all the creation of games with a unique and recognizable style. Visual style is an important
aspect of this and so the engine uses a unique method for drawing colors on screen. Each pixel can only be one of 4 colors but it
can also belong to either of 2 pallets. This means that you can effectively draw up to 8 colors on screen at a time. There is no
limit to how often you can change these pallets either allowing you to create uinque and interesting games with creative graphics.
This system may seem restrictive but that is by design. The golf engine firmly believes that restrictions foster creativity and
also help developers finish projects which are major goals of this engine. The 16 available 4 color pallets are shown bellow 
(in order from top Pal0 to bottom Pal15). Play around and find unique and interesting pallet combinations!

# Developing your game

# Releasing your game

### TODO
---
* Build out the readme so there is good documentation for the API
* Change Cls to take a Col so we dont need to store a bg color (Update the README)
* Create an example golang server for using this framework for playing games online
* Add instructions for installing and playing the golf examples
* Test the golf toolkit on a windows machine

### DONE
---
* Fix mouse transparency [x]
* Clean up startup animation code [x]
* Use all 8 colors on the loading animation to make the fading nicer [x]
* Fix text alpha [x]
* Show error on server automatic build in golf toolkit [x]
* Test new map code [x]
* chang the way the map code works so that it uses the new color atlas and works with the new sprite import code [x]
* give X, Y coords on unknown color in image [x]
* Change build code to use hex instead of bin to make smaller generated files [x]
* Test the new sprite importing code [x]
* Inject draw.js to make golf require less dependancies [x]
* Default to the black and gray pallets [x]
* Fix color pallet swapping caused by the startup anim (allow the user to set the pallets) [x]
* Template generation code should create arrays rather than slices [x]
* Add last 4 color pallets (Black White Red Blue pallet) [x]
* Add scale width and scale height opts to the text drawing [x]
* Add startup animation when you start the game [x]
* Fix the color pallet [x]
* Add function to load sprite flag data [x]
* Change the sprite functions from using ints to using floats [x]
* Change the SprOpts and TextOpts to be Sop and Top to make the API more terse [x]
* Add a save cart data function (save to a browser cookie) [x]
* Read cart data function (read from the browsers cookies) [x]
* Add readme to go examples [x]
* Create github for golfExamples games [x]
* Check out Faith and Faith 2 itch.io [x]
* Better error message when building fails in the golf_toolkit [x]
* Throw a waning when rebuilding on refresh fails [x]
* Add in command to modify the config file [x]
* Fix the gaps that start to form in the map file as you scale up (only a problem with scale w/h) [x]
* Make the map tool respect SprOpts, or make a separete MapOpts for pallet swaping [x]
* Make the build tool work with csv map files [x]
* Make golf_config not a hidden file [x]
* Use \n chars to seperate golf config files rather than commas. this will make it easier to read and use [x]
* restructure code. Delete bad old code. Make the repo nice and clean [x]
* Add function to load map data (reverse order) [x]
* Make tools for importings maps. [x]
  * Import CSV as map [x]
  * Import image as map [x]
* restructure init so that starting code is nicer (e.g. js folder, assets folder etc.) [x]
* oo for the filled in circle is a bad letter sequence. Picke a sequence that is less common in regular words [x]
* Create csv format for the flags (Probably just 1's and 0's) [x]
* Import sprite flag csv (Should be pretty easy) [x]
* Create the Golf ToolKit [x]
* Add about and help commands [x]
* Add clear and !! command to the go toolkit [x]
* Fix Filled Circles so the look the same as hollo ones [x]
* Code automatically recompiles on browsers refresh [x]
* Start and Stop server with golf_toolkit (nonblocking) [x]
* Add build and init to golf_toolkit [x]

### TODO long term
---
* Make it more fantasy console like
  * add a golf terminal that runs in the browser
  * build with golf engine so it has the same feel
  * sprite editor in the golf terminal
  * map editor and viewer
  * sprite flag editor 
* Sound? (I still have 20k memory for this)
* Add interpreted scripting language to make it more aproacable and to prevent golang from being an install requirement
* Let text use multips options with {} syntac to start and end option sections
* add vertical and horizontal flip to the text functions