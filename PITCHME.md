# Typing and existing React and Redux application


## Why do I need to type my code?

Don't force poor code to deal with input it is not prepared for :(

>Catching these kinds of errors in compile time can greatly benefit your overall productivity and sanity.


##  What options do I have for typing my javascript

|                                    | Flow                                               | Typescript                                                                                                                 |
| ---------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| What is?                           | Static Type Checker                                | A superset of JS language which has static type checking                                                                   |
| What does that mean?               | You still use babel to add support new JS features | Supports anything Stage 3 and later. Has addition features like `private` & `protected` properties,  and `Mixins` (traits) |
| Who made it?                       | Facebook                                           | Microsoft                                                                                                                  |
| NPM weekly downloads               | 499,469                                            | 6,137,721                                                                                                                  |
| Ease of use in existing code base? | ⭐⭐⭐⭐                                               | ⭐⭐                                                                                                                         |

! Note all ⭐ Are based on my opinion there is no official chart and this is a gross oversimplification


## Which should I use in my existing project?

Considerations:
- Cost to convert existing code base and company buy-in
- Tools and setup that you already have compared with typescript setup
- Developer preference and buy-in


## For the purposes of this talk let us assume:
We have a fairly large code base already. We have all the tools in place and we are happy with the JS features that we have and we can add to them with Babel.

...so we decide to use Flow


## What it looks like:

Type declaration and erroneous type usage:

```js
const animals: string[] = ['🐈', '🐕', '🐕', '🐅']

animals.push(99)
```

Console output:

```
Cannot call animals.push because number [1] is incompatible with string [2] in array element.

      4│
 [2]  5│ const animals: string[] = ['🐈', '🐕', '🐕', '🐅']
      6│
 [1]  7│ animals.push(99)
```


## How to set it up

1. Add flow as a dependency to your project `yarn add --dev flow-bin`
2. Add the flow preset to babel `yarn add --dev @babel/preset-flow`  & corresponding babel config
3. Add the `flow` command to your package json (be that stand alone, as part of the build or lint stages)
4. Run `npx flow init` to initialise  an empty `.flowconfig` file
5. Add the `// @flow` annotation at the top of any file you would like to


## 🎉 Congratulations you've now got flow in your project


## It will first tell you what things it needs help understanding
You don't need to type everything, some things it can figure out based on the rest of the code and its types


## Things you can do


## Example primitive type + input:

```tsx
const createOption = (cat: string) => (
  <option key={cat} value={cat}>
    {cat}
  </option>
)
```


## Example primitive type + input optional:

```tsx
const createOption = (cat?: string) =>
  cat ? (
    <option key={cat} value={cat}>
      {cat}
    </option>
  ) : (
    <option key='no-cat' value=''>
      No cat
    </option>
  )
```


## Example object type + return

```tsx
const createOption = (cat: string): React.Node => (
  <option key={cat} value={cat}>
    {cat}
  </option>
)
```



## Example of enum

```typescript
const animals: ('🐈' | '🐕' | '🐅' | '🦝' | '🦆')[] = [
  '🐈',
  '🐕',
  '🐅',
  '🦝',
  '🦆'
]
```


## Example of enum + type alias

```typescript
type Cat = '🐈' | '🐕' | '🐅' | '🦝' | '🦆'

const animals: Cat[] = ['🐈', '🐕', '🐅', '🦝', '🦆']
```


## Example of Component Property Types + function signatures

```typescript
type ComponentProps = {
  inputValue: string,
  handleNewCat: (value: string) => {},
  clearCat: () => {},
}

const App = ({ inputValue, handleNewCat, clearCat }: ComponentProps) => (
     //...
)
```

Note: PropTypes is at runtime vs static for Flow and Typescript



## Example of Redux State

```typescript
type State = { inputValue: string }

const InitialState = {
  inputValue: ''
}
```


## Example of Redux Action Creators
```typescript
const HANDLE_NEW_CAT = 'HANDLE_NEW_CAT'
const ClEAR_CAT = 'ClEAR_CAT'

type HandleNewCat = { type: 'HANDLE_NEW_CAT', value: string }
type ClearCat = { type: 'ClEAR_CAT' }

type Action = HandleNewCat | ClearCat

export const handleNewCat = (value: string): HandleNewCat => ({
  type: HANDLE_NEW_CAT,
  value
})

export const clearCat = (): ClearCat => ({
  type: ClEAR_CAT
})
```


## Example of Redux Reducer

```typescript
// To type action you shouldn't destruct it.
export const reducer = (state: State = InitialState, action: Action) => {
  switch (action.type) {
    case HANDLE_NEW_CAT:
      return {
        ...state,
        inputValue: action.value
      }

    case ClEAR_CAT:
      return {
        inputValue: ''
      }

    default:
      return state
  }
}
```


## You can export and import types

```typescript
export type State = { inputValue: string }
```
....

```typescript
import type { State } from './ducks'

const mapStateToProps = (state: State) => ({
  inputValue: state.inputValue
})

```


## Writing tests for your types

```typescript
const animals: string[] = ['🐈', '🐕', '🐕', '🐅']

// $ExpectError
animals.push(99)
```
Flow:
```config
[options]
suppress_comment=\\(.\\|\n\\)*\\$ExpectError
include_warnings=true
```
Typescript:

with `dtslint` package


## Convert to typescript demo....


## Something fun:
![Glow Logo](https://raw.githubusercontent.com/thejameskyle/glow/master/logo.jpg)



## Flow:
![flow output when getElementById can return null](https://snag.gy/fViESC.jpg)



## Glow
![flow output when getElementById can return null using glow](https://snag.gy/jCO1Kn.jpg)

