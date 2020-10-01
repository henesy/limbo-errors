# Common Limbo Compiler Errors

### cannot be qualified

```
tttfs.b:93: 'game' of type Game cannot be qualified with ->
```

In this case Game is an adt type and game is defined as `game: Game`.

What Limbo wants, is game to be defined as `game: ref Game`.

### link typecheck

```
link typecheck main->init() 0/4244b354
```

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


### are not compatible

```
poly.b:66: fn(a: T, b: T): int and fn(a: Integral, b: Integral): int are not compatible wrt eq
poly.b:66: function call type mismatch (fn[T](x: T, a: array of T): array of T vs fn[T](nil: ref Integral, nil: array of ref Integral): array of T)
```

The `eq()` function has the signature:

```
eq:		fn(a, b: Integral): int;
```

Note the absence of the `ref` keyword.

A satisfying implementation would resemble:

```
Integral: adt {
	n: int;
	eq:		fn(a, b: ref Integral): int;
	String:	fn(x: self ref Integral): string;
};

Integral.eq(a, b: ref Integral): int {
	return a.n == b.n;
}
```
