# bs-glamor – [BuckleScript](https://github.com/bucklescript/bucklescript) bindings for [glamor](https://github.com/threepointone/glamor)

The API is still **experimental**. Only a few functions from glamor such as `css` and `global` are exposed; other functions such as `renderStatic` are not supported yet.

## Installation

```sh
npm install --save bs-glamor
```

In your `bsconfig.json`, include `"bs-glamor"` in the `bs-dependencies`.

## Usage

The following examples (in [Reason](http://reasonml.github.io) syntax) assume that `Glamor` is included in the namespace:

```reason
open Glamor;
```

* Simple styling:

    ```reason
    css([
        color("red"),
        border("1px solid black")
    ])
    ```

* Styling with selectors (`@media`, `:hover`, `&`, etc.):

    ```reason
    css([
        color("red"),
        Selector("@media (min-width: 300px)", [
            color("green")
        ])
    ])
    ```

* Selectors can be nested:

    ```reason
    css([
        color("red"),
        Selector("@media (min-width: 300px)", [
            color("green"),
            Selector("& .foo", [
                color("blue")
            ])
        ])
    ])
    ```

You can isolate inclusion of the `Glamor` namespace in the following way:

```reason
Glamor.(css([color("red")]))
```

The result of the `css` function can be assigned to a class name, e.g. in JSX:

```reason
<div className=(css([color("red")])) />
```

You can also combine stylings with a class names. For example, if you want to use
some classes from third-party libraries (e.g. Bootstrap), or just to add a class name
for testing purposes along with glamor styles:

```reason
<div className=("btn " ^ css([color("red")])) />
```

### Merging CSS rules

You can merge CSS rules using `merge`:

```reason
let text_primary = css([color("indigo")]);
let small = css([fontSize("10px")]);

<p className=(merge([text_primary, small])) />
```

glamor will make sure that rules are merged in the correct order, managing nesting and precedence for you.

### Global CSS

 You can define global CSS rules with `global`:

 ```reason
 Glamor.(global("body", [margin("0px")]));
 Glamor.(global("h1, h2, h3", [color("palegoldenrod")]));
 ```

### Keyframes

Define animation keyframes:

```reason
let bounce = Glamor.keyframes([
  ("0%", [transform("scale(0.1)"), opacity("0")]),
  ("60%", [transform("scale(1.2)"), opacity("1")]),
  ("100%", [transform("scale(1)")])
]);
let styles = css([
    animationName(bounce),
    animationDuration("2s"),
    width("50px"),
    height("50px"),
    backgroundColor("red")
]);

<div className=styles>bounce!</div>
```

## Example

See [reason-react-tictactoe](https://github.com/poeschko/reason-react-tictactoe) for a live example.

## Development

```sh
npm run start
```

### Tests

There are some simplistic tests using [bs-jest](https://github.com/BuckleTypes/bs-jest).

```sh
npm run test
```

## Thanks

Thanks to [reason-react-example](https://github.com/chenglou/reason-react-example), [reason-react](https://github.com/reasonml/reason-react), and [bs-jest](https://github.com/BuckleTypes/bs-jest) for inspiration how to create a Reason library, and of course, thanks to [glamor](https://github.com/threepointone/glamor).
