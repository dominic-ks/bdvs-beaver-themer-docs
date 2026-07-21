# Left / Right Content Row


* * *

ContentBody{} FormattedBody{value='

## Summary

A typical row layout, where, on desktop, two columns will be presented; one contains an image whilst the other contains some other content.

Basic forms of this layout can, of course, be easily made with Beaver Builder, but some small design requirements can quickly make this tricky, and using a flexible content template can solve these issues.

## Example

The following example is from [https://saraholiverproperty.com/](https://saraholiverproperty.com/) and is extremely flexible, allowing for multiple layouts.

### HTML

```html
<div class="sop-lr-content-row">
  <div class="sop-lr-content-inner d-flex flex-column flex-md-[wpv-attribute name='lr-direction' label='Row Direction' type='select' options='row,row-reverse'] sop-direction-[wpv-attribute name='lr-direction' label='Row Direction' type='select' options='row,row-reverse'] sop-layout-type-[wpv-attribute name='lr-layout-type' label='Layout Type' type='select' options='compact,full'] sop-has-video-[wpv-attribute name='lr-has-video' label='Has Video?' type='select' options='no,yes' toggle='yes:lr-video-embed']">

    <div class="sop-lr-content-row-col sop-lr-content-row-image-col cover-background position-relative" style="background-image: url([wpv-attribute name='lr-image' label='Image' type='photo']); background-position-x: [wpv-attribute name='lr-image-x' label='Image Horizontal Align' type='select' options='center,left,right']; background-position-y: [wpv-attribute name='lr-image-y' label='Image Vertical Align' type='select' options='center,top,bottom'];">
      <div class="sop-lr-content-row-video-overlay position-absolute">
        <img src="https://saraholiverproperty.com/wp-content/uploads/2023/08/Play_Button.svg" class="sop-lr-content-video-icon" />

      </div>
    </div>

    <div class="sop-lr-content-row-col sop-lr-content-row-content-col position-relative">
      <div class="sop-lr-content-row-content-col-inner d-flex justify-content-center align-items-center h-100 p-4 py-lg-5 sop-bg-[wpv-attribute name='lr-content-bg' label='Content Background' type='select' options='grey-orange,peach,ceramic,vanilla,slate-grey'] sop-text-[wpv-attribute name='lr-text-color' label='Text Colour' type='select' options='light,dark,ceramic,vanilla']">
        <div class="so-lr-content-row-content-wrapper px-xl-4">
          <h2 class="sop-lr-content-title sop-text-[wpv-attribute name='lr-text-color' label='Text Colour' type='select' options='light,dark,ceramic,vanilla']">[wpv-attribute name='lr-title' label='Title' type='text']</h2>
          <div class="sop-lr-content-container">[wpv-attribute name='lr-content' label='Content' type='editor']</div>

          <div class="single-btn-container sop-btn-[wpv-attribute name='lr-show-btn' label='Show Button?' type='select' options='hide,show' toggle='show:destination,lr-btn-bg,lr-btn-text-color,lr-btn-hover-color,lr-btn-target,lr-button-text']">
            <a href="[wpv-attribute name='destination' label='Link Destination' type='link']" class="btn btn-lg btn-bg-[wpv-attribute name='lr-btn-bg' label='Button Background' type='select' options='grey-orange,peach,ceramic,vanilla,slate-grey'] btn-text-color-[wpv-attribute name='lr-btn-text-color' label='Button Text Colour' type='select' options='light,dark,ceramic,vanilla'] btn-hover-[wpv-attribute name='lr-btn-hover-color' label='Button Hover Colour' type='select' options='transparent,grey-orange,peach,ceramic,vanilla']" target="[wpv-attribute name='lr-btn-target' label='Target' type='select' options='_self,_blank']" rel="noopener">[wpv-attribute name='lr-button-text' label='Button Text' type='text']</a>
          </div>

        </div>
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-body">
            <button type="button" class="btn btn-modal-close btn-bg-grey-orange btn-hover-peach btn-text-color-light" data-dismiss="modal" aria-label="Close">
              <i class="fas fa-times"></i>
            </button>
            [wpv-attribute name='lr-video-embed' label='Video Embed' type='textarea']
          </div>
        </div>
      </div>
    </div>

  </div>
</div>
```

### CSS

```css
/** Left / Right Content Rows **/
.sop-lr-content-row-image-col {
	padding-top: 75%;
}

.sop-text-light {
	color: white;
}

.sop-text-ceramic {
	color: #F7F0EA;
}

.sop-text-vanilla {
	color: #D2B89A;
}

.sop-btn-hide {
	display: none;
}

.sop-lr-content-row-video-overlay {
	display: none;
	transition: all 0.5s;
}

.sop-has-video-yes .sop-lr-content-row-video-overlay {
	align-items: center;
	bottom: 0;
	cursor: pointer;
	display: flex;
	justify-content: center;
	left: 0;
	right: 0;
	top: 0;
}

.sop-has-video-yes .sop-lr-content-row-video-overlay:hover {
	background-color: #00000080;
}

.sop-lr-content-video-icon {
	width: 100px;
}

.btn-modal-close {
	align-items: center!important;
	border-radius: 100%!important;
	display: flex!important;
	font-size: 13px;
	height: 20px!important;
	justify-content: center!important;
	padding: 0!important;
	position: absolute!important;
	right: 10px!important;
	top: 10px!important;
	width: 20px!important;
	z-index: 9;
}

@media( min-width: 768px ) {

	.sop-lr-content-row-col {
		width: 50%;
	}

	.sop-layout-type-full .sop-lr-content-row-image-col {
		width: 66.666666%;
	}

	.sop-layout-type-full .sop-lr-content-row-content-col {
		width: 33.333333%;
	}

	.sop-lr-content-row-image-col {
		padding-top: 33%;
	}

	.sop-layout-type-full .sop-lr-content-row-image-col {
		padding-top: 50%;
	}

}

@media( min-width: 992px ) {

	.sop-direction-row .sop-lr-content-row-content-col-inner {
		margin-left: -50px;
	}

	.sop-direction-row-reverse .sop-lr-content-row-content-col-inner {
		margin-right: -50px;
	}

	.sop-layout-type-full.sop-direction-row .sop-lr-content-row-content-col-inner {
		margin-left: 0;
	}

	.sop-layout-type-full.sop-direction-row-reverse .sop-lr-content-row-content-col-inner {
		margin-right: 0;
	}

	.sop-lr-content-row-content-col {
		padding-bottom: 3rem;
		padding-top: 3rem;
	}

	.sop-layout-type-full .sop-lr-content-row-content-col {
		padding-bottom: 0;
		padding-top: 0;
	}

}
```

### JavaScript

This script operates the video modal if in use.

```js
jQuery( document ).ready( function() {

  jQuery( '.sop-lr-content-row-video-overlay' ).click( function() {
    
  	const rowParents = jQuery( this ).parents( '.sop-lr-content-row' );
    const rowParent = rowParents[0];
    const modalChildren = jQuery( rowParent ).find( '.modal' );
    const modal = modalChildren[0];
    
    jQuery( modal ).modal();
    
  });

});
```

## Theme-Based Left/Right Content Row

Create a flexible, reusable left/right content component with theme-based approach.

### Component Structure:
```
components/
└── view-template/
    └── left-right-content-row/
        ├── template.html
        ├── style.css
        └── script.js (optional)
```

### template.html:
```html
<!-- wpv-attribute name="lr-direction" label="Row Direction" type="select" options="row,row-reverse" -->
<!-- wpv-attribute name="lr-layout-type" label="Layout Type" type="select" options="compact,full" -->
<!-- wpv-attribute name="lr-has-video" label="Has Video?" type="select" options="no,yes" -->
<!-- wpv-attribute name="lr-image" label="Image" type="photo" -->
<!-- wpv-attribute name="lr-image-x" label="Image Horizontal Align" type="select" options="center,left,right" -->
<!-- wpv-attribute name="lr-image-y" label="Image Vertical Align" type="select" options="center,top,bottom" -->
<!-- wpv-attribute name="lr-video-embed" label="Video Embed Code" type="editor" -->
<!-- wpv-attribute name="lr-content-title" label="Content Title" type="text" -->
<!-- wpv-attribute name="lr-content-subtitle" label="Content Subtitle" type="text" -->
<!-- wpv-attribute name="lr-content-body" label="Content Body" type="editor" -->
<!-- wpv-attribute name="lr-content-link-text" label="Link Text" type="text" -->
<!-- wpv-attribute name="lr-content-link-url" label="Link URL" type="url" -->

<div class="lr-content-row lr-direction-{{lr-direction}} lr-layout-{{lr-layout-type}} lr-has-video-{{lr-has-video}}">
  <div class="lr-content-inner d-flex flex-column flex-md-{{lr-direction}}">

    <div class="lr-content-col lr-image-col cover-background position-relative" 
         style="background-image: url('{{lr-image}}'); background-position: {{lr-image-x}} {{lr-image-y}};">
      
  <!-- Optional video overlay -->
  <div class="lr-video-overlay position-absolute d-flex align-items-center justify-content-center">
        <button class="video-play-btn" data-video="{{lr-video-embed}}">
          <svg class="play-icon" width="60" height="60" viewBox="0 0 60 60">
            <circle cx="30" cy="30" r="30" fill="rgba(255,255,255,0.9)"/>
            <polygon points="24,18 24,42 42,30" fill="#333"/>
          </svg>
        </button>
      </div>
    </div>

    <div class="lr-content-col lr-text-col d-flex align-items-center">
      <div class="lr-text-content">
  <!-- Optional subtitle -->
  <p class="lr-subtitle">{{lr-content-subtitle}}</p>
        
        <h2 class="lr-title">{{lr-content-title}}</h2>
        
        <div class="lr-body">{{lr-content-body}}</div>
        
  <!-- Optional CTA -->
  <a href="{{lr-content-link-url}}" class="lr-cta-btn">{{lr-content-link-text}}</a>
      </div>
    </div>

  </div>
</div>
```

### style.css:
```css
.lr-content-row {
  margin: 4rem 0;
}

.lr-content-inner {
  min-height: 500px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.lr-content-col {
  flex: 1;
  min-height: 300px;
}

.lr-image-col {
  background-size: cover;
  background-repeat: no-repeat;
  position: relative;
}

.lr-text-col {
  padding: 3rem;
  background: white;
}

.lr-text-content {
  max-width: 500px;
}

.lr-subtitle {
  color: #666;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 0.5rem;
}

.lr-title {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
  color: #333;
  line-height: 1.2;
}

.lr-body {
  font-size: 1.1rem;
  line-height: 1.6;
  color: #666;
  margin-bottom: 2rem;
}

.lr-cta-btn {
  display: inline-block;
  background: #007cba;
  color: white;
  padding: 1rem 2rem;
  text-decoration: none;
  border-radius: 4px;
  font-weight: 500;
  transition: background 0.3s ease;
}

.lr-cta-btn:hover {
  background: #005a87;
  color: white;
  text-decoration: none;
}

/* Video overlay styling */
.lr-video-overlay {
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.video-play-btn {
  background: none;
  border: none;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.video-play-btn:hover {
  transform: scale(1.1);
}

/* Layout variations */
.lr-layout-compact .lr-content-inner {
  max-width: 1000px;
  margin: 0 auto;
}

.lr-layout-full .lr-content-inner {
  width: 100%;
}

/* Direction-specific styling */
.lr-direction-row .lr-text-col {
  border-left: 4px solid #007cba;
}

.lr-direction-row-reverse .lr-text-col {
  border-right: 4px solid #007cba;
}

/* Responsive design */
@media (max-width: 768px) {
  .lr-content-inner {
    flex-direction: column !important;
    min-height: auto;
  }
  
  .lr-content-col {
    min-height: 250px;
  }
  
  .lr-text-col {
    padding: 2rem;
  }
  
  .lr-title {
    font-size: 2rem;
  }
  
  .lr-direction-row .lr-text-col,
  .lr-direction-row-reverse .lr-text-col {
    border: none;
    border-top: 4px solid #007cba;
  }
}
```

### script.js:
```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Handle video overlay clicks
    const videoButtons = document.querySelectorAll('.video-play-btn');
    
    videoButtons.forEach(button => {
        button.addEventListener('click', function() {
            const videoEmbed = this.getAttribute('data-video');
            if (videoEmbed) {
                // Create modal or embed video inline
                showVideoModal(videoEmbed);
            }
        });
    });
    
    function showVideoModal(embedCode) {
        // Create a simple modal for video display
        const modal = document.createElement('div');
        modal.className = 'video-modal';
        modal.innerHTML = `
            <div class="video-modal-content">
                <button class="video-modal-close">&times;</button>
                <div class="video-embed">${embedCode}</div>
            </div>
        `;
        
        document.body.appendChild(modal);
        
        // Close modal functionality
        modal.querySelector('.video-modal-close').addEventListener('click', function() {
            document.body.removeChild(modal);
        });
        
        modal.addEventListener('click', function(e) {
            if (e.target === modal) {
                document.body.removeChild(modal);
            }
        });
    }
});
```

### Database-Based Approach (Legacy)

The original approach using direct attributes and complex HTML: