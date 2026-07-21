# Latest Posts / Content


* * *

## Summary

A common element for brochure sites is a “_Latest Posts”_ or other CPT, e.g. Case Studies, and often appear on the home page of a website.

## Approach

The general approach is to create a [Toolset View](../../General/Toolset.md#views) and insert into a page using the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method.

## Example

Wrap the `<wpv-loop></wpv-loop>` tags with a container and inner in the View, and use responsive utility classes to manage the layout

```html
<div class="cc-featured-portfolio-items-container">
  <div class="cc-featured-portfolio-items-inner d-md-flex">
  
    <wpv-loop>
      <!-- per post content goes here -->
    </wpv-loop>
    
  </div>
</div>
```

User a content template if the layout is complex:

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/726269983/image-20210329-165714.png?version=1&modificationDate=1617037036575&cacheVersion=1&api=v2)

If there are some differences between individual elements, use the `[wpv-item]` shortcode [as described here](https://toolset.com/documentation/programmer-reference/views/views-shortcodes/#vf-154400).

```html
<div class="cc-featured-portfolio-items-container">
  <div class="cc-featured-portfolio-items-inner d-md-flex">
  
    <wpv-loop>
    
      [wpv-item index=1]
        <div class="item-1-class">
          [wpv-body name="my-content-template"]
        </div>
    
      [wpv-item index=other]
        <div class="other-item-class">
          [wpv-body name="my-content-template"]
        </div>
    
    </wpv-loop>
    
  </div>
</div>
```

Add CSS and JavaScript as usual.

## Theme-Based Latest Posts Components

Create reusable components for displaying latest posts or custom post types with consistent styling and flexible layouts.

### Latest Posts Grid Component

#### Component Structure:
```
components/
└── view-template/
    └── latest-posts-grid/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

#### template.html:
```html
<!-- wpv-attribute name="section-title" label="Section Title" type="text" -->
<!-- wpv-attribute name="section-description" label="Section Description" type="editor" -->
<!-- wpv-attribute name="posts-per-row" label="Posts Per Row (2, 3, 4)" type="text" -->
<!-- wpv-attribute name="show-read-more" label="Show Read More Button" type="checkbox" -->
<!-- wpv-attribute name="view-shortcode" label="Toolset View Shortcode" type="text" -->

<div class="latest-posts-container">
  <div class="section-header text-center mb-5">
    <h2 class="section-title">{{section-title}}</h2>
    <div class="section-description">{{section-description}}</div>
  </div>
  
  <div class="posts-grid posts-grid-{{posts-per-row}}">
    {{view-shortcode}}
  </div>
  
  <!-- Optional: Read more button -->
  <div class="text-center mt-4">
    <a href="/blog" class="btn btn-primary">View All Posts</a>
  </div>
</div>
```

#### style.css:
```css
.latest-posts-container {
  padding: 4rem 0;
}

.section-title {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: #333;
}

.section-description {
  font-size: 1.1rem;
  color: #666;
  max-width: 600px;
  margin: 0 auto;
}

.posts-grid {
  display: grid;
  gap: 2rem;
  margin-bottom: 2rem;
}

.posts-grid-2 {
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

.posts-grid-3 {
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}

.posts-grid-4 {
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}

/* Individual post card styling */
.posts-grid .post-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.posts-grid .post-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.posts-grid .post-image {
  height: 200px;
  overflow: hidden;
}

.posts-grid .post-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.posts-grid .post-card:hover .post-image img {
  transform: scale(1.05);
}

.posts-grid .post-content {
  padding: 1.5rem;
}

.posts-grid .post-title {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
}

.posts-grid .post-title a {
  color: #333;
  text-decoration: none;
}

.posts-grid .post-title a:hover {
  color: #007cba;
}

.posts-grid .post-excerpt {
  color: #666;
  margin-bottom: 1rem;
  line-height: 1.6;
}

.posts-grid .post-meta {
  font-size: 0.9rem;
  color: #888;
  margin-bottom: 1rem;
}

.posts-grid .post-link {
  color: #007cba;
  text-decoration: none;
  font-weight: 500;
}

.posts-grid .post-link:hover {
  text-decoration: underline;
}

@media (max-width: 768px) {
  .latest-posts-container {
    padding: 2rem 0;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .posts-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
  
  .posts-grid .post-content {
    padding: 1rem;
  }
}
```

### Featured Posts Carousel Component

For a more dynamic presentation:

#### template.html:
```html
<!-- wpv-attribute name="carousel-title" label="Carousel Title" type="text" -->
<!-- wpv-attribute name="auto-play" label="Auto Play" type="checkbox" -->
<!-- wpv-attribute name="show-navigation" label="Show Navigation" type="checkbox" -->
<!-- wpv-attribute name="view-shortcode" label="Toolset View Shortcode" type="text" -->

<div class="featured-posts-carousel" data-autoplay="{{auto-play}}">
  <div class="carousel-header">
    <h2 class="carousel-title">{{carousel-title}}</h2>
  <!-- Optional navigation -->
  <div class="carousel-nav">
      <button class="nav-prev">&larr;</button>
      <button class="nav-next">&rarr;</button>
    </div>
  </div>
  
  <div class="carousel-container">
    <div class="carousel-track">
      {{view-shortcode}}
    </div>
  </div>
</div>
```

### Database-Based Approach (Legacy)

The original approach using Views directly:

```html
<div class="cc-featured-portfolio-items-container">
  <div class="cc-featured-portfolio-items-inner d-md-flex">
  
    <wpv-loop>
      <!-- per post content goes here -->
    </wpv-loop>
    
  </div>
</div>
```

With content templates for complex layouts:

```html
<div class="cc-featured-portfolio-items-container">
  <div class="cc-featured-portfolio-items-inner d-md-flex">
  
    <wpv-loop>
    
      [wpv-item index=1]
        <div class="item-1-class">
          [wpv-body name="my-content-template"]
        </div>
    
      [wpv-item index=other]
        <div class="other-item-class">
          [wpv-body name="my-content-template"]
        </div>
    
    </wpv-loop>
    
  </div>
</div>
```

### Benefits of Theme-Based Approach

1. **Consistency**: Standardized styling across all latest post sections
2. **Flexibility**: Easy configuration without code changes
3. **Reusability**: Use the same component for different post types
4. **Performance**: Optimized loading and caching
5. **Responsive**: Built-in responsive grid systems

---