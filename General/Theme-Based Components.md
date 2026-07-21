# Theme-Based Components

## Overview

The BDVS Beaver Themer plugin now supports theme-based content templates and views, allowing developers to create reusable components directly in the active theme's file structure. This approach provides better version control, easier deployment, and improved performance compared to database-stored templates.

## Benefits of Theme-Based Components

- **Version Control**: Components are stored as files in your theme, making them trackable in Git
- **Deployment**: Components deploy with your theme, ensuring consistency across environments
- **Performance**: No database queries needed to load component structure
- **Development Workflow**: Edit components with your preferred code editor
- **Backup**: Components are automatically included in theme backups

## Directory Structure

Theme-based components should be placed in your active theme's `components` directory:

```
wp-content/themes/your-theme/
└── components/
    ├── view-template/
    │   ├── component-name/
    │   │   ├── template.html
    │   │   ├── style.css
    │   │   └── script.js
    │   └── another-component/
    │       ├── template.html
    │       ├── style.css
    │       └── script.js
    └── view/
        ├── view-name/
        │   ├── template.html
        │   ├── style.css
        │   └── script.js
        └── another-view/
            ├── template.html
            ├── style.css
            └── script.js
```

## Component Types

### View Templates (Content Templates)
Located in `components/view-template/`, these are reusable content components that can be inserted into pages and posts.

### Views
Located in `components/view/`, these are typically used for displaying dynamic content like post listings, archives, etc. Views include two important files:

- `view.php` - Contains the WP_Query definition that loads the posts or custom content
- `template.php` - Contains the template with special tags to define the loop section

## File Structure

Each component directory must contain at minimum a template file. For view templates, this is `template.html`. For views, both `view.php` and `template.php` are required. The `style.css` and `script.js` files are optional for both.

### Required Files
- `template.html` - The HTML structure and content of the component

### Optional Files  
- `style.css` - Component-specific CSS styles
- `script.js` - Component-specific JavaScript functionality

## Using Theme-Based Components

### 1. Creating a Component

Create a new directory in the appropriate location:

```bash
mkdir wp-content/themes/your-theme/components/view-template/my-component
```

### 2. Component Template (template.html)

Create the main template file with your HTML structure:

```html
<!-- wpv-attribute name="title" type="text" label="Component Title" -->
<!-- wpv-attribute name="content" type="textarea" label="Component Content" -->
<!-- wpv-attribute name="button_text" type="text" label="Button Text" -->
<!-- wpv-attribute name="button_url" type="link" label="Button URL" -->

<div class="my-component">
  <h2 class="component-title">{{title}}</h2>
  <div class="component-content">{{content}}</div>
  <a href="{{button_url}}" class="component-button">{{button_text}}</a>
</div>
```

### 3. Component Styles (style.css)

Add component-specific CSS:

```css
.my-component {
  padding: 30px;
  background-color: #f7f7f7;
  border-radius: 5px;
  margin-bottom: 20px;
}

.component-title {
  color: #333;
  margin-bottom: 15px;
}

.component-content {
  margin-bottom: 20px;
}

.component-button {
  display: inline-block;
  padding: 10px 20px;
  background-color: #0078d7;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.component-button:hover {
  background-color: #005a9e;
}
```

### 4. Component JavaScript (script.js)

Add any required JavaScript functionality:

```javascript
// Component-specific JavaScript
document.addEventListener('DOMContentLoaded', function() {
    // Initialize component functionality
    const components = document.querySelectorAll('.my-component');
    
    components.forEach(function(component) {
        // Add component-specific behavior
        const button = component.querySelector('.component-button');
        if (button) {
            button.addEventListener('click', function(e) {
                // Custom click handling if needed
                console.log('Component button clicked');
            });
        }
    });
});
```

## Attributes and Dynamic Content

### Defining Attributes

Use HTML comments to define component attributes at the top of your `template.html` file:

```html
<!-- wpv-attribute name="attribute_name" type="field_type" label="Display Label" -->
```

### Available Attribute Types

- `text` - Single line text input
- `textarea` - Multi-line text input  
- `link` - URL input with link picker
- `select` - Dropdown selection (use `options` parameter)
- `checkbox` - Boolean checkbox
- `number` - Numeric input
- `email` - Email input
- `tel` - Phone number input
- `date` - Date picker

### Select Field Options

For select fields, define options using the `options` parameter:

```html
<!-- wpv-attribute name="button_style" type="select" label="Button Style" options="primary,secondary,outline" -->
```

### Using Attributes in Templates

Reference attributes in your template using double curly braces:

```html
<div class="component {{style_class}}">
  <h2>{{title}}</h2>
  <p>{{description}}</p>
  <a href="{{link_url}}" class="btn btn-{{button_style}}">{{button_text}}</a>
</div>
```

## Creating Views with Loop Sections

Views allow you to display dynamic content like post listings. The template for a view must include special comment tags to define which part of the template should repeat for each post in the query.

### View PHP Files

A view component requires two primary PHP files:

#### 1. view.php

This file defines your WP_Query object that will load the posts:

```php
<?php
// Get settings from attributes
$posts_per_page = isset($posts_per_page) ? intval($posts_per_page) : 3;
$post_type = isset($post_type) ? $post_type : 'post';

// Build query arguments
$args = array(
    'post_type' => $post_type,
    'posts_per_page' => $posts_per_page,
    'post_status' => 'publish'
);

// If a category is specified, add it to the query
if (isset($category) && !empty($category)) {
    $args['category_name'] = $category;
}

// Create the WP_Query object
$query = new WP_Query($args);
```

#### 2. template.php

This file contains your HTML template with special comment tags to define the loop section:

```php
<?php
/**
 * Template file for the recent-posts view
 */

// Add wpv-attribute comments to define settings for this view
// <!-- wpv-attribute name="posts_per_page" type="number" label="Number of Posts" -->
// <!-- wpv-attribute name="post_type" type="select" label="Post Type" options="post,page,product" -->
// <!-- wpv-attribute name="heading" type="text" label="Heading Text" -->
// <!-- wpv-attribute name="container_class" type="text" label="Container CSS Class" -->
?>

<div class="recent-posts-container <?php echo isset($container_class) ? esc_attr($container_class) : ''; ?>">
  <?php if (isset($heading) && !empty($heading)): ?>
    <h2 class="recent-posts-heading"><?php echo esc_html($heading); ?></h2>
  <?php endif; ?>
  
  <div class="posts-grid">
  
  <!-- BDVS LOOP START -->
  <article class="post-item">
    <h3><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h3>
    <?php the_excerpt(); ?>
    <a href="<?php the_permalink(); ?>">Read More</a>
  </article>
  <!-- BDVS LOOP END -->
  
  </div>
  
  <?php if (isset($show_pagination) && $show_pagination === 'yes'): ?>
  <div class="pagination">
    <?php echo paginate_links(array('total' => $wp_query->max_num_pages)); ?>
  </div>
  <?php endif; ?>
</div>
```

### Loop Tags Explained

- `<!-- BDVS LOOP START -->`: Marks the beginning of the content that will repeat for each post
- `<!-- BDVS LOOP END -->`: Marks the end of the repeating content

Everything before the `BDVS LOOP START` tag and after the `BDVS LOOP END` tag will be rendered once, while the content between these tags will be rendered for each post in the query results.

### Important Notes About Views

- The `view.php` file must define a variable named `$query` that is a valid WP_Query object
- WordPress template tags like `the_title()` and `the_content()` can be used inside the loop section
- Attributes are available both inside and outside the loop section
- The system automatically handles post context within the loop

## Advanced Examples

### Complex Component with Multiple Attributes

```html
<!-- wpv-attribute name="heading" type="text" label="Heading" -->
<!-- wpv-attribute name="subheading" type="text" label="Subheading" -->
<!-- wpv-attribute name="description" type="textarea" label="Description" -->
<!-- wpv-attribute name="image_url" type="link" label="Image URL" -->
<!-- wpv-attribute name="button_text" type="text" label="Button Text" -->
<!-- wpv-attribute name="button_url" type="link" label="Button URL" -->
<!-- wpv-attribute name="button_style" type="select" label="Button Style" options="primary,secondary,outline" -->
<!-- wpv-attribute name="alignment" type="select" label="Content Alignment" options="left,center,right" -->
<!-- wpv-attribute name="show_image" type="checkbox" label="Show Image" -->

<div class="feature-card text-{{alignment}}">
  <!-- Optional: image block (no built-in conditionals; control visibility via CSS/classes or leave fields empty) -->
  <div class="feature-image">
    <img src="{{image_url}}" alt="{{heading}}" class="img-fluid">
  </div>
  
  <div class="feature-content">
    <h2 class="feature-heading">{{heading}}</h2>
    <h3 class="feature-subheading">{{subheading}}</h3>
    <p class="feature-description">{{description}}</p>
    
    <!-- Optional: button block (no built-in conditionals) -->
    <a href="{{button_url}}" class="btn btn-{{button_style}}">{{button_text}}</a>
  </div>
</div>
```

## Component Discovery

