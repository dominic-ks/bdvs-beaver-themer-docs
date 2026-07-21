# Butt## Summary

Buttons* C* C* Create a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button" Create a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button"eate a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button"
* Use the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method to add attributes such as _url destination, button text, button type, link target_
* Add [CSS](../../General/CSS%20&%20Utility%20Classes.md) to the _Custom CSS_ section of the WordPress Customiser
* Insert the Content Template using [Beaver Builder](../../General/Beaver%20Builder.md) adding the relevant detailseate a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button"eate a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button"
* Use the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method to add attributes such as _url destination, button text, button type, link target_
* Add [CSS](../../General/CSS%20&%20Utility%20Classes.md) to the _Custom CSS_ section of the WordPress Customiser
* Insert the Content Template using [Beaver Builder](../../General/Beaver%20Builder.md) adding the relevant details a simple element but one that will inevitably be used lots of times across a site and so it's important to keep them consistent. The BDVS Beaver Themer plugin now supports both database-based and theme-based content templates for buttons. The approach is summarised below:

### Database-Based Approach (Original)
* Create a [Content Template](../../General/Toolset.md#content-templates) and name it "Single Button"
* Use the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) method to add attributes such as _url destination, button text, button type, link target_
* Add [CSS](/wiki/spaces/BDWD/pages/622002177/CSS+Utility+Classes) to the _Custom CSS_ section of the WordPress Customiser
* Insert the Content Template using [Beaver Builder](/wiki/spaces/BDWD/pages/610992129/Beaver+Builder) adding the relevant details

### Theme-Based Approach (Recommended)
* Create button components in your theme's `components/view-template/` directory
* Define component structure in `template.html` with attribute definitions
* Add component-specific CSS in `style.css`
* Components are version-controlled and deploy with your theme
* Better performance and easier maintenance

See the [Theme-Based Components](/docs/../General/Theme-Based%20Components.md) documentation for detailed information.tps://bedeviouswebdevelopment.atlassian.net/spaces/BDWD/pages/703463451/Buttons>

* * *

## Summary

Buttons are a simple element but one that will inevitably be used lots of times across a site and so it’s important to keep them consistent. The approach for buttons in the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) method is summarised below:

* Create a [Content Template](../General/Toolset.md#Content-Templates) and name it “Single Button”
* Use the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) method to add attributes such as _url destination, button text, button type, link target_
* Add [CSS](/wiki/spaces/BDWD/pages/622002177/CSS+Utility+Classes) to the _Custom CSS_ section of the WordPress Customiser
* Insert the Content Template using [Beaver Builder](/wiki/spaces/BDWD/pages/610992129/Beaver+Builder) adding the relevant details

## Theme-Based Button Components

### Single Button Component

Create a theme-based button component for better version control and deployment.

#### 1. Create Component Directory

```bash
mkdir -p wp-content/themes/your-theme/components/view-template/single-button
```

#### 2. Component Template (template.html)

```html
<!-- wpv-attribute name="button_text" type="text" label="Button Text" -->
<!-- wpv-attribute name="destination" type="link" label="Link Destination" -->
<!-- wpv-attribute name="button_type" type="select" label="Button Type" options="primary,secondary" -->
<!-- wpv-attribute name="target" type="select" label="Target" options="_self,_blank" -->
<!-- wpv-attribute name="button_align" type="select" label="Button Align" options="start,center,end" -->

<div class="single-btn-container d-flex justify-content-{{button_align}}">
  <a href="{{destination}}" class="btn btn-{{button_type}}" target="{{target}}">
    {{button_text}}
  </a>
</div>
```

#### 3. Component Styles (style.css)

```css
/* Single Button Component Styles */
.single-btn-container .btn {
  border-radius: 0;
  transition: all 0.5s;
  padding: 12px 24px;
  font-weight: 600;
  text-decoration: none;
  display: inline-block;
}

/* Primary Button Styles */
.single-btn-container .btn-primary {
  background-color: black;
  border: 2px solid black;
  color: white;
}

.single-btn-container .btn-primary:active,
.single-btn-container .btn-primary:focus,
.single-btn-container .btn-primary:hover {
  background-color: white;
  color: black;
  text-decoration: none;
}

/* Secondary Button Styles */
.single-btn-container .btn-secondary {
  background-color: white;
  border: 2px solid black;
  color: black;
}

.single-btn-container .btn-secondary:active,
.single-btn-container .btn-secondary:focus,
.single-btn-container .btn-secondary:hover {
  background-color: black;
  color: white;
  text-decoration: none;
}
```

