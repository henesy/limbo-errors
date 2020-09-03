# Common Limbo Compiler Errors

### tttfs.b:93: 'game' of type Game cannot be qualified with ->

In this case Game is an adt type and game is defined as `game: Game`. 

What Limbo wants, is game to be defined as `game: ref Game`. 

### link typecheck main->init() 0/4244b354

Something similar to this means that your init() function's signature does not match
what is expected. 

For a program intended to be run from the shell, etc. the proper signature is similar to:

	MyModuleName: module {
		init: fn(nil: ref Draw->Context, argv: list of string);
	};

	init(nil: ref Draw->Context, argv: list of string) {
	...
	}

Typically I see this when I drop the `ref` in `ref Draw->Context` somewhere. 


