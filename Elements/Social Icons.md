# Social Icons


* * *

Social icons can be added easily using`bdvs_bt_social_menu_shortcode`, with the optional `context` attribute, which adds a class to the social icon container to specific styles to be applied depending on where the icons appear.

The icons will be automatically rendered depending on which social sites have links assigned in the options under _Appearance > Customise > General > Social Links_ (Beaver Builder Theme specific option).

There is normal a level of boilerplate CSS required in order to get basic nice looking icons. The following is an example from the footer of this website - [https://saraholiverproperty.com/](https://saraholiverproperty.com/):

![image-20240425-091346.png](https://bedeviouswebdevelopment.atlassian.net/wiki/download/thumbnails/1314619393/image-20240425-091346.png?version=1&modificationDate=1714036428374&cacheVersion=1&api=v2&width=760&height=213)

The social icons are implemented with the following code:

### HTML

```html
<div class="sop-footer-col order-md-4">
  <div class="sop-footer-social-container d-flex justify-content-center">[bdvs_bt_social_menu_shortcode context="footer"]</div>
</div>
```

### CSS

```css
.fl-social-icons .fa-circle {
  display: none;
}

.fl-social-icons .fa-stack {
  background-color: #BCAC68;
  width: 35px;
  height: 35px;
}

.fl-social-icons .fa-stack i {
  height: 35px;
  transition: all 0.5s;
}

.fl-social-icons .fa-stack i:hover {
  background-color: #D2B89A;
}
```

## Theme-Based Social Icons Component

Create a reusable social icons component with customizable styling and context-aware designs.

### Component Structure:
```
components/
└── view-template/
    └── social-icons/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

### template.html:
```html
<!-- wpv-attribute name="context" label="Context (footer, header, sidebar)" type="text" -->
<!-- wpv-attribute name="icon-size" label="Icon Size (small, medium, large)" type="text" -->
<!-- wpv-attribute name="icon-style" label="Icon Style (circle, square, rounded)" type="text" -->
<!-- wpv-attribute name="alignment" label="Alignment (left, center, right)" type="text" -->

<div class="social-icons-container social-icons-{{context}} social-icons-{{icon-size}} social-icons-{{icon-style}} text-{{alignment}}">
  [bdvs_bt_social_menu_shortcode context="{{context}}"]
</div>
```

### style.css:
```css
/* Base social icons styling */
.social-icons-container .fl-social-icons .fa-circle {
  display: none;
}

.social-icons-container .fl-social-icons .fa-stack {
  transition: all 0.3s ease;
}

.social-icons-container .fl-social-icons .fa-stack i {
  transition: all 0.3s ease;
}

/* Size variations */
.social-icons-small .fl-social-icons .fa-stack {
  width: 25px;
  height: 25px;
}

.social-icons-medium .fl-social-icons .fa-stack {
  width: 35px;
  height: 35px;
}

.social-icons-large .fl-social-icons .fa-stack {
  width: 45px;
  height: 45px;
}

/* Style variations */
.social-icons-circle .fl-social-icons .fa-stack {
  border-radius: 50%;
}

.social-icons-square .fl-social-icons .fa-stack {
  border-radius: 0;
}

.social-icons-rounded .fl-social-icons .fa-stack {
  border-radius: 8px;
}

/* Context-specific styling */
.social-icons-footer .fl-social-icons .fa-stack {
  background-color: #BCAC68;
  color: white;
}

.social-icons-footer .fl-social-icons .fa-stack:hover {
  background-color: #D2B89A;
  transform: translateY(-2px);
}

.social-icons-header .fl-social-icons .fa-stack {
  background-color: transparent;
  color: #333;
  border: 2px solid #333;
}

.social-icons-header .fl-social-icons .fa-stack:hover {
  background-color: #333;
  color: white;
}

.social-icons-sidebar .fl-social-icons .fa-stack {
  background-color: #f8f9fa;
  color: #6c757d;
  border: 1px solid #dee2e6;
}

.social-icons-sidebar .fl-social-icons .fa-stack:hover {
  background-color: #007cba;
  color: white;
  border-color: #007cba;
}

/* Spacing */
.social-icons-container .fl-social-icons li {
  margin: 0 5px;
}

.social-icons-small .fl-social-icons li {
  margin: 0 3px;
}

.social-icons-large .fl-social-icons li {
  margin: 0 8px;
}
```

### Usage Examples

#### Footer Social Icons (Original Example):
```html
<!-- Component attributes -->
context: footer
icon-size: medium
icon-style: circle
alignment: center
```

#### Header Social Icons:
```html
<!-- Component attributes -->
context: header
icon-size: small
icon-style: rounded
alignment: right
```

#### Sidebar Social Icons:
```html
<!-- Component attributes -->
context: sidebar
icon-size: medium
icon-style: square
alignment: left
```

### Database-Based Approach (Legacy)

The original approach using shortcodes directly:

```html
<div class="sop-footer-social-container d-flex justify-content-center">
  [bdvs_bt_social_menu_shortcode context="footer"]
</div>
```

With corresponding CSS:

```css
.fl-social-icons .fa-circle {
  display: none;
}

.fl-social-icons .fa-stack {
  background-color: #BCAC68;
  width: 35px;
  height: 35px;
}

.fl-social-icons .fa-stack i {
  height: 35px;
  transition: all 0.5s;
}

.fl-social-icons .fa-stack i:hover {
  background-color: #D2B89A;
}
```

### Benefits of Theme-Based Approach

1. **Consistency**: Standardized social icon styling across the site
2. **Flexibility**: Easy to customize size, style, and context without code changes
3. **Maintainability**: Update styling in one place for all social icon instances
4. **Responsive**: Built-in responsive considerations for different screen sizes
5. **Accessibility**: Better semantic structure and hover states

---