[![Build Status](https://travis-ci.org/drcmda/react-spring.svg?branch=master)](https://travis-ci.org/drcmda/react-spring) [![npm version](https://badge.fury.io/js/react-spring.svg)](https://badge.fury.io/js/react-spring)

# Installation 🖥

    npm install react-spring

# Table of Contents 👇

* [What is it?](#what-is-it-)
* [Why do we need yet another?](#why-do-we-need-yet-another-)
* [Overview](#overview-)
* [Render props, interpolation and native rendering](#render-props-interpolation-and-native-rendering-)
* [Links](#links-)

# What is it? 🤔

<p align="middle">
  <img src="assets/spring.gif" width="285" />
  <img src="assets/transitions.gif" width="285" />
  <img src="assets/trails.gif" width="285" />
</p>
<p align="middle">
  <img src="assets/tree.gif" width="285" />
  <img src="assets/sunburst.gif" width="285" />
  <img src="assets/areas.gif" width="285" />
</p>
<p align="middle">
  <img src="assets/gestures.gif" width="285" />
  <img src="assets/reveals.gif" width="285" />
  <img src="assets/morph.gif" width="285" />
</p>
<p align="middle">
  <img src="assets/vertical.gif" width="285" />
  <img src="assets/horizontal.gif" width="285" />
  <img src="assets/keyframes-trail.gif" width="285" />
</p>
<p align="middle">
  <img src="assets/dragndrop.gif" width="285" />
  <img src="assets/stream.gif" width="285" />
  <img src="assets/time.gif" width="285" />
</p>

A set of simple, spring-physics based primitives (as in building blocks) that should cover most of your UI related animation needs once plain CSS can't cope any longer. Forget easings, durations, timeouts and so on as you fluidly move data from one state to another. This isn't meant to solve each and every problem but rather to give you tools flexible enough to confidently cast ideas into moving interfaces.

# Why do we need yet another? 🧐

react-spring is a cooked down fork of Christopher Chedeau's [animated](https://github.com/animatedjs/animated) (which is used in react-native by default). It is trying to bridge it with Cheng Lou's [react-motion](https://github.com/chenglou/react-motion). Although both are similarily spring-physics based they are still polar opposites.

|                | Declarative | Primitives | Interpolations | Performance |
| -------------- | ----------- | ---------- | -------------- | ----------- |
| React-motion   | ✅          | ✅         | ❌             | ❌          |
| Animated       | ❌          | ❌         | ✅             | ✅          |
| React-spring   | ✅          | ✅         | ✅             | ✅          |

react-spring builds upon animated's foundation, casting its imperative side out, making it leaner and more flexible. It inherits react-motions declarative api and goes to great lengths to simplify it. It has lots of useful primitives, can interpolate mostly everything and last but not least, can animate by committing directly to the dom instead of re-rendering a component frame-by-frame.

For a more detailed explanation read [Why React needed yet another animation library](https://medium.com/@drcmda/why-react-needed-yet-another-animation-library-introducing-react-spring-8212e424c5ce).

# Overview 🔭

#### Springs ([Demo](https://codesandbox.io/embed/oln44nx8xq))

<img src="assets/spring.gif" width="285" />

A `Spring` will move data from one state to another. It remembers the current state, value changes are always fluid.

```jsx
import { Spring } from 'react-spring'

<Spring from={{ opacity: 0 }} to={{ opacity: 1 }}>
    {styles => <div style={styles}>i will fade in</div>}
</Spring>
```

#### Mount/unmount Transitions ([Demo](https://codesandbox.io/embed/j150ykxrv))

<img src="assets/transitions.gif" width="285" />

`Transition` watches elements as they mount and unmount, it helps you to animate these changes.

```jsx
import { Transition } from 'react-spring'

<Transition
    keys={items.map(item => item.key)}
    from={{ opacity: 0, height: 0 }}
    enter={{ opacity: 1, height: 20 }}
    leave={{ opacity: 0, height: 0 }}>
    {items.map(item => styles => <li style={styles}>{item.text}</li>)}
</Transition>
```

#### 2-state Reveals ([Demo](https://codesandbox.io/embed/yj52v5689))

<img src="assets/reveals.gif" width="285" />

Given a single child instead of a list you can reveal components with it.

```jsx
import { Transition } from 'react-spring'

<Transition from={{ opacity: 0 }} enter={{ opacity: 1 }} leave={{ opacity: 0 }}>
    {toggle ? ComponentA : ComponentB}
</Transition>
```

#### Trails and staggered animations ([Demo](https://codesandbox.io/embed/vvmv6x01l5))

<img src="assets/trails.gif" width="285" />

`Trail` animates the first child of a list of elements, the rest follow the spring of their previous sibling.

```jsx
import { Trail } from 'react-spring'

<Trail from={{ opacity: 0 }} to={{ opacity: 1 }} keys={items.map(item => item.key)}>
    {items.map(item => styles => <div style={styles}>{item.text}</div>)}
</Trail>
```

#### Parallax and page transitions ([Demo](https://codesandbox.io/embed/548lqnmk6l))

<img src="assets/horizontal.gif" width="285" />

`Parallax` allows you to declaratively create page/scroll-based animations.

```jsx
import { Parallax, ParallaxLayer } from 'react-spring'

<Parallax pages={2}>
    <ParallaxLayer offset={0} speed={0.2}>
        first Page
    </ParallaxLayer>
    <ParallaxLayer offset={1} speed={0.5}>
        second Page
    </ParallaxLayer>
</Parallax>
```

#### Time/duration-based implementations and addons ([Demo](https://codesandbox.io/embed/q9lozyymr9))

<img src="assets/time.gif" width="285" />

You'll find varying implementations under [/dist/addons](https://github.com/drcmda/react-spring/tree/master/src/addons). For now there's a time-based animation as well common [easings](https://github.com/drcmda/react-spring/blob/master/src/addons/Easing.js), and IOS'es harmonic oscillator spring. All primitives understand the `impl` property which you can use to switch implementations.

```jsx
import { TimingAnimation, Easing } from 'react-spring/dist/addons'

<Spring impl={TimingAnimation} config={{ delay: 200, duration: 1000, easing: Easing.linear }} ...>
```

#### Keyframes ([Demo](https://codesandbox.io/embed/zl35mrkqmm))

<img src="assets/keyframes-trail.gif" width="285" />

`Keyframes` orchestrates animations in a script that you provide. Theoretically you can even switch between primitives, for instance going from a Spring, to a Trail, to a Transition. It tries its best to remember the last state so that animations are additive. Animation can be awaited and return current props. Be warned: the keyframe API is still highly experiemental and can be subject to changes.

```jsx
import { Keyframes, Spring } from 'react-spring'

<Keyframes script={async next => {
    await next(Spring, { from: { opacity: 0 }, to: { opacity: 1 } })
    await next(Spring, { to: { opacity: 0 } })
}}>
    {styles => <div style={styles}>Hello</div>}
</Keyframes>
```

# Render props, interpolation and native rendering 🚀

### Render props

The Api is driven by render props ([though we do expose imperative Api as well](https://github.com/drcmda/react-spring/edit/master/API-OVERVIEW.md#imperative-api)). By principle we offer both `render` and `children` as well as prop forwardwing (unrecognized props will be spread over the receiving component).

```jsx
const Header = ({ children, bold, ...styles }) => (
    <h1 style={styles}>
        {bold ? <b>{children}</b> : children}
    </h1>
)

<Spring render={Header} to={{ color: 'fuchsia' }} bold={this.state.bold}>
    hello there
</Spring>
```

### Interpolation

You can interpolate almost everything, from numbers, colors (names, rgb, rgba, hsl, hsla), paths (as long as the number of points match, otherwise use [custom interpolation](https://codesandbox.io/embed/lwpkp46om)), percentages, units, arrays and string patterns:

```jsx
<Spring to={{
    scale: toggle ? 1 : 2,
    start: toggle ? '#abc' : 'rgb(10,20,30)',
    end: toggle ? 'seagreen' : 'rgba(0,0,0,0.5)',
    stop: toggle ? '0%' : '50%',
    rotate: toggle ? '0deg' : '45deg',
    shadow: toggle ? '0 2px 2px 0px rgba(0, 0, 0, 0.12)' : '0 20px 20px 0px rgba(0, 0, 0, 0.5)',
    path: toggle ? 'M20,380 L380,380 L380,380 Z' : 'M20,20 L20,380 L380,380 Z',
    vector: toggle ? [1,2,50,100] : [20,30,1,-100],
}}>
```

### Native rendering (and interpolation)

| ![img](assets/without-native.jpeg)                                                                                                                                                                                        | ![img](assets/with-native.jpeg)                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <sub>Libraries animate by having React recalculate the component-tree on every frame. Here it attempts to animate a component consisting of ~300 sub-components, plowing through the frame budget and causing jank.</sub> | <sub>React-spring with the `native` property renders the component _only once_, from then on the animation will be applied directly to the dom in a requestAnimationFrame-loop, similar to how gsap and d3 do it.</sub> |

---

By default we'll render every frame (like in the image on the left) as it gives you more freedom (for instance this is the only way that you can animate React-component props). In situations where that becomes expensive use the `native` flag. The flag is available for all primitives (Spring, Transition & Trail, Keyframes, Parallax is native by design). **Try doing this in all situations where you can**, the benefits are worth it. Especially if your animated component consists of large subtrees, routes, etc.

```jsx
import { Spring, animated } from 'react-spring'

<Spring native from={{ opacity: 0 }} to={{ opacity: 1 }}>
    {styles => <animated.div style={styles}>i will fade in</animated.div>}
</Spring>
```

More about native rendering and interpolation [here](https://github.com/drcmda/react-spring/edit/master/API-OVERVIEW.md#native-rendering-and-interpolation).

# Links 🔗

#### [Examples and Codesandboxes](https://github.com/drcmda/react-spring/blob/master/examples)

Click for a combined example repository you can install as well as a collection of code-sandboxes to toy around with online.

#### [API Overview](https://github.com/drcmda/react-spring/blob/master/API-OVERVIEW.md)

If you ever plan to use this library, this should be a must-read. It will go a little deeper into the primitives and how "native" rendering can make a large performance impact (for the better of course).

#### [Full API reference](https://github.com/drcmda/react-spring/blob/master/API.md)

For annotated prop-types, good for finding out about all the obscure props that i don't want to bore you with (but which might come in handy, you never know).

[Changelog](https://github.com/drcmda/react-spring/blob/master/CHANGELOG.md) | [LICENSE](https://github.com/drcmda/react-spring/blob/master/LICENSE)
