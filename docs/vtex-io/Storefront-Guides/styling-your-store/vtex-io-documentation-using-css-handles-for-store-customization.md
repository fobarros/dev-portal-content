---
title: "Using CSS Handles for store customization"
slug: "vtex-io-documentation-using-css-handles-for-store-customization"
hidden: false
createdAt: "2020-06-03T16:02:45.024Z"
updatedAt: "2022-12-13T20:17:44.398Z"
---

## Introduction

CSS Handles is a **HTML elements identifier**. Therefore, it can be used to customize any of your store components by simply using CSS classes through the store theme's code.

At the end of the day, CSS Handles are nothing more than your **store's layout building assistants**. Follow the step-by-step below and find out how to use them in a fast and simple way!

## Step by step

1. While you are viewing your workspace on your browser, add `?__inspect` at the end of the URL of the page, and press enter. For example, `https://yourworkspace--youraccount.myvtex.com?__inspect`. Note that it must be a development workspace, rather than a production one, and it must be under the domain `myvtex.com`.

2. Hover on the element you want to customize. It should display its available CSS handles in a box (the large names beginning with `.`), along with their respective CSS file names, and other information.

![css-handles-inspect](https://cdn.jsdelivr.net/gh/vtexdocs/dev-portal-content@main/images/vtex-io-documentation-using-css-handles-for-store-customization-0.png)

> ⚠️ Before proceeding to the third step, check the CSS Handles table in the documentation of the app/block responsible for rendering the HTML element. It will therefore be possible to confirm whether the inspected Handle is valid and mainly if the customization requires another add-on to function, such as the HTML element's attribute.

> ⚠️ If upon checking the CSS Handles table, you notice that the desired Handle needs to use the attribute of the HTML element that will be customized, inspect the page again - traditionally this time, by using your browser and not the `?__inspect` functionality. Upon inspecting the page, look for the desired HTML element and copy the attribute linked to it that will be used in the upcoming customization.

3. Open the Store Theme code using the code editor of your preference. Then, create a file inside the css folder named after the text displayed below the desired handle. Following the example above, we would have `vtex.menu.css`.

4. In the new file, use one of the CSS handles listed and customize its properties. For example:

```css
.menuItem {  
    background: rgba(0, 0, 0, 0.2);
    margin: 5px;
    border-radius: 5px;
}
```

> ℹ️ Remember: if the Handle requests an add-on, such as the HTML element's attribute or a Handle modifier, add it next to the Handle's name, following this format: `{cssHandleName}--{addon)`.

Once you app is linked and the changes duly saved, the new customization should immediately be reflected onto your workspace.

![css handles customization applied to the menuItem](https://cdn.jsdelivr.net/gh/vtexdocs/dev-portal-content@main/images/vtex-io-documentation-using-css-handles-for-store-customization-1.png)

As we've seen, CSS Handles are used to overwrite a store's default style and thus independently customize a type of block from the rest of the theme. Note that the change above was applied to all `menu-item` blocks.

To independently customize a single `menu-item` block, you should use the  `blockClass` prop.

`blockClass` is a property whose value you may define freely. When passed onto a block, it serves as its single **identifier** for customization.

### Using the blockClass property

1. In the `json` file where your block is declared, add the prop `blockClass` to the element you want to customize, with any name as a value.

For example:

```json
"menu-item#your-item": {
  "props": {
    ...,
    "blockClass": "header"
  },
  ...
}
```

After saving and updating your workspace, when you inspect the element it should display an additional CSS handle along with its default one, like this:

![css handles with block class](https://cdn.jsdelivr.net/gh/vtexdocs/dev-portal-content@main/images/vtex-io-documentation-using-css-handles-for-store-customization-2.png)

After that, you can use the class `.menuItem--header` to target specifically the elements that have this `blockClass`.

![specific menu items with css handles applied using blockClass](https://cdn.jsdelivr.net/gh/vtexdocs/dev-portal-content@main/images/vtex-io-documentation-using-css-handles-for-store-customization-3.png)

> ℹ️  Our team is constantly working on developing CSS Handles for every possible store component. If however your are unable to find a CSS Handle for a component you wish to customize, share this scenario with us at [Store Discussion](https://github.com/vtex-apps/store-discussion).

## Best practices

In order to standardize CSS customization and avoid any potential breakdown in layout, **we recommend store customization to be performed exclusively using CSS Handles**.

However, it's common to come across store scenarios whose customization uses CSS Selectors. As the name implies, they are used to select elements for CSS customization according to the page HTML hierarchy.

This customization practice by HTML hierarchy was mostly deprecated. It means that **only** the CSS Selectors listed below will continue to be allowed for your store customization:

- Class selectors (e.g. `.foo`)
- Pseudo-selectors `:hover`, `:visited`, `:active`, `:disabled`, `:focus`, `:local`, `:empty`, and `:target`
- `:not()`
- `:first-child` and `:last-child`
- `:nth-child(even)`, `:nth-child(odd)`, and `:nth-child(2n)` (or any other step like `4n`, `5n`, etc)
- All pseudo-elements, such as  `::before`, `::after` and `::placeholder`
- Space combinator (e.g. `.foo .bar`)
- `[data-...]`
- `:global(vtex-{AppName}-{AppVersion}-{ComponentName})` for selection of elements that come from different apps

**Any CSS Selectors not on this list, such as** `:nth-child(2)`**,** `foo > bar` **and** `[alt="bar"]`**, it's not accepted by VTEX IO CLI during the [linking](https://developers.vtex.com/docs/guides/vtex-io-documentation-linking-an-app) of the store's theme with its local files.**

> ⚠️ Bear in mind that any customization that uses CSS Selectors is dependent on a HTML structure that, when changed, can break the retailer's desired customization. **Always opt to use CSS Handles**.
