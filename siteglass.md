# Siteglass

## [pattern-library](https://github.com/gdcorp-site/pattern-library)

The following is a review of the `/src` directory.

### `elements/buttons`

The `merch-button` seems to be a dependency for a few other buttons. There's a few items of note:

- On hover, the button will scale up 102%. On press, the button will scale down to 99%.
- Disabled buttons are set as opaque and given local intents to describe the disabled color.
- Small buttons have the border radius and padding adjusted.
- There is a CTA arrow added to the button using a pseudo element, not using the `as=cta` prop. There are several styles managing the treatment of this arrow.
- Local intents are used to change the look of primary, secondary, and inline buttons.
- There is an attribute called `data-width="full"` which sets `width: 100%` on the button.
- There is an attribute called `data-inverted` which sets new local intents for inverted colors.
- There are style changes for breakpoints and using `data-shrink`.
- There is a function which transforms a given `design` to one accepted by the `@ux/button`. `merch` -> `primary`, `merch-secondary` -> `secondary`.
- There is a feature which causes an element to be scrolled to once clicked built into the button.
- There is some logic built into the button which updates a given `href`, `itemTrackingCode` and `customProperties`. It will add the given item tracking code to all `href`s included on every button.

The `link` seems to be the base for some other buttons, specifically the `@ux/button` when meant to be displayed as a link.

- CSS included here seems specific to the link type of button.
- It updates the display to `inline-flex`.
- The text inside has `font-weight: 850` and `position: relative`.
- The underline is created using a pseudo element positioned _precisely_ with magic numbers to allow the underline to fade-in.
- The component has internal tracking logic similar to the `merch-button`.
- The component preloads a `<span/>` of over 20 icons that may appear in the button with every button. The content of this `<span/>` is empty in the client.

The `add-to-cart` button uses the `merch-button` as a base.

- The export is not a `<button>` but a styled `<form>` with hidden `<input>` elements.
- The component does additional tracking logic outside of the `merch-button` logic; setting additional tracking attributes.

The `video-play-button` uses the `link` and imports the `merch-button`.

- The component is not just a button; it includes a `<Lightbox/>` component triggered when pressed.
- While it uses link styles, it does not navigate to a new page. It is meant to trigger the `openLightBox` function within this component.
- The `merch-button` is eventually used for access to transcripts, passed into the `<Lightbox/>`.
- There are some styles which are mostly for interactions.

The `get-the-app` uses a styled `<a/>` element.

- It is used to render the platform badges for getting an app in the iOS App Store or Android Google Play.
- The `src` for internal `<img/>` is passed to this component. It seems to point at a sitecore CDN for the examples.
- The internal tracking for this component is less involved than other buttons; it uses an existing tracking util with no additional manipulation.

The `play-button` uses a styled `<button/>` element.

- It is _not_ used in the `video-play-button` component. It is visually a different button.
- It includes `whileHover` and `whileTap` props from `framer-motion` to adjust the scale. These do not work in the examples.

The `select-item` uses a styled `<button/>` element.

- The visual treatment seems to be a different button, meant to display in larger areas; almost card-like.
- There is incorrect usage of intents; mapping `foregroundColor` to a border.
- There is direct usage of a hashed CSS intent variable.
- This does not use the `tracking` util, but instead a `logEvent` util from traffic.
- There is a `onSelect` callback which returns the `id` and the inverse of the given `selected` prop.

### `elements/motion`

The main file helps with providing timing functions for animations, the first export `getCSSTransition` helps prepare CSS transitions with some reusable keywords.

### `elements/tab`

This component doesn't seem to have an example in the storybook.

- It's provided as a styled `<div/>` with some border and background transitions set.
- It is given an `onClick` with no additional accessibility markings.
- The `tracking` util is included with this component without custom configuration.
- There is a `switchTab` callback which returns `id`, `lineupName`, and `items`, all provided by the component props.

### `elements/widgets`

There are three components found here.

The `countdown-clock` has no appropriate analog to anything in UXCore. It is made up of several subcomponents

