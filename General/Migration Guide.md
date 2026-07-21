# Migration Guide: Database to Theme-Based Components

## Overview

This guide helps you migrate existing database-stored content templates to the new theme-based component system. The migration process maintains all functionality while providing better version control, performance, and maintainability.

## Benefits of Migration

- **Version Control**: Components tracked in Git with your theme
- **Performance**: No database queries for component structure
- **Deployment**: Components deploy automatically with theme updates
- **Development Workflow**: Edit with your preferred code editor
- **Backup**: Included in theme backups automatically
- **Team Collaboration**: Easier to share and review changes

## Migration Process

### Step 1: Identify Components to Migrate

1. **List Current Templates**: Go to **Toolset > Content Templates** in WordPress admin
2. **Document Usage**: Note where each template is used in your site
3. **Prioritize**: Start with frequently used or critical components
4. **Backup**: Export current templates as backup

### Step 2: Analyze Template Structure

For each template, identify:

- **HTML Structure**: The main content markup
- **Attributes**: All `[wpv-attribute]` shortcodes used
- **Custom CSS**: Any CSS in the template's Custom CSS section
- **Custom JavaScript**: Any JavaScript in the template's Custom JS section
- **Dependencies**: Other templates or shortcodes referenced

### Step 3: Create Theme Component

#### Create Directory Structure

```bash
# Navigate to your theme directory
cd wp-content/themes/your-theme/

# Create components directory if it doesn't exist
mkdir -p components/view-template

# Create specific component directory
mkdir components/view-template/your-component-name
```

#### Convert Template Content

1. **Extract HTML**: Copy the main HTML content from the database template
2. **Convert Attributes**: Transform `[wpv-attribute]` shortcodes to HTML comments
3. **Update References**: Change attribute references from shortcode format to `{{attribute_name}}`
4. **Extract CSS**: Move custom CSS to `style.css`
5. **Extract JavaScript**: Move custom JavaScript to `script.js`

### Step 4: Conversion Examples

#### Database Template Example

```html
<!-- Original Database Template -->
<div class="hero-section" style="background-color: [wpv-attribute name='bg-color' label='Background Color' type='color'];">
  <div class="container">
    <h1>[wpv-attribute name='title' label='Hero Title' type='text']</h1>
    <p>[wpv-attribute name='subtitle' label='Subtitle' type='textarea']</p>
    <a href="[wpv-attribute name='cta-url' label='CTA URL' type='link']" class="btn btn-[wpv-attribute name='btn-style' label='Button Style' type='select' options='primary,secondary']">
      [wpv-attribute name='cta-text' label='CTA Text' type='text']
    </a>
  </div>
</div>
```

#### Converted Theme Component

**template.html**
```html
<!-- wpv-attribute name="bg_color" type="color" label="Background Color" -->
<!-- wpv-attribute name="title" type="text" label="Hero Title" -->
<!-- wpv-attribute name="subtitle" type="textarea" label="Subtitle" -->
<!-- wpv-attribute name="cta_url" type="link" label="CTA URL" -->
<!-- wpv-attribute name="btn_style" type="select" label="Button Style" options="primary,secondary" -->
<!-- wpv-attribute name="cta_text" type="text" label="CTA Text" -->

<div class="hero-section" style="background-color: {{bg_color}};">
  <div class="container">
    <h1>{{title}}</h1>
    <p>{{subtitle}}</p>
    <a href="{{cta_url}}" class="btn btn-{{btn_style}}">{{cta_text}}</a>
  </div>
</div>
```

**style.css**
```css
.hero-section {
  padding: 100px 0;
  min-height: 500px;
  display: flex;
  align-items: center;
}

.hero-section .container {
  text-align: center;
}

.hero-section h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: white;
}

.hero-section p {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  color: rgba(255, 255, 255, 0.9);
}
```

### Step 5: Handle Complex Scenarios

#### Form Fields and Repeaters

**Database Version:**
```html
[bdvs_bt_form_repeater elements=[wpv-attribute name='items' label='Items' type='form' multiple='true' form='{"name":"title","label":"Title","type":"text"}|{"name":"desc","label":"Description","type":"textarea"}']]
<div class="item">
  <h3>{{title}}</h3>
  <p>{{desc}}</p>
</div>
[/bdvs_bt_form_repeater]
```

**Theme Version:**
```html
<!-- wpv-attribute name="items" type="form" label="Items" multiple="true" form='{"name":"title","label":"Title","type":"text"}|{"name":"desc","label":"Description","type":"textarea"}' -->

[bdvs_bt_form_repeater elements={{items}}]
<div class="item">
  <h3>{{title}}</h3>
  <p>{{desc}}</p>
</div>
[/bdvs_bt_form_repeater]
```

#### Embedded Templates

**Database Version:**
```html
[bdvs_bt_content_template id="123" title="Custom Title" description="Custom Description"]
```

**Theme Version:**
```html
<!-- wpv-attribute name="button_title" type="text" label="Button Title" -->
<!-- wpv-attribute name="button_description" type="textarea" label="Button Description" -->

[bdvs_bt_content_template id="theme_single_button" title="{{button_title}}" description="{{button_description}}"]
```

### Step 6: Testing and Validation

#### Test Component Functionality

1. **Create Test Page**: Add component to a test page using Beaver Builder
2. **Verify Attributes**: Ensure all attributes appear in the module settings
3. **Test Styling**: Confirm CSS loads correctly
4. **Test JavaScript**: Verify any JavaScript functionality works
5. **Responsive Testing**: Check component on different screen sizes

#### Common Issues and Solutions

**Attributes Not Appearing:**
- Check HTML comment syntax in `template.html`
- Verify attribute names don't contain special characters
- Clear any caching

**CSS Not Loading:**
- Confirm `style.css` file exists and is readable
- Check file permissions
- Verify CSS selectors are correct

**JavaScript Not Working:**
- Ensure `script.js` file is properly formatted
- Check browser console for errors
- Verify DOM ready handlers

### Step 7: Deployment and Cleanup

#### Deploy Components

1. **Add to Version Control**: Commit component files to Git
2. **Deploy to Staging**: Test components in staging environment
3. **Update Production**: Deploy components to production
4. **Update Templates**: Replace database templates with theme components

#### Clean Up Database Templates

1. **Backup First**: Export database templates before deletion
2. **Update Pages**: Replace old components with new theme-based ones
3. **Remove Unused**: Delete unused database templates
4. **Documentation**: Update any documentation referencing old templates

## Migration Checklist

### Pre-Migration
- [ ] Inventory all existing content templates
- [ ] Document template usage across the site
- [ ] Backup current templates
- [ ] Set up theme components directory structure

### During Migration
- [ ] Convert template HTML structure
- [ ] Transform attribute definitions
- [ ] Extract and organize CSS
- [ ] Extract and organize JavaScript
- [ ] Test component functionality
- [ ] Verify responsive behavior

### Post-Migration
- [ ] Update all pages using migrated components
- [ ] Remove unused database templates
- [ ] Update documentation
- [ ] Train team on new workflow
- [ ] Set up Git workflow for component updates

## Best Practices for Migration

### Naming Conventions

- **Component Names**: Use lowercase with hyphens (e.g., `hero-section`, `contact-form`)
- **Attribute Names**: Use lowercase with underscores (e.g., `button_text`, `bg_color`)
- **CSS Classes**: Prefix with component name to avoid conflicts

### File Organization

```
components/view-template/
├── hero-section/
│   ├── template.html
│   ├── style.css
│   └── script.js
├── contact-form/
│   ├── template.html
│   └── style.css
└── testimonial-slider/
    ├── template.html
    ├── style.css
    └── script.js
```

### Version Control

- **Commit Often**: Make small, focused commits for each component
- **Descriptive Messages**: Use clear commit messages describing changes
- **Branch Strategy**: Consider feature branches for major component updates
- **Documentation**: Include README files for complex components

### Performance Optimization

- **Minimize CSS**: Keep component CSS focused and minimal
- **Optimize JavaScript**: Only include necessary JavaScript functionality
- **Image Optimization**: Optimize any images used in components
- **Lazy Loading**: Consider lazy loading for JavaScript-heavy components

## Troubleshooting Common Issues

### Template Not Found
- Verify directory structure matches `components/view-template/component-name/`
- Check file permissions on template files
- Clear any WordPress caching

### Attributes Not Working
- Verify HTML comment syntax for attribute definitions
- Check attribute names for special characters or spaces
- Ensure template.html file is readable

### Styling Issues
- Confirm style.css file exists and has proper CSS syntax
- Check for CSS conflicts with theme or other plugins
- Verify CSS selectors target the correct elements

### JavaScript Errors
- Check browser console for JavaScript errors
- Verify script.js syntax is correct
- Ensure DOM elements exist before JavaScript tries to access them

## Getting Help

If you encounter issues during migration:

1. **Check Documentation**: Review the [Theme-Based Components](/docs/./Theme-Based%20Components.md) guide
2. **Validate Syntax**: Use online validators for HTML, CSS, and JavaScript
3. **Test Incrementally**: Migrate one component at a time
4. **Use Browser DevTools**: Debug CSS and JavaScript issues in the browser
5. **Consult Team**: Work with other developers familiar with the system

## Conclusion

Migrating to theme-based components provides significant benefits for development workflow, performance, and maintainability. While the initial migration requires effort, the long-term benefits make it worthwhile for any project using the BDVS Beaver Themer plugin.

Start with simple components to get familiar with the process, then gradually migrate more complex templates. The improved development experience and deployment workflow will quickly justify the migration effort.
