[![NPM][npm]][npm-url]
[![DevDependencies][deps-dev]][deps-dev-url]

# ts-pfy

Transforms Node.js-style callback-based function into a Promise-based.

This implementation is TypeScript-friendly.

Returns a function that will wrap the given `nodeFunction`. Instead 
of taking a callback, the returned function will return a promise whose 
fate is decided by the callback behavior of the given node function. 
The node function should conform to node.js convention of accepting 
a callback as last argument and calling that callback with error 
as the first argument and success value on the second argument.

If the `nodeFunction` calls its callback with multiple success values, 
the fulfillment value will be the first fulfillment item.

Setting `multiArgs` to `true` means the resulting promise will always 
fulfill with an array of the callback's success value(s). This is needed 
because promises only support a single success value while some callback 
API's have multiple success value. The default is to ignore all but 
the first success value of a callback function.

If you pass a `thisArg`, the `nodeFunction` will be called as a method 
on the `thisArg`.

## Install

```
npm install --save ts-pfy
```

## Usage

```ts
function promisify(
	nodeFunction: Function,
	options?: Partial<PromisifyOptions>,
): Function;
```

Example:

```ts
import * as Fs from 'fs';
import pfy from 'ts-pfy';

const readFile = pfy( Fs.readFile );

readFile( 'test.txt' ).then( ( value ) => value.byteLength );
```

## Options

### `thisArg`

Type: `any`
Default: `null`

A `this` context to use with function.

### `multiArgs`

Type: `boolean`
Default: `false`

Should multiple arguments be returned as an array?

With `false` only a first argument are returned in Promise.

## License

[MIT](LICENSE).

[npm]: https://img.shields.io/npm/v/ts-pfy.svg
[npm-url]: https://npmjs.com/package/ts-pfy

[deps-dev]: https://img.shields.io/david/dev/m18ru/ts-pfy.svg
[deps-dev-url]: https://david-dm.org/m18ru/ts-pfy?type=dev