### Multiple Buttons Component

#### 1. Create Component Directory

```bash
mkdir -p wp-content/themes/your-theme/components/view-template/multiple-buttons
```

#### 2. Component Template (template.html)

```html
<!-- wpv-attribute name="button_align" type="select" label="Alignment" options="start,center,end,between,around" -->
<!-- wpv-attribute name="buttons_form" type="form" label="Button" multiple="true" form='{"name":"btn-text","label":"Label","type":"text"}|{"name":"btn-destination","label":"Link","type":"link"}|{"name":"btn-bg","label":"Type","type":"select","options":"grey-orange,peach,ceramic,vanilla,slate-grey"}|{"name":"btn-text-color","label":"Button Text Color","type":"select","options":"light,dark,ceramic,vanilla"}|{"name":"btn-hover-color","label":"Hover Colour","type":"select","options":"transparent,grey-orange,peach,ceramic,vanilla"}|{"name":"btn-target","label":"Target","type":"select","options":"_self,_blank"}' -->

<div class="multiple-buttons-container">
  <div class="buttons-inner d-flex flex-column flex-md-row align-items-center justify-content-{{button_align}}">
    [bdvs_bt_form_repeater elements={{buttons_form}}]
    <div class="single-btn-container w-100 w-md-auto">
      <a href="{{btn-destination}}" class="btn btn-lg btn-bg-{{btn-bg}} btn-text-color-{{btn-text-color}} btn-hover-{{btn-hover-color}} w-100 w-md-auto" target="{{btn-target}}" rel="noopener">{{btn-text}}</a>
    </div>
    [/bdvs_bt_form_repeater]
  </div>
</div>
```

#### 3. Component Styles (style.css)

```css
/* Multiple Buttons Component Styles */
.multiple-buttons-container {
  margin: 20px 0;
}

.multiple-buttons-container .buttons-inner {
  gap: 15px;
}

.multiple-buttons-container .btn {
  border-radius: 0;
  transition: all 0.5s;
  font-weight: 600;
  text-decoration: none;
  border: 2px solid transparent;
}

/* Button Background Colors */
.multiple-buttons-container .btn-bg-grey-orange {
  background-color: #8B4513;
}

.multiple-buttons-container .btn-bg-peach {
  background-color: #FFCBA4;
}

.multiple-buttons-container .btn-bg-ceramic {
  background-color: #F5F5DC;
}

.multiple-buttons-container .btn-bg-vanilla {
  background-color: #F3E5AB;
}

.multiple-buttons-container .btn-bg-slate-grey {
  background-color: #708090;
}

/* Button Text Colors */
.multiple-buttons-container .btn-text-color-light {
  color: white;
}

.multiple-buttons-container .btn-text-color-dark {
  color: #333;
}

.multiple-buttons-container .btn-text-color-ceramic {
  color: #F5F5DC;
}

.multiple-buttons-container .btn-text-color-vanilla {
  color: #F3E5AB;
}

/* Hover Effects */
.multiple-buttons-container .btn-hover-transparent:hover {
  background-color: transparent;
  border-color: currentColor;
}

.multiple-buttons-container .btn-hover-grey-orange:hover {
  background-color: #8B4513;
  color: white;
}

.multiple-buttons-container .btn-hover-peach:hover {
  background-color: #FFCBA4;
  color: #333;
}

.multiple-buttons-container .btn-hover-ceramic:hover {
  background-color: #F5F5DC;
  color: #333;
}

.multiple-buttons-container .btn-hover-vanilla:hover {
  background-color: #F3E5AB;
  color: #333;
}
```

## Database-Based Approach (Legacy)

### Multiple Button Example (Database-Based)

#### Create a Content Template

