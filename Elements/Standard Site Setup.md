# Standard Site Setup


* * *

## Summary

Each WordPress site should have the same initial setup which is documented on this page

## Theme and Plugins

The following themes are required on every site:

* Beaver Builder Theme
* Beaver Builder Child Theme

The following plugins should be installed and activated on every site:

* Beaver Builder
* Beaver Themer
* Toolset Types
* Toolset Views
* Toolset Forms
* BDVS Beaver Themer

For other suitable and first choice plugins, please refer to [Tools & Plugins](../../General/Tools%20&%20Plugins.md#core-plugins)

## Settings

The following settings should be applied to any new WordPress install:

* Default _Hello World_ post and comment to be deleted
* Default _Sample Page_ page to renamed to “Home” (also update the slug to “home”)
* Site title and tagline to be set under _Settings > General_
* Site Language set to _English_ under _Settings > General_
* Site Time Zone set to _London_ under _Settings > General_
* Set the home page to a static page and choose _Home_ (unless something else is required)
* Check the _Users must be registered and logged in to comment_ option under _Settings > Discussion_
* _Permalinks set to Post Name under Settings > Permalinks_

## Theme-Based Components Setup

For sites using the new theme-based components functionality:

### Directory Structure
Create the following directory structure in your active theme:

```
components/
├── view-template/     # For content templates
└── view/             # For view templates  
```

### Initial Components
Consider creating these standard components for consistent site development:

1. **Header Components**
   - `components/view-template/site-header/`
   - `components/view-template/page-hero/`

2. **Content Components**
   - `components/view-template/content-section/`
   - `components/view-template/call-to-action/`
   - `components/view-template/testimonial/`

3. **Footer Components**
   - `components/view-template/site-footer/`
   - `components/view-template/contact-info/`

4. **Form Components**
   - `components/view-template/contact-form/`
   - `components/view-template/newsletter-signup/`

### File Permissions
Ensure the web server can read the components directory:
```bash
chmod -R 755 wp-content/themes/your-theme/components/
```

### Development Workflow
1. Create components in the theme's components directory
2. Test components using the BDVS Content Template module
3. Version control components with your theme files
4. Deploy components as part of your theme deployment

### Migration from Database Templates
If migrating from existing database-stored templates:
1. Export existing templates from the database
2. Convert to file-based structure following the [Migration Guide](../../General/Migration%20Guide.md)
3. Test thoroughly before removing database templates
4. Update documentation and team workflows