# Single Te* Use _Toolset > Posts Types_ to [create new post types](../../General/Toolset.md#custom-post-types) as needed
* User _Toolset > Custom Fields_ to [create custom fields](../../General/Toolset.md#custom-fields) for the post type. As a general rule all fields for a single post type should be in a single set named "_<CPT> Fields"_ and restricted to that post type as neededlates


* * *

## Summary

Single templates are used to display content for a single post or other CPT. Although they can also be used for WordPress pages, generally pages are more unique and would be build individually, while other content tends to use a template based on a consistent data schema across all the individual posts of that type.

The general setup is as follows:

* Use _Toolset > Posts Types_ to [create new post types](https://bedeviouswebdevelopment.atlassian.net/wiki/spaces/BDWD/pages/edit-v2/685047809#Custom-Post-Types) as needed
* User _Toolset > Custom Fields_ to [create custom fields](https://bedeviouswebdevelopment.atlassian.net/wiki/spaces/BDWD/pages/edit-v2/685047809#Custom-Fields) for the post type. As a general rule all fields for a single post type should be in a single set named “_<CPT> Fields”_ and restricted to that post type as needed
* Use [Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) to create a single template and assign it to the CPT in question
* Use [Beaver Builder](/wiki/spaces/BDWD/pages/610992129/Beaver+Builder) to create the template using the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) method
* Pull custom field data into the template using [Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer)

## Theme-Based Single Templates

Create reusable single post templates using the theme-based component system for consistent post layouts.

### Single Post Template Component

#### Component Structure:
```
components/
└── view-template/
    └── single-post-template/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

#### template.html:
```html
<!-- wpv-attribute name="show-featured-image" label="Show Featured Image" type="checkbox" -->
<!-- wpv-attribute name="show-post-meta" label="Show Post Meta" type="checkbox" -->
<!-- wpv-attribute name="show-author-bio" label="Show Author Bio" type="checkbox" -->
<!-- wpv-attribute name="show-related-posts" label="Show Related Posts" type="checkbox" -->
<!-- wpv-attribute name="content-width" label="Content Width" type="select" options="normal,wide,full" -->

<article class="single-post-template content-width-{{content-width}}">
  
  <!-- Optional featured image -->
  <div class="post-featured-image">
    [wpv-post-featured-image size="large"]
  </div>
  
  <div class="post-header">
    <h1 class="post-title">[wpv-post-title]</h1>
    
  <!-- Optional post meta -->
  <div class="post-meta">
      <span class="post-date">[wpv-post-date]</span>
      <span class="post-author">by [wpv-post-author]</span>
      <span class="post-categories">[wpv-post-taxonomy type="category"]</span>
    </div>
  </div>
  
  <div class="post-content">
    [wpv-post-body]
  </div>
  
  <div class="post-tags">
    [wpv-post-taxonomy type="post_tag"]
  </div>
  
  <!-- Optional author bio -->
  <div class="author-bio">
    <div class="author-avatar">
      [wpv-post-author display="avatar"]
    </div>
    <div class="author-info">
      <h3>About [wpv-post-author]</h3>
      <p>[wpv-post-author display="description"]</p>
    </div>
  </div>
  
  <!-- Optional related posts -->
  <div class="related-posts">
    <h3>Related Posts</h3>
    [wpv-view name="related-posts"]
  </div>
  
</article>
```

#### style.css:
```css
.single-post-template {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem 1rem;
}

.content-width-wide {
  max-width: 1000px;
}

.content-width-full {
  max-width: 100%;
}

.post-featured-image {
  margin-bottom: 2rem;
  border-radius: 8px;
  overflow: hidden;
}

.post-featured-image img {
  width: 100%;
  height: auto;
  display: block;
}

.post-header {
  margin-bottom: 2rem;
  text-align: center;
}

.post-title {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #333;
  line-height: 1.2;
}

.post-meta {
  display: flex;
  justify-content: center;
  gap: 1rem;
  color: #666;
  font-size: 0.9rem;
}

.post-meta span {
  position: relative;
}

.post-meta span:not(:last-child)::after {
  content: '•';
  margin-left: 1rem;
  color: #ccc;
}

.post-content {
  font-size: 1.1rem;
  line-height: 1.8;
  margin-bottom: 2rem;
}

.post-content h2,
.post-content h3,
.post-content h4 {
  margin-top: 2rem;
  margin-bottom: 1rem;
  color: #333;
}

.post-content p {
  margin-bottom: 1.5rem;
}

.post-content img {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  margin: 1rem 0;
}

.post-tags {
  margin-bottom: 3rem;
  padding-top: 2rem;
  border-top: 1px solid #eee;
}

.post-tags a {
  display: inline-block;
  background: #f8f9fa;
  color: #666;
  padding: 0.25rem 0.75rem;
  margin: 0.25rem 0.25rem 0.25rem 0;
  text-decoration: none;
  border-radius: 15px;
  font-size: 0.9rem;
  transition: background 0.3s ease;
}

.post-tags a:hover {
  background: #007cba;
  color: white;
}

.author-bio {
  display: flex;
  gap: 1rem;
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 3rem;
}

.author-avatar img {
  width: 80px;
  height: 80px;
  border-radius: 50%;
}

.author-info h3 {
  margin-bottom: 0.5rem;
  color: #333;
}

.author-info p {
  color: #666;
  margin: 0;
  line-height: 1.6;
}

.related-posts {
  padding-top: 3rem;
  border-top: 2px solid #eee;
}

.related-posts h3 {
  margin-bottom: 2rem;
  text-align: center;
  color: #333;
}

@media (max-width: 768px) {
  .single-post-template {
    padding: 1rem;
  }
  
  .post-title {
    font-size: 2rem;
  }
  
  .post-meta {
    flex-direction: column;
    gap: 0.5rem;
    align-items: center;
  }
  
  .post-meta span::after {
    display: none;
  }
  
  .author-bio {
    flex-direction: column;
    text-align: center;
  }
}
```

### Custom Post Type Template Example

For custom post types like portfolios, testimonials, or case studies:

#### template.html:
```html
<!-- wpv-attribute name="cpt-layout" label="Layout Style" type="select" options="standard,showcase,minimal" -->
<!-- wpv-attribute name="show-custom-fields" label="Show Custom Fields" type="checkbox" -->

<article class="single-cpt-template cpt-layout-{{cpt-layout}}">
  
  <div class="cpt-header">
    <h1 class="cpt-title">[wpv-post-title]</h1>
    <div class="cpt-excerpt">[wpv-post-excerpt]</div>
  </div>
  
  <!-- Optional custom fields -->
  <div class="cpt-custom-fields">
    <!-- Example custom fields for a portfolio item -->
    <div class="field-group">
      <label>Client:</label>
      <span>[types field="client"][/types]</span>
    </div>
    <div class="field-group">
      <label>Project Date:</label>
      <span>[types field="project-date"][/types]</span>
    </div>
    <div class="field-group">
      <label>Skills Used:</label>
      <span>[types field="skills"][/types]</span>
    </div>
  </div>
  
  <div class="cpt-content">
    [wpv-post-body]
  </div>
  
  <div class="cpt-gallery">
    [types field="project-gallery"][/types]
  </div>
  
</article>
```

### Benefits of Theme-Based Single Templates

1. **Consistency**: Standardized layouts across all single posts of the same type
2. **Customization**: Easy to modify layout options without code changes
3. **Reusability**: Use the same template structure for different custom post types
4. **Maintainability**: Update all single post layouts from one location
5. **Performance**: Optimized loading and better caching

---