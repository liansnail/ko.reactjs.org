---
title: React v0.12
author: [zpao]
---

We're happy to announce the availability of React v0.12! After over a week of baking as the release candidate, we uncovered and fixed a few small issues. Thanks to all of you who upgraded and gave us feedback!

We have talked a lot about some of the bigger changes in this release. [We introduced new terminology](/blog/2014/10/14/introducing-react-elements.html) and changed APIs to clean up and simplify some of the concepts of React. [We also made several changes to JSX](/blog/2014/10/16/react-v0.12-rc1.html) and deprecated a few functions. We won't go into depth about these changes again but we encourage you to read up on these changes in the linked posts. We'll summarize these changes and discuss some of the other changes and how they may impact you below. As always, a full changelog is also included below.

The release is available for download:

- **React**  
  Dev build with warnings: <https://fb.me/react-0.12.0.js>  
  Minified build for production: <https://fb.me/react-0.12.0.min.js>
- **React with Add-Ons**  
  Dev build with warnings: <https://fb.me/react-with-addons-0.12.0.js>  
  Minified build for production: <https://fb.me/react-with-addons-0.12.0.min.js>
- **In-Browser JSX transformer**  
  <https://fb.me/JSXTransformer-0.12.0.js>

We've also published version `0.12.0` of the `react` and `react-tools` packages on npm and the `react` package on bower.

## New Terminology & Updated APIs {/*new-terminology--updated-apis*/}

v0.12 is bringing about some new terminology. [We introduced](/blog/2014/10/14/introducing-react-elements.html) this 2 weeks ago and we've also documented it in [a new section of the documentation](/docs/glossary.html). As a part of this, we also corrected many of our top-level APIs to align with the terminology. `Component` has been removed from all of our `React.render*` methods. While at one point the argument you passed to these functions was called a Component, it no longer is. You are passing ReactElements. To align with `render` methods in your component classes, we decided to keep the top-level functions short and sweet. `React.renderComponent` is now `React.render`.

We also corrected some other misnomers. `React.isValidComponent` actually determines if the argument is a ReactElement, so it has been renamed to `React.isValidElement`. In the same vein, `React.PropTypes.component` is now `React.PropTypes.element` and `React.PropTypes.renderable` is now `React.PropTypes.node`.

The old methods will still work but will warn upon first use. They will be removed in v0.13.

## JSX Changes {/*jsx-changes*/}

[We talked more in depth about these before](/blog/2014/10/16/react-v0.12-rc1.html#jsx-changes), so here are the highlights.

- No more `/** @jsx React.DOM */`!
- We no longer transform to a straight function call. `<Component/>` now becomes `React.createElement(Component)`
- DOM components don't make use of `React.DOM`, instead we pass the tag name directly. `<div/>` becomes `React.createElement('div')`
- We introduced spread attributes as a quick way to transfer props.

## DevTools Improvements, No More `__internals` {#devtools-improvements-no-more-\_\_internals} {/*devtools-improvements-no-more-__internals-devtools-improvements-no-more-__internals*/}

For months we've gotten complaints about the React DevTools message. It shouldn't have logged the up-sell message when you were already using the DevTools. Unfortunately this was because the way we implemented these tools resulted in the DevTools knowing about React, but not the reverse. We finally gave this some attention and enabled React to know if the DevTools are installed. We released an update to the devtools several weeks ago making this possible. Extensions in Chrome should auto-update so you probably already have the update installed!

As a result of this update, we no longer need to expose several internal modules to the world. If you were taking advantage of this implementation detail, your code will break. `React.__internals` is no more.

## License Change - BSD {/*license-change---bsd*/}

We updated the license on React to the BSD 3-Clause license with an explicit patent grant. Previously we used the Apache 2 license. These licenses are very similar and our extra patent grant is equivalent to the grant provided in the Apache license. You can still use React with the confidence that we have granted the use of any patents covering it. This brings us in line with the same licensing we use across the majority of our open source projects at Facebook.

You can read the full text of the [LICENSE](https://github.com/facebook/react/blob/master/LICENSE) and [`PATENTS`](https://github.com/facebook/react/blob/master/PATENTS) files on GitHub.

---

## Changelog {/*changelog*/}

### React Core {/*react-core*/}

#### Breaking Changes {/*breaking-changes*/}

- `key` and `ref` moved off props object, now accessible on the element directly
- React is now BSD licensed with accompanying Patents grant
- Default prop resolution has moved to Element creation time instead of mount time, making them effectively static
- `React.__internals` is removed - it was exposed for DevTools which no longer needs access
- Composite Component functions can no longer be called directly - they must be wrapped with `React.createFactory` first. This is handled for you when using JSX.

#### New Features {/*new-features*/}

- Spread operator (`{...}`) introduced to deprecate `this.transferPropsTo`
- Added support for more HTML attributes: `acceptCharset`, `classID`, `manifest`

#### Deprecations {/*deprecations*/}

- `React.renderComponent` --> `React.render`
- `React.renderComponentToString` --> `React.renderToString`
- `React.renderComponentToStaticMarkup` --> `React.renderToStaticMarkup`
- `React.isValidComponent` --> `React.isValidElement`
- `React.PropTypes.component` --> `React.PropTypes.element`
- `React.PropTypes.renderable` --> `React.PropTypes.node`
- **DEPRECATED** `React.isValidClass`
- **DEPRECATED** `instance.transferPropsTo`
- **DEPRECATED** Returning `false` from event handlers to preventDefault
- **DEPRECATED** Convenience Constructor usage as function, instead wrap with `React.createFactory`
- **DEPRECATED** use of `key={null}` to assign implicit keys

#### Bug Fixes {/*bug-fixes*/}

- Better handling of events and updates in nested results, fixing value restoration in "layered" controlled components
- Correctly treat `event.getModifierState` as case sensitive
- Improved normalization of `event.charCode`
- Better error stacks when involving autobound methods
- Removed DevTools message when the DevTools are installed
- Correctly detect required language features across browsers
- Fixed support for some HTML attributes:
  - `list` updates correctly now
  - `scrollLeft`, `scrollTop` removed, these should not be specified as props
- Improved error messages

### React With Addons {/*react-with-addons*/}

#### New Features {/*new-features-1*/}

- `React.addons.batchedUpdates` added to API for hooking into update cycle

#### Breaking Changes {/*breaking-changes-1*/}

- `React.addons.update` uses `assign` instead of `copyProperties` which does `hasOwnProperty` checks. Properties on prototypes will no longer be updated correctly.

#### Bug Fixes {/*bug-fixes-1*/}

- Fixed some issues with CSS Transitions

### JSX {/*jsx*/}

#### Breaking Changes {/*breaking-changes-2*/}

- Enforced convention: lower case tag names are always treated as HTML tags, upper case tag names are always treated as composite components
- JSX no longer transforms to simple function calls

#### New Features {/*new-features-2*/}

- `@jsx React.DOM` no longer required
- spread (`{...}`) operator introduced to allow easier use of props

#### Bug Fixes {/*bug-fixes-2*/}

- JSXTransformer: Make sourcemaps an option when using APIs directly (eg, for react-rails)
