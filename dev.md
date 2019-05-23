# For Developers

You might want to look at the guide for [Product and Design](/product-design.md) first, as it's useful to have in mind to understand the rationale for some of these tips.

If this guide looks a bit long, start by reading just [Questions to Ask in Code Reviews](#questions-to-ask-in-code-reviews) and [Common Easy to Fix Issues](#common-easy-to-fix-issues), which will help you avoid the majority of accessibility issues. If you like understanding how things work under the hood, you might like the [Concepts](#concepts) section.



## Questions to Ask in Code Reviews
See sections below for explanations.
* If you see an `img` tag or `svg`, ask: should it have alt text?
* If you see a click handler, ask: should this be a button?
* If you see an array of DOM elements (say created via a `map`), ask: should this be a list?
* If you see new pages or sections of pages created, ask: Is there something that should be a `heading`?

-----------------------------

## Common Easy to Avoid Issues
These issues are as easy to avoid as to include, once you know about them! These issues are common, and knowing about best practices will help you avoid most or many issues on your site.

### Semantic HTML
Following these practices generally results in cleaner and easier to read and maintain code, too!

[Overview from MDN](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML)

#### Heading tags
* [Heading Accessibility Concerns (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements#Accessibility_concerns)
* Screen readers use these to navigate. So use them in accord with semantics, rather than styling.
* Don't skip heading tag levels (eg have just an `h5` tag, or only `h1` and `h3` tags without any `h2` tags.
* `h1` tags
  * Avoid having more than one `h1` tag per page. If there is no top level header on the page but the user would get an overview of what the page was about visually, use a `h1` tag visible to screen readers only.

#### Links and buttons
* If it has a click handler, it should probably be a `button`.
* If it has an anchor tag with `onClick` rather than an `href`, it should probably be a `button`.
* [Links vs Buttons article](https://marcysutton.com/links-vs-buttons-in-modern-web-applications/). Note: there may in some cases be an unclear distinction between a button and a link.
* If it's clickable with a mouse, you should be able to reach it via tab and see a focus state, and activate it via enter.

#### Lists
Screen readers use these to navigate. If it's a list, use list, and list item tags.

### Images
#### `<img>`
* All `img`'s should have `alt` attributes.
* If the image is purely decorative, use the empty string: `alt=“”`.
* The alt text should provide the info that the image provides, rather than describing it. Eg: a right arrow image might have `alt='next slide'`, rather than `alt='right arrow'`.

#### `<svg>`
Here's how to handle alternative text for `svg`'s:
* Suppose the `svg` is non decorative, and is not in a button or link. Add role="img" and an aria-labelledby attribute to the `<svg>` tag. Then add a `<title>` tag within the `<svg>` tag. Ensure the aria-labelledby attribute holds the ID of the `<title>` tag and that the `<title>` is a direct descendant of the `<svg>` tag. For example:
    ```html
      <svg role="img" aria-labelledby=“next”>
        <title id=“next">Next Slide</title>
      </svg>
    ```
* In the case of icons in buttons or links, instead of the above, you can add `aria-label="alternative text here"` to the button/anchor tag element.
* If your `svg` is _not_ in a button or link, it is not sufficient to wrap the element in a div with an `aria-label`, as screen readers will not pick up this information.
* In the case of a purely decorative `svg`, you can leave it without title and labelled-by attributes. Screen readers will not pick it up.

-----------------------------

# Keyboard Navigation
* [Using Native Keyboard Accessibility (MDN)](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Using_native_keyboard_accessibility)
* Every element on the page that you can interact with via a mouse should also have equivalent functionality with a keyboard.
* Focusable elements (buttons, links, inputs) should have a visible focus state.
* [Tab index](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex) can be useful. [MDN guide](https://developers.google.com/web/fundamentals/accessibility/focus/using-tabindex), [Accessibility guide](https://webaim.org/techniques/keyboard/tabindex).

-----------------------------

# ARIA
* [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) (“Accessible Rich Internet Application", or just "Aria") is a set of attributes that can be added to HTML elements to provide more information for screen readers. Commonly used examples are `aria-label` and `role`.
* Some things to keep in mind before using ARIA:
  * A common anti-pattern is to use ARIA excessively. Aria attributes will overwrite rather than supplement information already present, and are generally not needed for semantic HTML.
  * The first rule of the [Aria Authoring Guidelines](https://www.w3.org/TR/wai-aria-practices-1.1/) is "No Aria is Better than Bad Aria."
  * Aria and `role` attributes will not change the behavior of an element. For example, adding ‘role=“button”’ to a div, will not make it reachable and usable with a keyboard. You will have to handle this separately (via `tabIndex` and listening to keydown events). Ideally, you can just use an HTML ‘button’ element.
* Cases where ARIA is often a good idea:
  * Use `aria-label` when a screen reader would otherwise not have a name for the node, for example in labeling `svg`s as in [the example above](#Images). 
  * ARIA attributes are to be used when creating some custom UI controls, where there isn't a semantic HTML solution. For example, they are useful when creating components such as modals, flyouts, dropdown menus, or carousels.
* How to use ARIA
  * [Google web foundations intro to aria](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/)
  * [Aria Authoring Guidelines from W3C](https://en.wikipedia.org/wiki/WAI-ARIA) gives examples of UI patterns, explaining what keyboard interactions and aria attributes are expected.

-----------------------------

# Concepts
This is how accessibility in HTML and browsers works "under the hood."
## The Accessibility Tree and Assistive Technologies
  * Browsers construct an [Accessibility Tree](https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree), which contains a subset of information in the DOM, and which assistive technologies (such as screen readers) rely on to surface information about the page. So a screen reader user won’t know about anything that isn’t in the accessibility tree.
  * The nodes of the accessibility tree correspond to a subset of the nodes in the DOM. The nodes are “accessibility objects,” which have name, role, and state attributes. A checkbox, for example, might have a name “accept conditions,” a role “checkbox,” and a state “checked.”
  * How do these accessibility objects get the name, state and role attributes that assistive technologies will depend on? This is based on semantic markup. For example, a `<button>` element will get a role `button`, but a `<div>` with a click handler will not. When semantic markup isn’t sufficient, the developer can supplement with aria or role attributes, for example by adding `role=“button”` to a `div`.
  * Besides surfacing information about a given node, the accessibility tree's structure provides an overview of the structure of a page, which assistive technologies rely on to aid navigation. Navigating the tree structure is based on semantic markup such as headers, sections and lists.
  * Suggestions for developing an intuition about this:
     * Try out Firefox's [Accessibility Inspector tool](https://developer.mozilla.org/en-US/docs/Tools/Accessibility_inspector), which lets you see the accessibility tree. Take a look at Chrome's [Accessibility Pane](https://developers.google.com/web/updates/2018/01/devtools#a11y-pane) in the elements tab.
     * Try out Voice Over on Mac. Some quick things to try out:
          1. Turn on voice over with `command F5`,
          3. View/hear the navigation "rotor" with `ctrl option u`.

-----------------------------

# Tools and Resources

### Tutorials and general guides
  * [Brief web.dev tips from google](https://web.dev/accessible) - this is a good one to start with as it is clear and has interactive demos.
  * [Overview course](https://developers.google.com/web/fundamentals/accessibility/) from google, same content as free [Google/Udacity course](https://www.udacity.com/course/web-accessibility--ud891).
  * [Egghead video course](https://egghead.io/courses/start-building-accessible-web-applications-today)
  * [MDN Learn Accessibility article](https://developer.mozilla.org/en-US/docs/Learn/Accessibility)
  * [A11y Project](https://a11yproject.com/patterns) - 'patterns' section has useful code samples
  * [Accessibility in Mind articles](https://webaim.org/techniques/) - eg see article on [forms](https://webaim.org/techniques/forms/)
  * [React docs](https://reactjs.org/docs/accessibility.html) include some good resources, with tools (such as linters) suggested at the end.

### Useful tools built into browsers
  * [Accessibility Insights Chrome Extension](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni) from Microsoft: Chrome extension that surfaces issues in the browser.
  * Chrome has an accessibility audit built into the [Lighthouse](https://developers.google.com/web/tools/lighthouse/) audit tools!
  * Firefox has a [Accessibility Inspector tool](https://developer.mozilla.org/en-US/docs/Tools/Accessibility_inspector) that help you understand what screen readers see.
  * On MacOS, if tab does not focus links for in Firefox or Safari, to fix: https://www.scottohara.me/blog/2014/10/03/link-tabbing-firefox-osx.html

### Linters: 
  * [`eslint-plugin-jsx-a11y`](https://www.npmjs.com/package/eslint-plugin-jsx-a11y)
  * [`tslint-react-a11y`](https://github.com/joaovieira/tslint-react-a11y)

### Browser add ons (surfacing info in browser/console).
These allow you to see accessibility issues on your site and learn about best practices.
* Microsoft Accessibility Insights Chrome extension
* [Totally](http://khan.github.io/tota11y/) highlights issues where they appear on the screen. Product/design might find these useful too, eg to spot color contrast issues.
* [Axe](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd/related?hl=en-US) lists issues in a tab in dev tools. It includes suggestions about how to fix the issue.
* Axe rules can be [configured](https://github.com/dequelabs/axe-core/blob/master/doc/API.md#api-name-axeconfigure).

### ARIA
  * [W3C Aria Authoring Guidelines](https://en.wikipedia.org/wiki/WAI-ARIA)
  * [Intro from Google Web Fundamentals](https://en.wikipedia.org/wiki/WAI-ARIA)
  * [MDN intro](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

### Accessiblity Tree
   * [Overview from MDN](https://developers.google.com/web/fundamentals/accessibility/semantics-builtin/the-accessibility-tree)
   * [Web Accessibility with Accessibility API](https://www.smashingmagazine.com/2015/03/web-accessibility-with-accessibility-api/) 
   * [The Accessibility Tree](http://whatsock.com/training/) - detailed discussion of screen reader idiosyncrasies.
   * [From Semantics to Screen Readers](https://alistapart.com/article/semantics-to-screen-readers)



To leave comments, questions, suggestions, or other feedback, feel free to do so by [opening an issue here](https://github.com/bebebebebe/accessibility-guides/issues). Thanks!