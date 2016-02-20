# What am I even doing


## Clients
Each client pushes _commands_ to the game, and receives _events_ back. These
are not 1-1, several events may come back for a single command.

Should there be a 'capture' event which means 'please get the next command'?

Websocket client does thin translation between wire format (JSON?) and objects
to push/pull from channels. Termbox client is a standard terminal game client.

## Screens
At any given time, the game 'knows' what macro state is in by keeping a primary
'screen' active. This 'screen' can be the standard game viewport, the title
screen, the menu, etc.

Clients should know what kinds of commands can be sent to different screens.
Commands are a pretty literal description of the keypresses that user entered,
so that keybindings can be stored in one place (instead of being for a single
client.)

A client should know how to render a screen when it is asked to. 

## Game loop
```
screens.push(titlescreen)
commands = inchannel
events = outchannel
commands <- start!

forever:
  if screens.empty()
    quit
  command = <-commands
  end?, newscreen = screen.do(command, events)
  if end?:
    screens.pop()
    if newscreen:
      screens.push(newscreen)
    events <- newscreenEvent(newscreen)
```

Probably the gameworld or some global context needs to be given to each screen
so it can figure out what is going on.

Things
======
- simple game loop for starting + switching to mapscreen w/ termbox-go client