The plugin automatically discovers theme-based components by scanning the `components` directory structure. Components are identified by:

1. **Directory Name**: Used as the component slug/identifier
2. **template.html**: Required file containing the component structure
3. **style.css**: Optional CSS styles
4. **script.js**: Optional JavaScript functionality

## Overrides and Core Components

The plugin ships with a small set of core components bundled inside the plugin at `components/`. Your theme can override any core component by creating a component with the same slug in the theme.

- Resolution order: Theme first, then plugin core
- Paths scanned:
  - Theme: `wp-content/themes/your-theme/components/{view-template|view}/<slug>/`
  - Plugin: `wp-content/plugins/bdvs-beaver-themer/components/{view-template|view}/<slug>/`
- The UI labels items as “Theme: …” or “Core: …” to indicate the source

To override, copy a core component folder from the plugin into your theme’s `components/` keeping the same slug, then modify safely.

## Integration with Beaver Builder

Theme-based components integrate seamlessly with Beaver Builder:

1. **BDVS Content Template Module**: Use the existing module to insert theme-based components
2. **Component Selection**: Theme-based components appear alongside database-stored templates
3. **Attribute Configuration**: Configure component attributes through the Beaver Builder interface
4. **Live Preview**: See changes in real-time while editing

## Best Practices

### Naming Conventions

- **Component Names**: Use lowercase with hyphens (e.g., `hero-section`, `feature-card`)
- **CSS Classes**: Prefix with component name to avoid conflicts
- **Attribute Names**: Use descriptive names with underscores (e.g., `button_text`, `background_color`)

### File Organization

```
components/view-template/
├── buttons/
│   ├── template.html
│   └── style.css
├── hero-sections/
│   ├── template.html
│   ├── style.css
│   └── script.js
└── cards/
    ├── template.html
    └── style.css
```

### CSS Scoping

Always scope your CSS to avoid conflicts:

```css
/* Good - Scoped to component */
.hero-section {
  background: #f0f0f0;
}

.hero-section .hero-title {
  font-size: 2rem;
}

/* Avoid - Global styles */
.title {
  font-size: 2rem;
}
```

### Performance Considerations

- Keep CSS and JavaScript minimal and component-specific
- Use efficient selectors
- Minimize the number of attributes per component
- Consider lazy loading for JavaScript-heavy components

## Migration from Database Templates

To convert existing database-stored templates to theme-based components:

1. **Export Template**: Copy the HTML content from the database template
2. **Create Component Directory**: Create the appropriate folder structure
3. **Create template.html**: Paste the content and add attribute definitions
4. **Extract CSS**: Move custom CSS to `style.css`
5. **Extract JavaScript**: Move custom JavaScript to `script.js`
6. **Test Component**: Verify the component works as expected

## Troubleshooting

### Component Not Appearing

- Verify the directory structure is correct
- Ensure `template.html` or `template.php` exists and is readable
- Check file permissions
- Clear any caching

### Attributes Not Working

- Verify attribute definitions are in HTML comments
- Check attribute syntax and spelling
- Ensure attributes are referenced correctly in template

### View Loop Issues

- Make sure your template contains both `<!-- BDVS LOOP START -->` and `<!-- BDVS LOOP END -->` tags
- Verify that your `view.php` file creates a valid WP_Query object named `$query`
- Check that you're using WordPress template tags inside the loop section
- Ensure the loop tags are not nested inside other PHP control structures

### Styling Issues

- Check CSS file is being loaded
- Verify CSS selectors are correct
- Ensure CSS is scoped properly
- Check for conflicts with theme or other plugins

## Development Workflow

### For View Templates

1. **Create Component**: Set up directory and files
2. **Define Attributes**: Add attribute definitions to template
3. **Build Template**: Create HTML structure with attribute placeholders
4. **Add Styles**: Create component-specific CSS
5. **Add JavaScript**: Add any required functionality
6. **Test Component**: Verify in Beaver Builder
7. **Refine**: Iterate on design and functionality
8. **Document**: Add usage notes for other developers

### For Views

1. **Create Component Directory**: Set up directory structure for the view
2. **Create view.php**: Define the WP_Query to load data
3. **Create template.php**: Build the template with loop tags
4. **Define Attributes**: Add attribute definitions to the template
5. **Add Wrapper HTML**: Add content that appears before and after the loop
6. **Define Loop Content**: Add content that repeats for each post
7. **Add Styles**: Create component-specific CSS
8. **Add JavaScript**: Add any required functionality
9. **Test Component**: Verify the loop works correctly
10. **Document**: Add usage notes for other developers