```html
[wpv-attribute name='' label='' type='']
<div class="sop-buttons-container">
  <div class="sop-buttons-inner d-flex flex-column flex-md-row align-items-center justify-content-[wpv-attribute name='button-align' label='Alignment' type='select' options='start,center,end,between,around']">

    [bdvs_bt_form_repeater elements=[wpv-attribute name='buttons-form' label='Button' type='form' multiple='true' form='{"name":"btn-text","label":"Label","type":"text"}|{"name":"btn-destination","label":"Link","type":"link"}|{"name":"btn-bg","label":"Type","type":"select","options":"grey-orange,peach,ceramic,vanilla,slate-grey"}|{"name":"btn-text-color","label":"Button Text Color","type":"select","options":"light,dark,ceramic,vanilla"}|{"name":"btn-hover-color","label":"Hover Colour","type":"select","options":"transparent,grey-orange,peach,ceramic,vanilla"}|{"name":"btn-target","label":"Target","type":"select","options":"_self,_blank"}']]

    <div class="single-btn-container w-100 w-md-auto">
      <a href="{{btn-destination}}" class="btn btn-lg btn-bg-{{btn-bg}} btn-text-color-{{btn-text-color}} btn-hover-{{btn-hover-color}} w-100 w-md-auto" target="{{btn-target}}" rel="noopener">{{btn-text}}</a>
    </div>

    [/bdvs_bt_form_repeater]

  </div>
</div>
```

### Single Button Example (Database-Based)

#### Create a Content Template

```html
<div class="single-btn-container d-flex justify-content-[wpv-attribute name='button-align' label='Button Align' type='select' options='start,center,end']">
  <a href="[wpv-attribute name='destination' label='Link Destination' type='link']" class="btn btn-[wpv-attribute name='button-type' label='Button Type' type='select' options='primary,secondary']" target="[wpv-attribute name='target' label='Target' type='select' options='_self,_blank']">
    [wpv-attribute name='button-text' label="Button Text" type='text']
  </a>
</div>
```

This content template uses four wpv-attribute shortcodes, but you could add any number as needed depending on your case:

|     |     |
| --- | --- |
| **Shortcode** | **Description** |
| `[wpv-attribute name='destination' label="Link Destination" type='link']` | The URL that the button will link to |
| `[wpv-attribute name='button-text' label="Button Text" type='text']` | The text that will be displayed on the button |
| `[wpv-attribute name='button-type' label='Button Type' type='select' options='primary,secondary']` | A choice of _primary_ or _secondary_ that will be appended to _btn-_ in order to generate a bootstrap button class, e.g. `btn-primary` |
| `[wpv-attribute name='target' label='Target' type='select' options='_self,_blank']` | A choice of __self_ or __blank_ to determine if the link will open in a new tab or not. Note that __self_ is given first as this will be the default |

#### Add CSS to the Custom CSS (Database-Based)

You can overwrite properties of the bootstrap button classes as required

```css
/**my btn overrides**/
.btn {
  border-radius: 0;
  transition: all 0.5s; 
}

/**set my primary styles**/
.btn-primary {
  background-color: black;
  border: 2px solid black;
  color: white;
}

/**set the same styles to active, focus and hover**/
.btn-primary:active,
.btn-primary:focus,
.btn-primary:hover {
  background-color: white;
  color: black;
}

/**set my secondary styles**/
.btn-secondary {
  background-color: white;
  border: 2px solid black;
  color: black;
}

/**set the same styles to active, focus and hover**/
.btn-secondary:active,
.btn-secondary:focus,
.btn-secondary:hover {
  background-color: black;
  color: white;
}
```

#### CSS Notes:

* Buttons should always have transition effects
* Borders should be set on the default style if they are required for the hover effect where they match the initial background colour

### Add the Button Using Beaver Builder

Adding the BDVS Content Template module in Beaver Builder and selecting _Single Button_ will give you a form that looks like this:

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/703463451/image-20210324-124831.png?version=1&modificationDate=1616590115258&cacheVersion=1&api=v2)

## Other Contexts

It may be the case that buttons are required to be used in other contexts other than just placing a single button using the page builder, these could be:

* A set of two buttons: create an additional “_Dual Button”_ Content Template
* As an action button in another Content Template, View or Archive: simply include the markup in those elements entering the desired values as opposed to the _wpv-attribute_ shortcodes