# Headers

<ht## Key Points

The app* The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the header and it's content, keeping it closely aligned to the CMS The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the header and it's content, keeping it closely aligned to the CMS The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the header and it's content, keeping it closely aligned to the CMSoach can be summarised by these points:

### Database-Based Approach (Original)
* Headers are to created by creating a [Beaver Themer](/wiki/spaces/BDWD/pages/611287049/Beaver+Themer) Header with no margins or padding and containing a BDVS Content Template Module from the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) plugin
* The Toolset Content Template embedded should contain a [Bootstrap Navbar](https://getbootstrap.com/docs/4.6/components/navbar/)
* The [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) contains a number of [helper functions](../../General/BDVS%20Beaver%20Themer.md#shortcodes) and shortcodes that will assist in generating the header and it's content, keeping it closely aligned to the CMS
* Utility classes should be used as much as possible, however, headers are an example of an element where more CSS is likely to be required

### Theme-Based Approach (Recommended)
* Create header components in your theme's `components/view-template/` directory
* Define component structure in `template.html` with attribute definitions
* Add component-specific CSS in `style.css` 
* Components are version-controlled and deploy with your theme
* Better performance and easier maintenance
* Embed using the BDVS Content Template module in Beaver Themer headersdeviouswebdevelopment.atlassian.net/spaces/BDWD/pages/627703809/Headers>

* * *

## Summary

Of course, all sites need headers, and they tend to take a disproportionately large amount of time to get right, especially when trying to adhere to a provided design.

The approach used here allows flexibility enough for most designs to be met, whilst utilising existing and centralised tools as much as possible for ease of maintenance.

Headers can now be created using either the traditional database-based approach or the newer theme-based approach for better version control and maintainability.

## Key Points

The approach can be summarised by these points:

* Headers are to created by creating a [Beaver Themer](/wiki/spaces/BDWD/pages/611287049/Beaver+Themer) Header with no margins or padding and containing a BDVS Content Template Module from the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) plugin
* The Toolset Content Template embedded should contain a [Bootstrap Navbar](https://getbootstrap.com/docs/4.6/components/navbar/)
* The [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) contains a number of [helper functions](../General/BDVS%20Beaver%20Themer.md#Shortcodes) and shortcodes that will assist in generating the header and it’s content, keeping it closely aligned to the CMS
* Utility classes should be used as much as possible, however, headers are an example of an element where more CSS is likely to be required

## CTAs & Special Entries

It is common that a design will call for a CTA menu item, e.g. a _Sign Up_ CTA that stands out from the remainder of the menu items.

This should be handled by creating a new CSS class for CTA menu items, adding the CSS to the CSS section of the Content Template and applying the class to the required menu items by editing the menu under _Appearance > Menus_ in the WordPress admin area.

## Theme-Based Header Example

### Create Header Component

#### 1. Create Component Directory

```bash
mkdir -p wp-content/themes/your-theme/components/view-template/site-header
```

#### 2. Component Template (template.html)

```html
<!-- wpv-attribute name="phone_number" type="text" label="Phone Number" -->
<!-- wpv-attribute name="show_mobile_phone" type="checkbox" label="Show Phone on Mobile" -->
<!-- wpv-attribute name="menu_location" type="text" label="Menu Location" -->
<!-- wpv-attribute name="brand_text" type="text" label="Brand Text (if no logo)" -->

<nav class="site-header-nav navbar navbar-expand-lg navbar-dark flex-column flex-lg-row">
  
  <!-- Optional mobile phone block -->
  <div class="d-flex d-lg-none justify-content-center w-100">
    <a href="tel:{{phone_number}}" class="d-block no-decoration mobile-phone-link">
      <span class="icon-circle d-inline-flex align-items-center justify-content-center rounded-circle bg-white text-primary mr-1">
        <i class="fas fa-phone"></i>
      </span>
      <strong class="text-primary">Call us on</strong> {{phone_number}}
    </a>
  </div>

  <div class="mobile-nav-layout d-flex align-items-center">
    <a class="navbar-brand py-3 py-md-1" href="/">
      [bdvs_bt_site_title_logo]
    </a>

    <div class="d-flex d-md-none justify-content-center">
      <div class="navbar-toggler" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="toggle-bar"></span>
        <span class="toggle-bar"></span>
        <span class="toggle-bar"></span>
      </div>
    </div>
  </div>

  <div class="collapse navbar-collapse justify-content-end" id="navbarSupportedContent">
    [bdvs_bt_menu_shortcode theme_location="{{menu_location}}" menu_class="navbar-nav" container_class=""]
  </div>

</nav>
```

#### 3. Component Styles (style.css)

```css
/* Site Header Component Styles */
.site-header-nav {
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.site-header-nav .navbar-brand {
  font-weight: 700;
  font-size: 1.5rem;
}

.site-header-nav .navbar-nav .nav-link {
  color: #333;
  font-weight: 500;
  padding: 0.75rem 1rem;
  transition: color 0.3s ease;
}

.site-header-nav .navbar-nav .nav-link:hover {
  color: #007bff;
}

.mobile-phone-link {
  text-decoration: none;
  font-size: 16px;
  padding: 0.5rem 1rem;
  color: #333;
}

.icon-circle {
  width: 30px;
  height: 30px;
  font-size: 14px;
}

.navbar-toggler {
  border: none;
  padding: 0.25rem 0.5rem;
  background: transparent;
  cursor: pointer;
}

.toggle-bar {
  display: block;
  width: 25px;
  height: 3px;
  background-color: #333;
  margin: 3px 0;
  transition: 0.3s;
}

/* CTA Menu Item Styles */
.site-header-nav .navbar-nav .menu-item-cta .nav-link {
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  margin-left: 0.5rem;
}

.site-header-nav .navbar-nav .menu-item-cta .nav-link:hover {
  background-color: #0056b3;
  color: white;
}

/* Responsive adjustments */
@media (max-width: 991px) {
  .site-header-nav .navbar-nav .menu-item-cta .nav-link {
    margin-left: 0;
    margin-top: 0.5rem;
    text-align: center;
  }
}
```

#### 4. Component JavaScript (script.js)

```javascript
// Site Header Component JavaScript
document.addEventListener('DOMContentLoaded', function() {
    // Handle navbar toggler animation
    const togglers = document.querySelectorAll('.navbar-toggler');
    
    togglers.forEach(function(toggler) {
        toggler.addEventListener('click', function() {
            const bars = this.querySelectorAll('.toggle-bar');
            bars.forEach(function(bar) {
                bar.classList.toggle('active');
            });
        });
    });
    
    // Handle scroll effects
    const navbar = document.querySelector('.site-header-nav');
    if (navbar) {
        window.addEventListener('scroll', function() {
            if (window.scrollY > 50) {
                navbar.classList.add('scrolled');
            } else {
                navbar.classList.remove('scrolled');
            }
        });
    }
});
```

## Database-Based Header Example: [https://solupak.com/](https://solupak.com/)

### HTML:

```html
<nav class="sp-header-nav navbar navbar-expand-md navbar-dark flex-column flex-lg-row">

  <div class="d-flex d-lg-none justify-content-center w-100">
    <a href="tel:[bdvs_bt_get_theme_mod mod='bdvs-phone-number']" class="d-block no-decoration" style="font-size: 16px; padding: 0 1rem;">
      <span class="sp-icon-circle d-inline-flex align-items-center justify-content-center rounded-circle bg-white sp-green-text mr-1"><i class="fas fa-phone"></i></span>
      <strong class="sp-green-text">Call us on</strong> [bdvs_bt_get_theme_mod mod="bdvs-phone-number"]
    </a>
  </div>

  <div class="sp-mobile-nav-layout d-flex align-items-center">
    <a class="navbar-brand py-3 py-md-1" href="[wpv-bloginfo show='url']">
      [bdvs_bt_site_title_logo]
    </a>

    <div class="d-flex d-md-none justify-content-center">
      <div class="navbar-toggler" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="sp-bg-green"></span>
        <span class="sp-bg-green"></span>
        <span class="sp-bg-green"></span>
      </div>
    </div>
  </div>

  <div class="collapse navbar-collapse justify-content-end flex-wrap" id="navbarSupportedContent">

    <div class="d-none d-lg-flex justify-content-end w-100" style="padding: 0.25rem 1rem;">
      <a href="tel:[bdvs_bt_get_theme_mod mod='bdvs-phone-number']" class="d-block no-decoration" style="font-size: 16px; padding: 0 1rem;">
        <span class="sp-icon-circle d-inline-flex align-items-center justify-content-center rounded-circle bg-white sp-green-text mr-1"><i class="fas fa-phone"></i></span>
        <strong class="sp-green-text">Call us on</strong> [bdvs_bt_get_theme_mod mod="bdvs-phone-number"]
      </a>
    </div>

    [bdvs_bt_menu_shortcode container_class="collapse navbar-collapse justify-content-end" menu_class="navbar nav d-flex flex-column flex-md-row" container_id="navbarSupportedContent"]

  </div>

</nav>
```

### CSS:

```css
header .nav-link {
  font-size: 14px;
  font-weight: 600;
}

header .current-menu-item .nav-link {
  color: #27c6aa;
}

.navbar-toggler {
  cursor: pointer;
  height: 45px;
  position: relative;
  top: -3px;
  -webkit-transform: rotate(0deg);
  -moz-transform: rotate(0deg);
  -o-transform: rotate(0deg);
  transform: rotate(0deg);
  -webkit-transition: .5s ease-in-out;
  -moz-transition: .5s ease-in-out;
  -o-transition: .5s ease-in-out;
  transition: .5s ease-in-out;
  width: 50px;
}

.navbar-toggler span {
  border-radius: 9px;
  display: block;
  height: 7px;
  left: 0;
  opacity: 1;
  position: absolute;
  -webkit-transform: rotate(0deg);
  -moz-transform: rotate(0deg);
  -o-transform: rotate(0deg);
  transform: rotate(0deg);
  -webkit-transition: .25s ease-in-out;
  -moz-transition: .25s ease-in-out;
  -o-transition: .25s ease-in-out;
  transition: .25s ease-in-out;
  width: 100%;
}

.navbar-toggler span:nth-child(1) {
  top: 0px;
}

.navbar-toggler span:nth-child(2) {
  top: 15px;
}

.navbar-toggler span:nth-child(3) {
  top: 30px;
}

.navbar-toggler.open span:nth-child(1) {
  top: 18px;
  -webkit-transform: rotate(135deg);
  -moz-transform: rotate(135deg);
  -o-transform: rotate(135deg);
  transform: rotate(135deg);
}

.navbar-toggler.open span:nth-child(2) {
  left: -60px;
  opacity: 0;
}

.navbar-toggler.open span:nth-child(3) {
  top: 18px;
  -webkit-transform: rotate(-135deg);
  -moz-transform: rotate(-135deg);
  -o-transform: rotate(-135deg);
  transform: rotate(-135deg);
}

@media( min-width: 768px ) {

  .sp-mega-menu .dropdown-menu {
    column-count: 2;
    left: 50%;
    position: fixed;
    top: 225px;
    transform: translate(-50%);
    white-space: nowrap;
  }

}

@media( min-width: 1024px ) {

  .sp-mega-menu .dropdown-menu {
    column-count: 3;
    top: 125px;
  }

}
```

### JavaScript:

```js
jQuery( document ).ready( function() {
	jQuery( '.navbar-toggler' ).click( function() {
		jQuery( this ).toggleClass( 'open' );
	});
});
```

_NB. JavaScript required in this case due to the custom animated hamburger icon, if using the default Boostrap nav hamburger, no JavaScript is required._