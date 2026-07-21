# Forms


* * *

## Summary

All forms on brochure sites should geTo eTo embedSometimes it's useful to be able to relate contact form submissions to other types of data in WordPress. In this case we can [create a relationship](../../General/Toolset.md#custom-post-type-relationships) between the _Website Contacts_ CPT and any other post type in WordPress using [Toolset Relationships](../../General/Toolset.md#custom-post-type-relationships).the form you can use the Toolset Forms options on any WP Tiny MCE form, or, create a Toolset [Content Template](../../General/Toolset.md#content-templates) and embed it with the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method.

## Relationship Fields

Sometimes it's useful to be able to relate contact form submissions to other types of data in WordPress. In this case we can [create a relationship](../../General/Toolset.md#custom-post-type-relationships) between the _Website Contacts_ CPT and any other post type in WordPress using [Toolset Relationships](../../General/Toolset.md#custom-post-type-relationships).the form you can use the Toolset Forms options on any WP Tiny MCE form, or, create a Toolset [Content Template](../../General/Toolset.md#content-templates) and embed it with the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method.

## Relationship Fields

Sometimes it's useful to be able to relate contact form submissions to other types of data in WordPress. In this case we can [create a relationship](../../General/Toolset.md#custom-post-type-relationships) between the _Website Contacts_ CPT and any other post type in WordPress using [Toolset Relationships](../../General/Toolset.md#custom-post-type-relationships).ly use the Toolset Forms plugin. This plugin is a good choice because it provides a lot of basic features - validation, handling submissions, email notifications etc. but with a lot of flexibility:

* You can use any fields defined by Toolset Custom Fields
* You can add special fields, e.g. select with parent posts of the current page
* You can use raw HTML, CSS and JavaScript for any layout and functionality required with no artificial limits that are set by some form plugins

## Contact Forms

Toolset Forms can be used in place of regular contact forms. To do this, complete the following steps:

### Add recaptcha V2 Keys

Use provided recaptcha keys and enter them under _Toolset > Settings > Forms_.

### Create a Custom Post Type

