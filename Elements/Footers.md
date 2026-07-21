# Footers


* * The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the footer and it's content, keeping it closely aligned to the CMS The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the footer and it's content, keeping it closely aligned to the CMS *

## Summary

Footers, like headers, seem to be among those elements that take a disproportionate amount of time for what they are. Also like headers, they require more custom CSS that other elements. The approach is the same as for headers.

## Key Points

The approach can be summarised by these points:

* Footers are to created by creating a [Beaver Themer](../../General/Beaver%20Themer.md) Header with no margins or padding and containing a BDVS Content Template Module from the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) plugin
* The [BDVS Beaver Themer](../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../General/BDVS%20Beaver%20Themer.md#Shortcodes) and shortcodes that will assist in generating the footer and it’s content, keeping it closely aligned to the CMS
* Utility classes should be used as much as possible, however, headers are an example of an element where more CSS is likely to be required
* [Responsive mobile first principles](../../General/CSS%20&%20Utility%20Classes.md) are very often important here as footers tend to contain multiple oddly sized columns with different content in each column.

## Different Page, Different Footer?

Occasionally a site will call for slightly different footers on different pages, perhaps a site has a [large scale footer](#large-scale-footers) but one page does not. In this case the following approach should be taken:

* Create a Beaver Themer Footer layout as described above
* Save the row containing the BDVS Content Template module, ensuring it is saved globally
* Create an additional Beaver Themer Footer layout and name it appropriately
* Assign the footers based on rules by editing the layouts in the WordPress admin area, the rules will depend on the specific case

## Example: [https://solupak.com/](https://solupak.com/)

### Database-Based Approach (Legacy)

#### HTML:

```html
<div class="sp-footer-container text-center text-md-left py-5">
  <div class="sp-footer-inner d-md-flex">

    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner">
        <div class="wp-footer-col-sub mb-2">
          <h3>[wpv-attribute name="col-1-title" label="Column 1 Title" type="text"]</h3>
          [bdvs_bt_menu_shortcode depth="1" menu_class="sp-footer-menu d-flex flex-column align-items-center align-items-md-start"]
        </div>
      </div>
    </div>
 
    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner"> 

        <div class="wp-footer-col-sub mb-2">
          <h3>[wpv-attribute name="col-2-title" label="Column 2 Title" type="text"]</h3>      
          [wpv-attribute name="col-2-content" label="Column 2 Content" type="editor"]
        </div>

        <div class="wp-footer-col-sub mb-2">
          <div>Call us on</div>
          <div>[bdvs_bt_get_theme_mod mod="bdvs-phone-number"]</div>
        </div>

        <div class="wp-footer-col-sub mb-2">
          <a class="sp-green-text" href="mailto:[bdvs_bt_get_theme_mod mod='bdvs-email-address']">Email us</a>
        </div>

      </div>
    </div>

    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner">

        <div class="wp-footer-col-sub mb-2">
          <h3>[wpv-attribute name="col-3-title" label="Column 3 Title" type="text"]</h3>
          [wpv-attribute name="col-3-content" label="Column 3 Content" type="editor"]
        </div>

        <div class="wp-footer-col-sub mb-2 sp-blue-text">
          © [wpv-bloginfo] [bdvs_bt_copyright_year]
        </div>

      </div>
    </div>

  </div>
</div>
```

### Theme-Based Approach (Recommended)

Create a footer component in your theme directory structure:

#### Component Structure:
```
components/
└── view-template/
    └── solupak-footer/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

#### template.html:
```html
<!-- wpv-attribute name="col-1-title" label="Column 1 Title" type="text" -->
<!-- wpv-attribute name="col-2-title" label="Column 2 Title" type="text" -->
<!-- wpv-attribute name="col-2-content" label="Column 2 Content" type="editor" -->
<!-- wpv-attribute name="col-3-title" label="Column 3 Title" type="text" -->
<!-- wpv-attribute name="col-3-content" label="Column 3 Content" type="editor" -->

<div class="sp-footer-container text-center text-md-left py-5">
  <div class="sp-footer-inner d-md-flex">

    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner">
        <div class="wp-footer-col-sub mb-2">
          <h3>{{col-1-title}}</h3>
          [bdvs_bt_menu_shortcode depth="1" menu_class="sp-footer-menu d-flex flex-column align-items-center align-items-md-start"]
        </div>
      </div>
    </div>
 
    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner"> 

        <div class="wp-footer-col-sub mb-2">
          <h3>{{col-2-title}}</h3>      
          {{col-2-content}}
        </div>

        <div class="wp-footer-col-sub mb-2">
          <div>Call us on</div>
          <div>[bdvs_bt_get_theme_mod mod="bdvs-phone-number"]</div>
        </div>

        <div class="wp-footer-col-sub mb-2">
          <a class="sp-green-text" href="mailto:[bdvs_bt_get_theme_mod mod='bdvs-email-address']">Email us</a>
        </div>

      </div>
    </div>

    <div class="sp-footer-col d-md-flex justify-content-center w-md-33">
      <div class="sp-footer-col-inner">

        <div class="wp-footer-col-sub mb-2">
          <h3>{{col-3-title}}</h3>
          {{col-3-content}}
        </div>

        <div class="wp-footer-col-sub mb-2 sp-blue-text">
          © [wpv-bloginfo] [bdvs_bt_copyright_year]
        </div>

      </div>
    </div>

  </div>
</div>
```

#### style.css:
```css
.sp-footer-container {
  font-size: 16px;
}

.sp-footer-menu {
  list-style-type: none !important;
  padding-left: 0 !important;
}

.sp-footer-container .nav-link {
  padding-bottom: 2px;
  padding-left: 0;
  padding-right: 0;
  padding-top: 2px;
}

footer {
  position: relative;
  top: -2px;
}
```

### Shared CSS Between Approaches:

```css
.sp-footer-container {
  font-size: 16px;
}

.sp-footer-menu {
  list-style-type: none !important;
  padding-left: 0 !important;
}

.sp-footer-container .nav-link {
  padding-bottom: 2px;
  padding-left: 0;
  padding-right: 0;
  padding-top: 2px;
}

footer {
  position: relative;
  top: -2px;
}
```

## Large Scale Footers

On some sites, though they have what would be considered a traditional footer, they can additionally have a considerable amount of additional content that appears before the footer on every page, therefore you could consider this to be part of the footer. In these cases, these rows should be part of the Beaver Themer Footer element.

### Example: [https://solupak.com/](https://solupak.com/)

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/650608641/footer-example.png?version=2&modificationDate=1615311312816&cacheVersion=1&api=v2)