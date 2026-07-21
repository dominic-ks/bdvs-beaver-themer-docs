# BDVS Beaver Themer

* * *

## Contents

## Summary

The BDVS Beaver Themer plugin allows you to build completely custom, reusable elements for a WordPress website that are embeddable using Beaver Builder and that contain content that is easily manageable for non-developer users.

The plugin now supports two approaches for creating content templates:

### Database-Based Templates (Original)
- Content Templates are created and stored in the WordPress database
- Managed through the WordPress admin interface
- Suitable for non-developer users

### Theme-Based Templates (Recommended)
- Content Templates are stored as files in your active theme
- Better version control and deployment workflow
- Improved performance and maintainability
- Ideal for development teams

For detailed information on theme-based templates, see the [Theme-Based Components](/docs/./Theme-Based%20Components.md) documentation.

## The Process

### Database-Based Approach

The basic premise of the database-based process can be viewed using the following gif:

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/593100813/bdvs_bt_demo_01_20210222.gif?version=1&modificationDate=1614014767644&cacheVersion=1&api=v2)

As you can see the following steps are required:

* Create a Toolset Content Template
* Add any HTML, CSS and JavaScript required
* Add wpv-attribute shortcodes to create user input fields and link data
* Add a BDVS Content Template module to a page or template using Beaver Builder
* Enter content as applicable

### Theme-Based Approach

For theme-based components:

* Create component directory in your theme's `components/view-template/` folder
* Create `template.html` with your HTML structure and attribute definitions
* Create `style.css` with component-specific CSS (optional)
* Create `script.js` with component-specific JavaScript (optional)
* Add a BDVS Content Template module to a page using Beaver Builder
* Select your theme-based component and configure attributes

## [wpv-attribute] Shortcode

One of the key aims in this process is to create re-usable modules, so where ever there is an attribute in a Content Template that needs to be configurable by the user, a wpv-attribute shortcode should be used to enable the option in Beaver Builder.

This shortcode is part of the Toolset Views plugin, but has been adapted for use for this case. The shortcode takes 3 attributes:

* `name` - the name of the attribute, must be unique to the Content Template _unless_ you specifically want the same field to be used twice with the same content or data. In this case, you should put two identical shortcodes in the template, though only one field will appear in the Beaver Builder module.
* `label` - the label that you want to display next to the field in the Beaver Builder module
* `type` - the type of field to display in the Beaver Builder module, potentially any of the field types supported by Beaver Themer could be used here (see link below) however these are the most commonly used and tested:
    * color
    * editor
    * form
    * link
    * multiple-photo
    * photo
    * select
    * text
* `options` - a comma separated list of options to display in a select field when `type` is set to “select”

### References

