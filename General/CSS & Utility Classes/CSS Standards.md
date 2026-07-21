# CSS Standards

* * *

## Summary

Generally speaking, please follow the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/) for CSS. This page covers some specifics when working on projects with Be Devious Web Development.

The general theme / order of preference

* Use utility classes as far as possible:
    * When using Bootstrap these will remain inline in the HTML.
    * When using Tailwind these should be wrapped using the `@apply` syntax where ever possible, see the [Tailwind example](#tailwind-example) below.
* When writing CSS, create new utility, Bootstrap style classes where ever possible, especially when the style is likely to need to be kept consistent across a project, e.g. spacing, font sizes or colours.

## General

* Use classes over IDs unless an element really needs to be unique

```css
.more-of-these {
  color: red;
}

#less-of-these {
  color: red;
}
```

* Classes, IDs, data-attributes etc. should be written in lower case with hyphens separating words

```java
.this-is-good {
  color: red;
}

.thisIsNotGood {
  color: red;
}
```

* Ease of reading is important; a longer easier to read class is better than a short obscure one

```java
.this-is-easy-to-read-even-though-its-long {
  color: red;
}

.tietrwtil { /**not easy to read**/
  color: red;
}
```

* CSS attributes should be listed in alphabetical order unless otherwise required for functionality

```css
/**Good**/
.alphabetical {
  border: none;
  color: black;
  display: flex;
  padding: 10px;
}

/**Not so good**/
.any-order {
  display: flex;
  padding: 10px;
  border: none;
  color: black;
}
```

* MOBILE FIRST CSS AND UTILITIES! CSS and utility classes should be added for the smallest viewpoint first with responsive classes and media queries to set changes for larger viewpoints.

```java
/**Like this**/
.my-class {
  display: flex;
  flex-direction: column;
}

@media( min-width: 768px ) {

  .my-class {
    flex-direction: row;
  }

}

/**Not like this**/
.my-class {
  display: flex;
}

@media( max-width: 768px ) {

  .my-class {
    flex-direction: column;
  }

}
<!-- Like this -->
<div class="d-flex flex-column flex-md-row"></div>

<!-- Not like this -->
<div class="d-flex flex-sm-column flex-md-row"></div>
```

* Use the standard [Boostrap breakpoints](https://getbootstrap.com/docs/4.6/layout/overview/#responsive-breakpoints):

```java
@media( min-width: 576px ) { }
@media( min-width: 768px ) { }
@media( min-width: 1024px ) { }
@media( min-width: 1200px ) { }
```

* Use of SCSS Variables:

```css
/** Declare the root variable **/
:root {
  --color-primary: #2DB875;
}

/** create a utility type class **/
.color-dark-green {
  color: var(--color-primary-green);
}
```

## Utility Class Examples

### Prefixing

- All project-specific CSS classes must be prefixed with `bbt-` to avoid collisions with Bootstrap/Beaver/WordPress and to improve searchability (e.g. `bbt-feature-card`, `bbt-recent-posts`).

### Example 1:

Instead of:

```html
<div class="my-class">
  My Content
</div>
.my-class {
  align-items: center;
  display: flex;
  justify-content: center;
  padding: 1.2em;
}
```

Do this:

```java
<div class="my-class d-flex align-items-center justify-content-center p-1">
  My Content
</div>
```

Although arguably this is harder to read and may require more code updates when re-formatting, it ensures consistent formatting of similar classes.

### Example 2:

Instead of:

```html
<div class="my-class">
  My Content
</div>
.my-class {
  background-image: url(../path-to-my-image.jpg);
  background-position: center;
  background-size: cover;
}
```

Do this:

```html
<div class="my-class cover-background">
  My Content
</div>
.my-class {
  background-image: url(../path-to-my-image.jpg);
}
```

## Tailwind Example

```css
.my-tailwind-class {
  @apply w-full h-full flex;
  /** some other properties and their values... **/
}
```