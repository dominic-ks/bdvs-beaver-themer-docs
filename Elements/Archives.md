# Archives


* * *

## Summary

Archives follow a different pattern than many other elements in this system of working, as we start here with a Toolset Archive and allow the Toolset system to handle all of the archive queries as well as search / pagination as necessary.

Aside from this, WordPress Archives should be built in much the same way as a Toolset View.

## Setting the Archive Slug

By default WordPress will use the post type as the URL slug for a post type archive but often these are not attractive or user / front end friendly. To edit this, simply edit the post type under Toolset > Post Types.

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/676364289/image-20210318-172840.png?version=1&modificationDate=1616088523088&cacheVersion=1&api=v2)

**NB.** that this step only applies to CPTs and not for WordPress Posts. The posts slug should be set by creating a page with the required title and slug and then setting this page as the blog page under _Settings > Reading > Posts Page_.

## Archive Headers and Footers

Often it is required that an archive page contains some sort of introduction content, e.g. a hero image & title that matches or is similar to the format of the home page or other pages. In these instances the approach, for both content after the header of before the footer:

* Create a new Beaver Themer “Part”
* Assign the part to appear after the header and only on the archive pages that it is required
* In these instances, until a better approach is identified, it is acceptable to enter a page title and other content directly in the Beaver Builder, as it’s not easy to control Archive Titles, for example

### Example Archive Anatomy

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/676364289/New%20Wireframe%201.png?version=1&modificationDate=1616089439267&cacheVersion=1&api=v2)

## Theme-Based Archive Components

While archives primarily use Toolset's built-in functionality, you can create theme-based components for archive headers, footers, and custom archive layouts.

### Archive Header Component

Create a reusable archive header component for consistent styling across different archive pages:

#### Component Structure:
```
components/
└── view-template/
    └── archive-header/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

#### template.html:
```html
<!-- wpv-attribute name="archive-title" label="Archive Title" type="text" -->
<!-- wpv-attribute name="archive-description" label="Archive Description" type="editor" -->
<!-- wpv-attribute name="background-image" label="Background Image URL" type="url" -->
<!-- wpv-attribute name="show-breadcrumbs" label="Show Breadcrumbs" type="checkbox" -->

<div class="archive-header" style="background-image: url('{{background-image}}');">
  <div class="archive-header-overlay">
    <div class="container">
  <!-- Optional breadcrumbs -->
  <nav class="breadcrumbs mb-3">
        <a href="[wpv-bloginfo show='url']">Home</a>
        <span class="separator"> / </span>
        <span class="current">{{archive-title}}</span>
      </nav>
      
      <h1 class="archive-title">{{archive-title}}</h1>
      <div class="archive-description">{{archive-description}}</div>
    </div>
  </div>
</div>
```

#### style.css:
```css
.archive-header {
  position: relative;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  min-height: 300px;
  display: flex;
  align-items: center;
}

.archive-header-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  width: 100%;
}

.archive-header .container {
  position: relative;
  z-index: 2;
  color: white;
  text-align: center;
}

.archive-title {
  font-size: 3rem;
  margin-bottom: 1rem;
  font-weight: 700;
}

.archive-description {
  font-size: 1.25rem;
  max-width: 600px;
  margin: 0 auto;
}

.breadcrumbs a {
  color: rgba(255, 255, 255, 0.8);
  text-decoration: none;
}

.breadcrumbs a:hover {
  color: white;
}

.breadcrumbs .separator {
  color: rgba(255, 255, 255, 0.6);
  margin: 0 5px;
}

.breadcrumbs .current {
  color: white;
}

@media (max-width: 768px) {
  .archive-header {
    min-height: 200px;
  }
  
  .archive-title {
    font-size: 2rem;
  }
  
  .archive-description {
    font-size: 1rem;
  }
}
```

### Archive Card Component

Create a component for individual archive items with consistent styling:

#### Component Structure:
```
components/
└── view/
    └── archive-card/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

#### template.html:
```html
<!-- wpv-attribute name="show-excerpt" label="Show Excerpt" type="checkbox" -->
<!-- wpv-attribute name="show-date" label="Show Date" type="checkbox" -->
<!-- wpv-attribute name="show-author" label="Show Author" type="checkbox" -->
<!-- wpv-attribute name="card-style" label="Card Style (minimal, detailed, featured)" type="text" -->

<article class="archive-card archive-card-{{card-style}}">
  <div class="card-image">
    [wpv-post-featured-image size="medium"]
  </div>
  
  <div class="card-content">
    <h3 class="card-title">
      <a href="[wpv-post-url]">[wpv-post-title]</a>
    </h3>
    
  <!-- Optional excerpt -->
    <div class="card-excerpt">
      [wpv-post-excerpt]
    </div>
    
    <div class="card-meta">
  <!-- Optional date -->
      <span class="card-date">[wpv-post-date]</span>
      
  <!-- Optional author -->
      <span class="card-author">by [wpv-post-author]</span>
    </div>
    
    <a href="[wpv-post-url]" class="card-link">Read More</a>
  </div>
</article>
```

#### style.css:
```css
.archive-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  margin-bottom: 2rem;
}

.archive-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.card-image img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

.card-title {
  margin-bottom: 1rem;
}

.card-title a {
  color: #333;
  text-decoration: none;
  font-weight: 600;
}

.card-title a:hover {
  color: #007cba;
}

.card-excerpt {
  color: #666;
  margin-bottom: 1rem;
  line-height: 1.6;
}

.card-meta {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
  font-size: 0.9rem;
  color: #888;
}

.card-link {
  display: inline-block;
  background: #007cba;
  color: white;
  padding: 0.5rem 1rem;
  text-decoration: none;
  border-radius: 4px;
  transition: background 0.3s ease;
}

.card-link:hover {
  background: #005a87;
  color: white;
}

/* Card style variations */
.archive-card-minimal .card-image {
  display: none;
}

.archive-card-minimal .card-content {
  padding: 1rem;
}

.archive-card-featured {
  border-left: 4px solid #007cba;
}

.archive-card-featured .card-title {
  font-size: 1.5rem;
}

.archive-card-detailed .card-content {
  padding: 2rem;
}

@media (max-width: 768px) {
  .archive-card {
    margin-bottom: 1rem;
  }
  
  .card-content {
    padding: 1rem;
  }
  
  .card-meta {
    flex-direction: column;
    gap: 0.5rem;
  }
}
```

### Benefits of Theme-Based Archive Components

1. **Consistency**: Maintain consistent archive styling across different post types
2. **Flexibility**: Easy to customize archive layouts without editing code
3. **Reusability**: Create archive headers and cards that work across multiple archives
4. **Performance**: Optimized CSS and JavaScript loading
5. **Maintenance**: Centralized styling and structure management

---