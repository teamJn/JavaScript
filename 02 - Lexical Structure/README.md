Lexical structure is a set of basic rules that tell how to write a program.

## Character Set
JavaScript programs are written using the _Unicode_ character set. Unicode is a superset of ASCII and Latin-1 and supports virtually every written language currently used on the planet.
```javascript
// JS printing malayalam using unicode
console.log('\u0D1C\u0D4B\u0D2C\u0D3F'); // "ജോബി" My name in malayalam language

// Using unicode as object property
const obj = {
  \u0D1C\u0D4B\u0D2C\u0D3F : "Ernakulam"
}
```