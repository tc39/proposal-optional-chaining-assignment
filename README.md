# Optional Chaining Assignment

Proposal to add support for optional chaining on the left of assignment operators: `a?.b = c`.

## Status

- Champion: Nicolò Ribaudo
- Stage: 1
- Slides:
  - [July 2023](https://docs.google.com/presentation/d/1KL9MRyxprgXDEsxT8Ddrdro074L3fQm88zXHsWL-Dwk)

## Motivation

It often happens that you need to assign to a property of an object, but only if that object actually exists.

The standard way to do it is by guarding the assignment using an `if` statement:
```js
if (obj) obj.prop = value;
```

The language provides a way to _read_ a property of an object but only if that object actually exists (`obj?.prop = value`), and developers have to remember that the same syntax is not supported when assigning.

## Use cases

See [this gist](https://gist.github.com/nicolo-ribaudo/d264e424b618e7deaeca1d6e4f16a7c0) for examples of where this feature would be useful in the Babel and TypeScript code bases.

You can look for examples of where you may use optional chaining in your projects by searching for `if \((.*?)\)[\s\n]*\{?[\s\n]*\1\.`. This regular expression will have both false positives and false negatives, but it's a potential starting point.

## Description

This proposal introduces the following syntax:
| **New syntax** | **Equivalent ES2023** |
|:--:|:--:|
| `expr1?.prop = val`   | `expr1 == null ? undefined : expr1.prop = val`   |
| `expr1?.prop += val`  | `expr1 == null ? undefined : expr1.prop += val`  |
| `expr1?.prop ??= val` | `expr1 == null ? undefined : expr1.prop ??= val` |
| `expr1?.[key] = val`  | `expr1 == null ? undefined : expr1[key] = val`   |
| `expr1?.foo().prop[key] = val`  | `expr1 == null ? undefined : expr1.foo().prop[key] = val`   |

## Implementations

- [X] Babel ([7.23.0](https://babeljs.io/blog/2023/09/25/7.23.0#optional-chaining-assignment-15751))
- [ ] TypeScript ([partially supported](https://www.typescriptlang.org/play?#code/DYUwLgBAhgXBB2BXYwIB8IG8IA85IFsAjEAJwgF8BuAKBqgH4A6HCAXggEZbGWIBqDgGZaQA) in the transform thanks to error recovery)
- [ ] V8
- [ ] JSC
- [ ] SpiderMonkey