* Beaver Themer Field Reference - [https://docs.wpbeaverbuilder.com/beaver-builder/developer/custom-modules/cmdg-10-setting-fields-reference](https://docs.wpbeaverbuilder.com/beaver-builder/developer/custom-modules/cmdg-10-setting-fields-reference)

## Theme-Based Attribute Syntax

For theme-based components, the syntax is slightly different. Instead of using shortcodes within the content, attributes are defined as HTML comments at the top of the `template.html` file:

```html
<!-- wpv-attribute name="title" type="text" label="Component Title" -->
<!-- wpv-attribute name="description" type="textarea" label="Description" -->
<!-- wpv-attribute name="button_text" type="text" label="Button Text" -->
<!-- wpv-attribute name="button_url" type="link" label="Button URL" -->
<!-- wpv-attribute name="button_style" type="select" label="Button Style" options="primary,secondary" -->
```

Then in the template body, reference these attributes using double curly braces:

```html
<div class="my-component">
  <h2>{{title}}</h2>
  <p>{{description}}</p>
  <a href="{{button_url}}" class="btn btn-{{button_style}}">{{button_text}}</a>
</div>
```

### Benefits of Theme-Based Syntax

- **Version Control**: Attributes are defined in files, making them trackable in Git
- **Performance**: No need to parse shortcodes during runtime
- **Cleaner Templates**: Separation of attribute definitions and template structure
- **IDE Support**: Better syntax highlighting and autocomplete in code editors

## Examples

Here are some examples of the most commonly used field types, each example shows just one linked field, in reality there may be many linked fields in a template and the static content shown below would be replaced with a wpv-attribute shortcode allowing user managed content.

### Color

Use for adding colours to templates can be added to the style attribute of an HTML element

```html
<p style="color: #[wpv-attribute name='color' label='Colour' type='color'];">
  Change the colour of this text
</p>
```

### Editor

Use where longer or rich content is needed, e.g. with formatting and images

```java
<div class="some-content">
  [wpv-attribute name='content' label='Content' type='editor']
</div>
```

### Form

A bug exists currently that means form type fields will only work as long as there is at least one other “normal” `wpv-attribute` shortcode in the Content Template. If your content template has no other fields, you can simply enter a blank one at the start of the template, i.e.

`[wpv-attribute name='' label='' type='']`

Forms are the most complex field type and gives the ability to allow users to enter multiple sets of data consisting of multiple fields when used in conjunction with `multiple="true"` shortcode att. Examples of use could include:

* A table, where the form allows entering one row of data per instance
* Testimonials, where the form allows entering details of one testimonial per instance
* A menu, where the form allows entering of one menu item per instance

As the repeatable form also requires it’s own set of field definitions, this also needs to be entered into the shortcode in a slightly different way, which _can_ look a little messy, but it’s totally worth it.

**Example:** A simple list:

```html
<ul class="">
  [bdvs_bt_form_repeater elements=[wpv-attribute name='form-field' label='Form Field' type='form' multiple='true' form='{"name":"label","label":"Label","type":"text"}|{"name":"value","label":"Value","type":"text"}']]
  <li><strong>{{label}}:</strong> {{value}}</li>
  [/bdvs_bt_form_repeater]
</ul>
```

A breakdown of the `wpv-attribute` shortcode atts:

* `name` - name of the field as usual
* `label` - label of the field as usual
* `type` - type of the field as usual, in this case set to `form`
* `multiple` - set to `true` in order to allow multiple
* `form` - a pseudo-json array detailing the fields for the form. Note that each field contains `name`, `label` and `type` and can in theory support any values that any other field can, except that a form cannot contain another form. Each field object is separated with a pipe - | - character.

The `wpv-attribute` shortcode itself is passed to the `elements` attribute of the `bdvs_bt_form_repeater` shortcode. The contents of which will be printed to the screen once for each instance of the form.

Within the template you will notice two parameters enclosed in Angular style `{{ }}` brackets. When the template is rendered these tags will be replaced with the values assigned to the fields with those keys.

### Link

Used to generate a URL for a link and allows selection of pages and posts etc.

```html
<a href="[wpv-attribute name='link' label='Link Address' type='link']" class="my-link">
  My Link
</a>
```

### Multiple Photo

Used to allow users to select multiple images and will output a string of comma separated image IDs. Use in conjunction with the `bdvs_bt_element_repeater` and `bdvs_bt_image_url` to list image URLs for sliders, for example:

```html
[bdvs_bt_element_repeater elements="[wpv-attribute name='images' label='Images' type='multiple-photos']"]
  <li>[bdvs_bt_image_url item="$element_id"]</li>
[/bdvs_bt_element_repeater]
```

The `wpv-attribute` shortcode itself is passed to the `elements` attribute of the `bdvs_bt_form_repeater` shortcode. The contents of which will be printed to the screen once for each instance of the field.

Within the template you will notice a parameter `$element_id`. When the template is rendered this will be replaced with the ID of the media item that was selected which may be passed to another shortcode, as it is in this example.

### Photo

Used to get the URL of an image from the Media Library

```java
<img src="[wpv-attribute name='image' label='Image' type='photo']" />
```

### Select

Used to allow selection of pre-determined options, examples could include link targets or classes including utility classes

```html
<a href="#" target="[wpv-attribute name='target' label='Target' type='select' options='_self,_blank']">
  My Link
</a>

<div class="[wpv-attribute name='color-class' label='Colour' type='select' options='text-white,text-dark']">
  Here's some content...
</div>
```

### Text

Use for general short text strings and headings

```html
<p>[wpv-atribute name='text' label='Text' type='text']</p>
```

## The `[bdvs_bt_content_template]` Shortcode

In order to make the most of the content templates, it’s possible to embed a content template inside another template, view or archive by using the `[bdvs_bt_content_template]` shortcode.

For example:

```html
[bdvs_bt_content_template id="150" lr-direction="row-reverse" lr-content="testing"]
```

Here, the `id` is the ID of the content template that you are embedding. Both `lr-direction` and `lr-content` are present as `name` attributes in a `[wpv-attribute]` shortcode in the target template, i.e.

```java
[wpv-attribute name="lr-direction" type="select" label="Direction" options="row,row-reverse"]
[wpv-attribute name="lr-content" type="editor" label="Content"]
```

In this case the target content template will be rendered and the passed values will be inserted.

## Theme Mods

There are some bits of information that are useful to store centrally to a site so that it’s not repeated requiring multiple updates if changed. Typical examples include:

* A company’s phone number
* A company’s contact email address

In the [shortcodes table](#shortcodes), the `[bdvs_bt_get_theme_mod]` shortcode allows printing this information in content or Content Templates, however some php is required in order to allow administrator users to enter this information in the customiser. The code should be added to the functions.php file in the Beaver Builder Child Theme, here is an example:

```php
/**
*
* Add company info to customiser
*
**/

function bdvs_customize_register( $wp_customize ) {

  //here the mod name for the shortcode is bdvs-phone-number
  $wp_customize->add_setting( 'bdvs-phone-number', array());
  $wp_customize->add_control( new WP_Customize_Control( $wp_customize , 'bdvs-phone-number-control' , array(
      'label'      => __( 'Phone Number', 'bdvs' ),
      'section'    => 'title_tagline',
      'settings'   => 'bdvs-phone-number',
      'priority'   => 50,
    ))
  );

  //here the mod name for the shortcode is bdvs-email-address
  $wp_customize->add_setting( 'bdvs-email-address', array());
  $wp_customize->add_control( new WP_Customize_Control( $wp_customize , 'bdvs-email-address-control' , array(
      'label'      => __( 'Email Address', 'bdvs' ),
      'section'    => 'title_tagline',
      'settings'   => 'bdvs-email-address',
      'priority'   => 50,
    ))
  );
  
}

add_action( 'customize_register' , 'bdvs_customize_register' );
```

## Utility Classes

The plugin includes a small number of utility classes, largely aimed at plugging a few gaps in Bootstrap.

* `.cover-background` - Applies the following and is used to set the background for image backgrounds:
    * `background-position: center;`
    * `background-size: cover;`
* `.d-hidden` - sets the height to 0 and overflow to hidden to hide elements
* `.py-100` - sets the lower and upper padding to 100px, useful for supporting columns that are empty, except for a background image to ensure they keep height on mobile
* A number of responsive width classes:

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| **Width** | **< 576px** | **768px and up** | **1024px and up** | **1200px and up** |
| auto | w-auto | w-md-auto | w-lg-auto | w-xl-auto |
| 25% | w-25 | w-md-25 | w-lg-25 | w-xl-25 |
| 33% | w-33 | w-md-33 | w-lg-33 | w-xl-33 |
| 50% | w-50 | w-md-50 | w-lg-50 | w-xl-50 |
| 75% | w-75 | w-md-75 | w-lg-75 | w-xl-75 |
| 100% | w-100 | w-md-100 | w-lg-100 | w-xl-100 |

## Shortcodes

The following shortcodes are included in the plugin and are useful for adding CMS content to Content Templates.

|     |     |     |
| --- | --- | --- |
| **Shortcode** | **Attributes and Defaults** | **Description** |
| bdvs_bt_site_title_logo | N/A | Will output the site title or the site header logo as defined in the Customiser under _Header > Header Logo_<br><br>**Example:**<br><br>`[bdvs_bt_site_title_logo]` |
| bdvs_bt_menu_shortcode | $container_class = 'bdvs-menu-container'<br><br>$container_id = ''<br><br>$depth = 0<br><br>$echo = false<br><br>$theme_location = false<br><br>$menu_class = 'bdvs-menu'<br><br>$menu_id = '' | Create a WordPress menu.<br><br>All of the shortcode args are passed to the wp_nav_menu function with their corresponding parameter names.<br><br>**Example:**<br><br>`[bdvs_bt_menu_shortcode container_class="" container_id="" depth="" echo="" theme_location="" menu_class="" menu_id=""]` |
| bdvs_bt_get_theme_mod | $mod = N/A | Outputs the value of a theme mod, i.e. a custom value that is added via the Customiser, see [theme mods](#theme-mods), and allows CMS management of strings such as a company phone number or email address.<br><br>**Example:**<br><br>`[bdvs_bt_get_theme_mod mod="bdvs-phone-number"]` |
| bdvs_bt_social_menu_shortcode | $context = 'nav' | Outputs social media icons as defined in the Customiser under _General > Social Links_.<br><br>**Example:**<br><br>`[bdvs_bt_social_menu_shortcode context="nav"]` |
| bdvs_bt_copyright_year | N/A | Outputs the current year, generally for adding the copyright year to a footer.<br><br>**Example:**<br><br>`[bdvs_bt_copyright_year]` |
| bdvs_bt_image_url | $item = 0<br><br>$size = ‘full’ | Outputs the URL of an image when provided with the image ID as `item` and an optional size.<br><br>**Example**<br><br>`[bdvs_bt_image_url item="1"]` |
| bdvs_bt_element_repeater | $elements = null<br><br>**$content** = null<br><br>$deliminator = #~ | Takes a list of `$elements` which must be a comma separated string, it will loop each of these elements and output the shortcode content replacing any instances of `$element_id` with the element.<br><br>**Example**<br><br>```java<br><ul><br>  [bdvs_bt_element_repeater elements="Hello,World!"]<br>    <li>"$element_id"</li><br>  [/bdvs_bt_element_repeater] <br></ul><br>```<br><br>Will output:<br><br>```html<br><ul><br>  <li>Hello</li><br>  <li>World!</li><br></ul><br>``` |
| bdvs_bt_content_template | $id = null<br><br>[…$args] | Render the Content Template with ID `$id` and pass in any provided `$args`.<br><br>**Example**<br><br>`[bdvs_bt_content_template id="150" lr-direction="row-reverse" lr-content="testing"]` |

## Known Issues and Gotchas

When adding a content template to a page for the first time, any CSS or JavaScript may not be loaded initially, you may need to refresh the screen.

A bug exists currently that means form type fields will only work as long as there is at least one other “normal” `wpv-attribute` shortcode in the Content Template. If your content template has no other fields, you can simply enter a blank one at the start of the template, i.e.

`[wpv-attribute name='' label='' type='']`

It’s not currently possible to embed one BDVS Content Template inside another, the plugin will not identify the fields in the subcomponent.

When using the `bdvs_bt_element_repeater` shortcode or the `wpv-attribute`, subsequently using `$element_id` or another `{{field}}` does not work in the `src` attribute in an `img` tag, use `style="background-image: url({{field}});"` instead.

Be careful adding the same fields to multiple content templates, they may be over-written by each other. For example, it may be useful to include the following element and shortcode in multiple templates:

`<button class="btn btn-primary">[wpv-attribute name='button-text' label='Button Text' type='text']</button>`

This is fine and will work. However, if you include these two shortcodes separately:

`[wpv-attribute name='button-text' label='Button Text' type='text']`

`[wpv-attribute name='button-text' label='Button Text' type='editor']`

One may be over-written by the other and you may not get the field type you expect.