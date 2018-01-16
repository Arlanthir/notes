# Wordpress

A Wordpress website is composed of Themes and Plugins.


## Themes

Themes are what defines the structure and style of the site.
They require at least 2 files: **index.php** (the default content) and **style.css** (the default style and theme metadata).
Optionally, it can use **functions.php** to register hooks callbacks, like enqueueing scripts and styles.

![Template hierarchy](https://developer.wordpress.org/files/2014/10/wp-hierarchy.png)

Scripts can be localized using the function **wp_localize_script**, to translate text or pass in arguments from PHP.


## Plugins

Plugins add features to a website.
They can be used regardless of the applied theme but may offer additional content that it can use.
For instance, they can define **shortcodes** that can be included in your content by writing *[thecodename]* in
the intended textarea.

Plugins generally reside in a folder with their name in `wp-content/plugins` and must have at least a php file.
Plugins in the Wordpress Plugins repository must also include a [readme.txt](https://wordpress.org/plugins/developers/).


## Development Flow

Development is made through the use of **hooks**, basically events that can be listened to on your themes and plugins.
They are divided in **actions** and **filters**, whether they are the result of an action or used to filter data.

Plugins can define entities, represented by **custom post types** and **taxonomies** of **categories** for posts.


## Queries

```php
<?php
    $args = array(
        'post_type' => 'tutor'
    );
    $q = new WP_Query($args);

    if ($q->have_posts()) {
        while ($q->have_posts()) {
            $q->the_post();
?>
            <div class=""><?php echo the_title(); ?></div>
<?php
            }
    }

    /* Restore original Post Data */
    wp_reset_postdata();
?>
```


## Navigation Menus

To register the menu in **functions.php**, action `after_setup_theme`:
```php
register_nav_menus(array(
     'primary' => __('Primary Menu', 'twentyfifteen'),
     'social' => __('Social Links Menu', 'twentyfifteen')
));
```

To display the menu:
```php
wp_nav_menu(array(
    'menu_class' => 'nav-menu',
    'theme_location' => 'primary'
));
```