[Create a CPT](../../General/Toolset.md#custom-post-types) called _Website Contacts_ or similar, with the following settings of note:

* Sections to display when editing:
    * Title (can be useful to use as a “name” field in the form)
    * _No others…_

|     |     |
| --- | --- |
| **Options - disable** | **Options - enable** |
| * Rewrite<br>* Allow permalinks…<br>* Feeds<br>* has_archive<br>* hierachal<br>* can_export<br>* show_in_nav_menus<br>* query_var<br>* show_in_rest (requires classic editor at the top of the page) | * show_in_menu<br>* show_ui<br>* publicly_queriable<br>* exclude_from_search |

### Add Custom Fields for the CPT

Add additional custom fields that are needed for the form and assign them to the new CPT, remembering that if you are using the WP Title field as the name field, you do not need an additional field.

**Tip:** you can set validation requirements, e.g. _required_ to fields when adding them and these settings will be carried through to the form.

### Create a New Form

Create a new form under _Toolset > Post Forms > Add New_.

You can use the wizard for the initial setup. Give the form a title and choose the new post type, set the status to private and after submissions display a message and tick _Submit this form without reloading the page (use AJAX)_ (unless you have other requirements)

If the new post type is not available in the dropdown, ensure that you have publicly_queriable enabled in the CPT settings

**Important:** be sure to set the status after submission to private to ensure that any information submitted by users is not accidentally made publicly available.

Add the message to be displayed after a form submission, **NB.** you can use HTML here, any CSS you want for this section will need to be added in the main form editor.

When editing the main form it is best to edit in Expert Mode to give maximum control over the form. You can leave the form with it’s default HTML for now and edit it later.

When you reach the notification emails section **ALWAYS** add an email. If you’re not sure what to add to the email at this point, add an _Admin Notification_ email and set it to [contact@bedevious.co.uk](mailto:contact@bedevious.co.uk) with the email body saying something like: “_The contact form from <site-name> has been submitted but a notification email has not yet been set up”._ This will ensure that if we forget to add a notification we’ll be notified when someone uses the form.

### Build the Form

Building a form from here will follow the same process as creating a [Toolset Content Template](../../General/Toolset.md#content-templates), using the HTML, CSS and JavaScript fields as required.

Useful links:

* [https://toolset.com/documentation/programmer-reference/forms/cred-shortcodes/](https://toolset.com/documentation/programmer-reference/forms/cred-shortcodes/)

### Embed the Form

To embed the form you can use the Toolset Forms options on any WP Tiny MCE form, or, create a Toolset [Content Template](../../General/Toolset.md#content-templates) and embed it with the [BDVS Beaver Themer](../../General/BDVS%20Beaver%20Themer.md) method.

## Relationship Fields

Sometimes it’s useful to be able to relate contact form submissions to other types of data in WordPress. In this case we can [create a relationship](../General/Toolset.md#Custom-Post-Type-Relationships) between the _Website Contacts_ CPT and any other post type in WordPress using [Toolset Relationships](../General/Toolset.md#Custom-Post-Type-Relationships).

Once the relationship is set you can allow users to select other posts from a select menu and even include them in the admin notifications for the form.

Useful links:

* [https://toolset.com/course-lesson/how-to-set-up-post-relationships-in-wordpress/](https://toolset.com/course-lesson/how-to-set-up-post-relationships-in-wordpress/)
* [https://toolset.com/course-lesson/front-end-relationship-forms-for-connecting-posts/](https://toolset.com/course-lesson/front-end-relationship-forms-for-connecting-posts/)

## Post Titles

When setting up a new form, the post title field is always added by default. If the form design calls for a single “name” field, then you can simply use the post title field for this. If the form design calls for separate first and last name fields, then these should be added as separate custom fields. We can then use the following snippets to auto-fill the post title field, which should be hidden, with a combination of the first and last name field values so that the submissions are easily identifiable in the WordPress admin area.

Example:

```html
<div class="d-none">
  <!-- post field should be in here, hidden by the bootstrap d-none class -->
</div>
```

```js
// be sure to check and use the correct keys for your first and last name field names
// i.e. your's may not be 'wpcf-contact-first-name' and 'wpcf-contact-last-name'
jQuery( document ).ready( function() {
  jQuery( '[name="wpcf-contact-first-name"],[name="wpcf-contact-last-name"]' ).on( 'keyup' , function() {

    var firstName = jQuery( '[name="wpcf-contact-first-name"]' ).val();
    var lastName = jQuery( '[name="wpcf-contact-last-name"]' ).val();
    var fullName = firstName + ' ' + lastName;

    jQuery( '[name="post_title"]' ).val( fullName );

  });
});
```

## Theme-Based Form Components

While forms themselves are typically managed through Toolset Forms, you can create theme-based components for form styling and custom form layouts that work alongside Toolset Forms functionality.

### Creating a Styled Form Component

Create a form component that provides custom styling and layout for your Toolset forms:

#### Component Structure:
```
components/
└── view-template/
    └── contact-form/
        ├── template.html
        ├── style.css
        └── script.js
```

#### template.html:
```html
<!-- wpv-attribute name="form-title" label="Form Title" type="text" -->
<!-- wpv-attribute name="form-description" label="Form Description" type="editor" -->
<!-- wpv-attribute name="form-shortcode" label="Toolset Form Shortcode" type="text" -->

<div class="custom-form-container">
  <div class="form-header mb-4">
    <h2 class="form-title">{{form-title}}</h2>
    <div class="form-description">{{form-description}}</div>
  </div>
  
  <div class="form-wrapper">
    {{form-shortcode}}
  </div>
</div>
```

#### style.css:
```css
.custom-form-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.form-title {
  color: #333;
  margin-bottom: 1rem;
}

.form-description {
  color: #666;
  margin-bottom: 2rem;
}

.form-wrapper .form-group {
  margin-bottom: 1.5rem;
}

.form-wrapper label {
  font-weight: 600;
  margin-bottom: 0.5rem;
  display: block;
}

.form-wrapper input[type="text"],
.form-wrapper input[type="email"],
.form-wrapper textarea {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.form-wrapper input[type="text"]:focus,
.form-wrapper input[type="email"]:focus,
.form-wrapper textarea:focus {
  outline: none;
  border-color: #007cba;
  box-shadow: 0 0 0 2px rgba(0, 124, 186, 0.25);
}

.form-wrapper .form-submit input {
  background: #007cba;
  color: white;
  padding: 0.75rem 2rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.form-wrapper .form-submit input:hover {
  background: #005a87;
}
```

#### script.js:
```javascript
document.addEventListener('DOMContentLoaded', function() {
    // Add any custom form interactions here
    const formInputs = document.querySelectorAll('.custom-form-container input, .custom-form-container textarea');
    
    formInputs.forEach(input => {
        input.addEventListener('focus', function() {
            this.parentElement.classList.add('focused');
        });
        
        input.addEventListener('blur', function() {
            this.parentElement.classList.remove('focused');
        });
    });
});
```

### Using the Component

When using this component in your BDVS Content Template module:

1. **Form Title**: Enter the title for your form
2. **Form Description**: Add any explanatory text about the form
3. **Form Shortcode**: Enter the Toolset form shortcode, e.g., `[cred_form form="contact-form"]`

### Advanced Form Component Example

For more complex forms with conditional fields and custom validation:

#### template.html:
```html
<!-- wpv-attribute name="form-title" label="Form Title" type="text" -->
<!-- wpv-attribute name="show-phone" label="Show Phone Field" type="checkbox" -->
<!-- wpv-attribute name="required-message" label="Required Fields Message" type="text" -->
<!-- wpv-attribute name="form-shortcode" label="Toolset Form Shortcode" type="text" -->

<div class="advanced-form-container" data-show-phone="{{show-phone}}">
  <div class="form-header">
    <h2>{{form-title}}</h2>
    <p class="required-notice">{{required-message}}</p>
  </div>
  
  <div class="form-content">
    {{form-shortcode}}
  </div>
  
  <div class="form-footer">
    <small class="text-muted">All information is kept confidential and secure.</small>
  </div>
</div>
```

### Benefits of Theme-Based Form Components

1. **Consistent Styling**: Maintain consistent form appearance across all site forms
2. **Reusable Components**: Create once, use multiple times with different configurations
3. **Version Control**: Track changes to form styling and structure
4. **Performance**: CSS and JavaScript are loaded only when needed
5. **Maintenance**: Easy to update styling across all forms from one location

---