- There is a separate component for when the countdown has expired. It renders an empty state and `merch-button`.
- The `time-block` component displays a segment of time (eg., `44`) and its label (eg., `seconds`). There is also references to the window within the CSS.
- The `active-countdown` component combines all of the `time-block` components with no additional logic. There is also references to the window within the CSS.
- The `index.js` for this component handles the time logic. The `isoDate` helper method is especially noteworthy. It seems to expect a `RFC 3339` date string, eventually doing some additional string manipulation, including the addition of an offset of `-700` and then creating a date object from the result.

The `sso-form` is a wrapper of the `auth-react/sso-loader` component, which prepares a configuration specific to this use. There is no visual example as the request to `https://sso.test-godaddy.com/v1/api/assets/path` fails in Storybook. This may be an opportunity to follow up with SSO to handle graceful failure.

The `trustpilot` component renders ratings with Trustpilot branding and has no appropriate analog to anything in UXCore.

- There is a text formatter which means to bold the substring found within the `[]`. `'3.5 out of 5 stars based on [19,469 reviews]'` turns into 3.5 out of 5 stars based on **19,469 reviews**.
- Star size is adjusted based on window size.
- There is a `renderRating` function that is located outside of this component. It creates the star `<svg/>`. There is note that a conscious decision to not use intents here was made. Not for the Trustpilot branding but because it is non-interactive.

### `other/actions`

It seems that this directory holds button combinations for different uses. Instead of importing exactly what you need, you give a few props to a base component which then renders the correct component based on those props.

- `base-actions.js` is the main switch statement importing several components from the library which depend on the `actionsLayout` prop.
- `button-set.js` imports the `NSMerchButtonSet` from the `merch-button` directory and inserts the provided `buttons`.
- `index.js` exports the `base-actions.js` and applies a few modal components as options.

### `other/attributed-image`

This component seems to add attribution to a given image. The attribution itself is another component.

### `other/attribution-block`

This component provides the attrbution to images. The styles are a single styled `<p/>` element with several conditionally applied styles, including one which hides the component on mobile devices. It also imports a `formatAttribution` function from `base.functions.js` which seems to be used in several kinds of attributions across the library.

### `other/card`

The card component is composed of the `<SectionItem/>` and `<HeadlineLockup/>` from within pattern library. From the visual examples, this component provides the _content_ within the card, not the padding or border. It seems the imported components do the heavy lifting for this convenience component.

### `other/domain-search`

This component seems to be a wrapper for `@site/domain-search-box` with its own set of overrides including similar tracking manipulation found in earlier buttons.

### `other/ep-search`

This component seems to represent an Example Page Search, which two internal directories `search-bar` and `search-page`. There doesn't seem to be much UXCore related components here. The majority of this seems to use [`@elastic/react-search-ui`](https://www.npmjs.com/package/@elastic/react-search-ui).

- The `search-box-wrapper.js` which exists at top level to this directory seems to take the `@elastic/react-search-ui` component and apply additional styles and logic.
- The `search-bar/index.js` renders the `@elastic/react-search-ui` provider and `search-box-wrapper.js` component with a small config.
- The `search-page/index.js` renders a `<EPSearchPage/>` component which is mostly driven by a `<WithSearch/>` component expected from `@elastic/react-search-ui`.

### `other/filter-nav`

This component is a horizontal collection of rounded buttons which act like radio buttons, only one selected at a time.

- The component uses `.ux-text` and `.ux-text-title` classes but does not import `@ux/text` styles.

### `other/flexbox`

This component is a layout helper which componentizes the flexbox model.

- The component has several options which handle how to display the children based on media query. The number provided to the specific media query is meant to describe the number of children which should appear in a single row.
- The component removes `padding-left` and `padding-right` and then applies `padding-inline-start` and `padding-inline-end`. While this does change depending on the orientation it is not clear why padding is set to `0` at first.
- There are `background` properties applied without a given background image prop.
- There is a TODO to allow the flexbox children to scroll in certain prop configurations.

### `other/form-group`

There are a few components within this directory.

- The main `formGroupField.js` component is another "switch" component which import several components and renders the proper one based on the `typeName`.
- The `RecaptchaField.js` component is a wrapper for [`react-google-recaptcha`](https://www.npmjs.com/package/react-google-recaptcha).
- The `submit` type returns a `<NSMerchButton/>` button element.
- The `tel` type returns a wrapped `@ux/telephone-input` component `TelephoneField.js` which does some validation and also handles masking using [`libphonenumber-js`](https://www.npmjs.com/package/libphonenumber-js).
- The `dropdown` type provides a `@ux/dropdown` component with a few style overrides.
- The default component returns a `@ux/text-input` lightly overridden to handle focus.
- The root `index.js` provides a standard form composition which are commonly found in modals or cards; stacked fields with a submit button.

### `other/headline-lockup`

This component provides standard headline elements.

- The default tag for the component is `<p/>` and when the default is used `aria-level` and `role="heading"` are added to the element.
- The default size is `h1` and is provided as a classname to the component. The styles are not provided here but in another `style-common`-like file.
- The few overrides added to this component apply `perspective: 66vw` without notes to why; GitLens does not link to the correct PR. It is assumed it had to do with some animation that was applied. No examples seem to display this. 
- There are a list of overrides to the `<sup/>` element when it appears within this component.
- The `index.js` file seems to be a template for the stories to display all the different configurations.

### `other/image`

This directory holds ways of displaying an image. There are three types, `lazy-background-image.js`, `lazy-picture.js`, and the export from `index.js`. All use an `IntersectionObserver` to trigger something from within the component but it is not a uniform need. In some cases it is the foundation for lazy loading, it others it is meant for tracking data.

- `lazy-picture.js` uses the `<picture/>` element where the children provided to the component are meant to be all of the possible sources, `loading` defaults to `lazy` for the `<img/>`.
- `lazy-background-image.js` has identical styled CSS for a `<section/>` and a `<div/>`.
- `index.js` uses a technique to load a small transparent image while the element is not in view. It also can add a `data-responsive` attribute that doesn't seem to have any associated CSS.

### `other/input-bar`

This uses a heavily overridden `@ux/search` and `@ux/button` components. The file also has difficulty with syntax highlighting.

### `other/intent-modal`

This doesn't seem to be production components but instead example lockups of modals that can appear within the experience. `@ux/modal` does not have its styles overridden here but there is a `z-index` of `108000` applied to the parent container. There are two kinds of content.

- `intent-form.js` provides a list of card-like buttons, the `<SelectItem/>` found in `buttons`.
- `intent-thank-you-page.js` provides a larger leading image which uses the `<LazyPicture/>` component along with additional wrapping styles.

### `other/klarna-merchandising`

This component seems to provide Klarna advertising: "4 interest-free payments of $37.50. Learn more". The two files here are separated _mostly_ by the rendering layer and the data layer. The latter manipulates data from Klarna to determine how to display the results in the component.

- It seems that the Klarna assets are not with this component but are provided by Klarna url. In some storybooks, this image failed and would probably benefit from a fallback.

### `other/lazy-video`

This is a single component that uses the same general technique from the `<Image/>` component by using an IntersectionObserver to load when the video is in view.

- The component has a `videoSource` configuration object which can hold multiple sources to be rendered within the component. This was my initial expectation to how the `<picture/>` element would have been handled.

### `other/lead-gen-form-modal`

A specific kind of modal that seems to have several different configurations based on the Storybook. This holds the `lead-gen-form.js` and the `lead-gen-success.js`.

- There is a Google Analytic function for tracking this experience.
- The component ships as a button and modal.
- The form component is mostly data configuration for tracking.
- The success component is mostly displaying a video or image.

### `other/list`

This is a stylized `<ul/>` that can display icons next to each item.

- It uses `.ux-text`, `.ux-text-paragraph`, and `.ux-text-size1` classes without explicitly importing the `@ux/text` package.
- There is usage of the `::before` pseudo however this isn't for leading icons which are handled in React. It seems the `::before` content is only for standard `<ul/>` usage expecting bullets or numbers.

### `other/multi-column-item`

This component seems to be another kind of lockup which expects an image and text.

- There is CSS in this component that specifically targets `data-cy` attributes to change the presentation.
- There are hard coded hex colors within the CSS.

### `other/paragraph`

This is a paragraph component but also includes some additional features.

- There is support to add a tooltip this component.
- When a tooltip is provided, the component renders as a `<div/>` losing semantics.

### `other/planbox-item`

This is a lockup expected to exist within pricing plan options.

- The component imports several components from the library: `actions`, `list`, `merch-button`, `price`, `tooltip`, 
- The majority of the component consists of CSS, mostly media queries that adjust the grid layout.

### `other/price`

This is another lockup component to display pricing. It is one of the components within the `planbox-item` composition.

- The component imports several components from the library: `tooltip`, `tag`, `headline-lockup`, `paragraph`, `klarna-merchandising`.
- A large collection of minute typography margin changes based on tag name and media query.
- There is a check to potentially render a legacy version of the lockup.

### `other/product-list-page`

This folder containers subfolders, some of which contain additional components.

[RETURN TO THIS FOR REVIEW]

### `other/savings-calculator`

This is a collection of components that create an entire experience.

[RETURN TO THIS FOR REVIEW]

### `other/section`

This component seems to handle padding and a few alignment strategies.

- While there's flex alignment properties, it is not clear where `display: flex` is ultimately added.
- There are a few media queries which alter padding values.

### `other/section-container`

This is a different component from `other/section`.

- This component seems to handle dimensions more than space, it has a few `max-width` properties set at different breakpoints.
- It also uses viewport units for margin.
- There is a comment mentioning that the JSX is meant to render a "FOS PlanBox Section", so it is possible this is was a dependency of `plan-box-item` at some point. It is not imported into that component in source.

### `other/section-item`

Another component that we assume is related to sections in some way.

- The component contains some of the most complex conditional CSS-in-JS I've ever seen, mostly attempting to adjust the padding and margins.
- The component is a host for CSS, it does not have any further logic.

### `other/spinner`

This is a wrapper for `@ux/spinner` which adds the ability to center the element in its container and add tracking data for when it appears.

### `other/sso-deck`

This seems to be a wrapper for the `auth-react/sso-deck` component to help apply some additional configuration.

### `other/sso-iframe`

This seems to be an older SSO widget, as it does not import any components from `auth-react`. It also includes a significant amount of logic to handle the authentication handshake.

### `other/tag`

This does not use `@ux/tag`, it is a custom implementation of the component.

- The component uses a `<p/>` tag as it's based, adjusting the display to `inline-flex`.
- It duplicates the efforts of the original `@ux/tag`, while adding a few other types: `new`, `product`, and `freemium` which are styled using other inappropriate intents.

### `other/text-loop`

It was not immediately clear what this component is for. From the name, I assumed it to work like a marquee. However, it includes handling for a video. Finding a single use in the library it is meant for adding text on top of video.

### `other/tooltip`

This component makes use of `@ux/tooltip`, `@ux/dialog`, and `@ux/button` with some adjustments to these components but otherwise a fine implementation.

---

### `navigation/footer`

There are a collection of components found in this directory.

#### `footer/breadcrumbs`

This component renders navigational breadcrumbs. This is an unexpected pattern to be found in a footer, as the purpose of this pattern is to show where the user is. Providing this to a normally congested footer meant for global sitemaps seems misguided.

- The component uses a styled `<ol/>` but does not use the semantic `<nav/>` as a container.
- There is some string manipulation to normalize incoming titles from decoded URIs and update given urls to some hardcoded keys.
- The chevrons are rendered as elements; not CSS pseudos.

#### `footer/currency-selector`

This is a custom dropdown experience but does not use any UXCore component other than icons.

- The component recreates the functionality of a trigger and tray.
- It does not seem to have keyboard navigation for the list based on the source code. In the example, the arrow keys scroll the background.
- There are several areas where colors are hard coded within source.

#### `footer/footer-controls`

This component seems to be a basic composition of a few smaller footer components; `market-selector`, `currency-selector`, `logo-link`, `footer-social-media` along with other components `menu`, `menu-item`, and the `site-search` found in the header components.

- The component makes use of `grid-template-areas` to handle responsive reflow at media queries.
- The component will conditionally render the subcomponents as the configuration requires.

#### `footer/footer-legal`

This component provides the legal messaging for the footer. It is composed of text and links.

- This component has much more CSS than one might expect.
- It also seems to be able to render a menu, however the example does not seem to include one.

#### `footer/footer-menu`

This component is the mega menu of sitemap links found in the footer.

- The top border is a hard coded color.
- The top and bottom padding uses `vh` units.
- There is very little logic to the component itself and uses `navigation/other/menu` components.

#### `footer/footer-social-media`

This renders the given children in an unordered list. It does not contain any social media branded elements or properties itself.

#### `footer/guides-link`

This component is an ad for GoDaddy Guides. It does not seem to render properly in Storybook.

- The component places block level elements within an `<a/>` element. It does not render properly because the `<a/>` element should at least be marked as `display: block;`.
- There are missing intents to describe color for parts of this component.
- The CSS uses `order` to change the visual order of elements for breakpoints.


#### `footer/guides-logo`

This is the logo used within the `footer/guides-link`.

#### `footer/logo-link`

This is an `<a/>` element as a logo for the current site.

- This conditionally renders the GoDaddy logo or the 123Reg logo based on the given `companyName` prop. There are no other options.
- The 123Reg logo is provided by `header/logo-svg/logoreg-dark`

#### `footer/market-selector`

This provides a custom dropdown experience of the list of available markets.

- Like `currency-selector`, it uses a custom dropdown implementation and not anything from UXCore.
- It uses `order`, `z-index`, `position: fixed;`, viewport units, hardcoded colors and inappropriate intents in the CSS.
- Much of the tray logic is repeated here from `currency-selector` including the lack of keyboard navigation.
- Visually, it is not immediately clear that the list is interactive from the example.

#### `footer/newsletter-signup`

This uses a `@ux/text-input` and `@ux/button` but are both heavily overridden.

- There are missing intents to describe color for parts of this component.
- The CSS uses `order` to change the visual order of elements for breakpoints.
- There is logic to change the content of the button when disabled, one of the possible displays is a UXCore `<Ellipsis/>` icon meant to display loading.
- The success message uses `@ux/text` classes without import `@ux/text`.

#### `footer/sales-footer`

This seems to be a composition of many of the larger components found in this directory: `newsletter-signup`, `guides-link`, `footer-menu`, `footer-legal`, `footer-controls`. This also imports a component (`fixed-banner`) from the `page-header`.

#### `footer/selectors`

It is not clear from the name what this is. From the usage in `footer-controls`, it is meant to compose the market and currency selectors.

#### `footer/support-banner`

This is a colored banner which displays support text and phone number.

- Incorrect use of `figure` intent color for background.

#### `header/account-tray`

This is a dropdown with several menu options meant to handle common navigation, profile, and settings. It also handles authentication. There are a few files within this directory that are used together, composing the final component.

- There are no UXCore components (except for icons) imported in these files.
- The `common.js` file holds a custom `<Flyout/>`, `<Overlay/>`, and `<ActClose/>` (close button) among other components.
- There is a hard coded list of `INVERSED_NAME_MARKETS` which includes several Asian markets meant to reverse the the user first and last name.

#### `header/contact-us-tray-panel-group`

This is a button with a flyout that presents different ways of contacting the company.

#### `header/logo-svg`

This holds two logos, one for GoDaddy and one for 123 Reg. These are `.js` files, not `.svg` files.

- The Storybook shows a configuration that allows for `showCountryNameOnLogo` amongst other props, none of the props are found here. They are found in a file located higher in this directory `header/logo.js`.

#### `header/primary-nav-tray`

This seems to be a horizontal navigation layout. There are several files and folders here but the Storybook only shows a list of links.

- Applies classes to the `html` element.
- Creates custom buttons.
- Color inversion done with incorrect intents.
- `z-index` used for layering.
- Hard coded colors.
- Custom close icon (using HTML entity).
- Magic numbers (`20vw`, `23rem`, `12.5rem`, `1.9rem`).
- Viewport units.

#### `header/secondary-nav`

This is a horizontal list of links, able to show flyouts and become sticky to the top of the page.

- `z-index` used for layering.
- Incorrect intent usage.
- Hard coded color.
- Selecting elements via `data-cy` in CSS.
- Viewport units.
- Width media query usage.
- Copy of `scrollToAnchor()` within.
- Hard coded `marginPixels` in JS.
- Creates custom dropdowns.

#### `header/site-search`

This is a button that triggers a drawer from the top of the page with a search field and other links.

- Uses a modified `@ux/button` and `@ux/search`.
- Creates a custom backdrop, using hard coded color.
- Uses `grid-template-areas`.
- Width media query usage.
- Numerous querystring manipulations.
- Selecting elements via `data-cy` in CSS.
- Magic numbers (`0.565rem`, `133%`).
- Uses `@ux/text` classes without importing `@ux/text`.

#### `header/cart.js`

This is a link with a cart icon within. It can show a notification with or without the cart total.

- When the total is present, there is completely different CSS for when it is not present, including the notification's color.
- Magic numbers (`-7px`, `14px`, `-.25rem`, `.85rem`).
- Hard coded `loaderId` within component logic.

#### `header/headerCartConfig.js`

This is a hidden input which sets props for a form submit.

#### `header/help-center-tray.js`

This is a dropdown that provides help links.

- Custom dropdown implementation, local to this component.
- Magic numbers (`.22rem`, `.23rem`, `-2px`, `.8rem`, `20rem`).
- `z-index` used for layering.
- Width media query usage.
- Would benefit from logical properties.
- Custom close button using HTML entity.
- Hard coded colors.

#### `header/logo.js`

This is meant as a wrapper that allows for additional attributes to be applied to logos.

- There is a SVG written in this source file for the GD logo. The 123 Reg logo is written within `./logo-svg/logoreg.js`.

#### `header/notification-tray.js`

This is a dropdown that provides notifications.

- Magic numbers (`.8125rem`, `320px`, `.9375rem`).
- Hard coded color.
- Would benefit from logical properties.
- `z-index` used for layering.

#### `header/primary-nav.js`

This is a composition of many of the elements within the `/header` directory. There is a main container that holds these elements which has its own styles.

- `z-index` used for layering.
- Hard coded color.
- Incorrect intent usage.
- Width media query usage.
- Hard coded private label logic.

#### `header/right-aligned-item.js`

This seems to be a special link component but it does not use any existing components to create the link, only a styled `<a/>`. 

- Hard coded color.
- Width media query usage.

#### `header/skip-navigation.js`

This is a hidden link visible when focused by tabbing for accessibility. It uses a `@ux/button` with custom styling.

- Due to the `transition` property on the `@ux/button`, there is a transition for the button appearing into the window.
- Would benefit from logical properties.
- Would benefit from modern approach: https://benmyers.dev/blog/skip-links/
- Skip target is sometimes determined _after_ rendering, in which case there might be a bug returning an element instead of a `href`.

#### `other/anchor-link-nav`

This is another navigation component which has elements scroll into view when a link is clicked. This does not seem to make use of the CSS only approach of `scroll-behavior: smooth` and anchor links. Instead completing JS calculations and listeners for resize and scroll. The items also use `@ux/text` classes without importing the CSS.

#### `other/menu`

This seems to be another navigation pattern, used mostly within the footer. It seems to be a styled list with no further functionality and lots of width media queries.

#### `other/sticky-nav`

This is another navigation pattern meant to be sticky (other navigation patterns have sticky as an optional prop, so does this one). This seems to have similar behavior to `anchor-link-nav` with a lot less JS (but still using JS for the scroll trigger).

- `z-index` used for layering.
- Hard coded color.
- Incorrect intent usage.
- Width media query usage.
- Magic numbers (`4.375`, `7.5rem`, `0.0625rem`).

