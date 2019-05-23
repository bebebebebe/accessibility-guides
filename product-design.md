# For Product and Design

One of the most frequently given pieces of advice on accessibility best practices is to start considering accessibility as early as possible, preferably at the design stage.

Below are the top three areas where accessibility issues can be addressed at the product design stage before dev work begins: [keyboard interaction](#1-keyboard-interactions), [color contrast](#2-color-contrast), and [alternative text](#3-alternative-text). The guide lists things to check in user stories and acceptance criteria before dev work begins.


## 1. Keyboard Interactions 
### Brief Summary
User interactions should be defined for keyboard when defined for mouse / touch. 

### Why is it Important?
Due to motor impairments, some users can’t use a mouse but will instead use either a keyboard or a specialized assistive technology switch device. Blind or low vision people using a screen reader will typically also use an assistive technology device or keyboard rather than a mouse. The good news for product and design is that such devices all tap into the same api as the keyboard. So all we have to do to support them is support the keyboard. As a bonus, this will create a better UX for all users, as many prefer to be able to use a keyboard to interact with sites. It’s typically more efficient in terms of development time to design for keyboard up front, rather than retrofitting as a follow up later.

### Keyboard Behavior
Start by being familiar with these common keyboard behaviors:

  * **Focus**: form inputs, buttons, and links can be in a ‘focus’ state. When you’re typing in an input field, it has focus. When you tab to a button or link so that you could press enter to press it, then it is focused.
  * **Buttons**: when you’re focused on a button, you can press it via the enter or space key.
  * **Links**: when you’re focused on a link, you can press it via the enter key.
  * **Forms**: when focused on an element in a form, you can submit it via the enter key. 

### Common Ways to Adapt Mouse Behavior to Keyboard Behavior
  * **Mouse use: Hover:** If an element updates on hover, make it a focusable element that updates in the same way on focus.
  * **Mouse use: Click:** If something happens when you click on an element with a mouse, make the element focusable and make the same thing happen on enter/space. 

### Checklist for Acceptance Criteria  (to list in User Stories)
1. Product: Are there Acceptance Critera (AC) pertaining to hover or click/touch interactions? 
  * If so, add AC’s so that equivalent functionality for keyboard only users is defined.  
  * Example AC: “When I hover on the button, the text on the button changes from ‘Selected’ to ‘Cancel’. 
  * Example addition for this AC: “When I focus on the button via tab, the text on the button changes from ‘Selected’ to ‘Cancel.’”  
2. Product: define focus order if new interactive elements are introduced.
  * When focusing on elements on the page via tab, what is the tab order? This should match the layout generally, so will likely only need definition for complex interactive elements. 
3. Design: define visible focus states. 
  * This is important so that keyboard users can see where they are on the page, in the same way that a mouse cursor tells a mouse user where they are on a page.
  * Visible focus states can generally be the browser defaults (e.g. in Chrome, this is a blue focus ring), but designers may want to define their own focus states. 

--------------------------------

## 2. Color Contrast 
### Brief Summary
Color contrast between text and background should meet guidelines per [WebAIM's color checker](https://webaim.org/resources/contrastchecker/ ).

### Why is it Important?
People perceive color differently, and some people with low vision need higher contrast to be able to read. Color blind users and users with some visual impairment should be able to understand your site. 

### Checklist for Design Mocks / Wireframes
Designers can use a Sketch plugin, Stark, to validate sufficient color contrast. In other mocks, look out for low contrast and double check possible issues via the color contrast checker liked above. 

--------------------------------

## 3. Alternative Text 
## Brief Summary
Images and icons that convey visual information need “alternative text” labels that are exposed by a screen reader but are not visible on the screen. 

### Why is it Important?
Screen readers depend on these to convey the use and content of the site. (Imagine trying to use buttons with icons that conveys their use, but without being able to see the icons.)

### Checklist for Acceptance Criteria
* Are any images or icons introduced? Supply alternative text for them in the story. 
* Treat this the same way as other text. The same people responsible for writing other text should write this text. If you translate text for internationalization, you should also translate this text.
* In the case of a purely decorative image that does not convey meaning, you do not need to supply alternative text.
* [WebAIM alt text guide](https://www.w3.org/WAI/tutorials/images/decision-tree/)

--------------------------------

## Specific Areas:
These are more specific areas to look out for where there are important accessibility principles that can be addressed before dev work begins. Many of these areas relate to keyboard interactions, and are about having equivalent keyboard and mouse user options. (I may create some other docs with details about these!) 
* A new page (define page title, and skip links if the page has many links at the top repeated on other pages).
* Forms and user input.
* Modals.
* Dropdown menus.
* Tooltips and expandable content.
* Updating or temporary content (toasts, banners, time dependent messaging).

--------------------------------

## Tools to help you catch issues
### Comment on tooling limitations
Automated accessibility testing is useful, but keep in mind that it will only catch a small number of issues, so you do not want to rely on it exclusively. However, it is a great way to catch some issues and become more aware of what you should be looking out for.
### Suggestion
[Accessibility Insights Chrome Extension](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni) is intuitive and easy to use for non developers as well as developers.

## Resources
* [Salesforce, 7 Things Every Designer Needs to Know about Accessibility](https://medium.com/salesforce-ux/7-things-every-designer-needs-to-know-about-accessibility-64f105f0881b)
* [Accessibility for Teams](https://developers.google.com/web/fundamentals/accessibility/a11y-for-teams) article from Google give some areas where designers and product managers can play a role.
* [Checklist from VoxMedia](http://accessibility.voxmedia.com/) lists areas where product and design can play a role.

To leave comments, questions, suggestions, or other feedback, feel free to do so by [opening an issue here](https://github.com/bebebebebe/accessibility-guides/issues). Thanks!
