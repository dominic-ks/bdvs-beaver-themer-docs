# Beaver Builder Custom Fonts

* * *

## Summary

Where custom fonts are required, we use the best practice to add the fonts and register them with Beaver Builder so that they are available in all font selection interfaces.

### Method 1: Host the Font Files

The preferred location for font files is:

* <child-theme-dir>/inc/fonts/<font-name>

Once uploaded, code is required in both functions.php and styles.css:

```php
/**
*
* Add ccustom BB fonts
*
* @param $system_fonts arr an array of registered fonts
*
**/

function my_bb_custom_fonts( $system_fonts ) {

  $system_fonts['Agenda'] = array(
    'fallback' => 'Verdana, Arial, sans-serif',
    'weights' => array(
      '400',
      '700',
    ),
  );

  return $system_fonts;
  
}

//Add to Beaver Builder Theme Customizer
add_filter( 'fl_theme_system_fonts', 'my_bb_custom_fonts' );

//Add to Page Builder modules
add_filter( 'fl_builder_font_families_system', 'my_bb_custom_fonts' );
```

```css
@font-face {
  font-family: 'Agenda';
  src: url('fonts/agenda/agenda-medium.ttf') format('woff');
  font-weight: 400;
  font-style: medium;
}

@font-face {
  font-family: 'Agenda';
  src: url('fonts/agenda/agenda-bold.ttf') format('woff');
  font-weight: 700;
  font-style: bold;
}
```

### Method 2: Remote Load the Fonts

The second method requires only code in the functions.php:

```java
/**
*
* Enqueue styles and scripts
*
**/

function my_enqueue_styles_and_scripts() {
  wp_enqueue_style( 'my-font' , <font-css-url> , array() , '0.0.1' );
}

add_action( 'wp_enqueue_scripts', 'my_enqueue_styles_and_scripts' );

/**
*
* Add ccustom BB fonts
*
* @param $system_fonts arr an array of registered fonts
*
**/

function my_bb_custom_fonts( $system_fonts ) {

  $system_fonts['Agenda'] = array(
    'fallback' => 'Verdana, Arial, sans-serif',
    'weights' => array(
      '400',
      '700',
    ),
  );

  return $system_fonts;
  
}

//Add to Beaver Builder Theme Customizer
add_filter( 'fl_theme_system_fonts', 'my_bb_custom_fonts' );

//Add to Page Builder modules
add_filter( 'fl_builder_font_families_system', 'my_bb_custom_fonts' );
```