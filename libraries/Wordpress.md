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

### Actions
```php
function register_my_menu() {
    // ...
}
add_action('after_setup_theme', 'register_my_menu', /* priority */ 10, /* number of arguments */ 1);
```

### Filters
```php
function my_filter($arg) {
    // ...
    return $arg;
}
add_filter('some_filter', 'my_filter', /* priority */ 10, /* number of arguments */ 1);
```

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

Or:

```php
function edition_number_porfolio_where($where) {
	global $wpdb;
	global $keyword;
	$where .= ' AND ' . $wpdb->posts . '.post_title LIKE \'%' . esc_sql(like_escape($keyword)) . '%\'';
	return $where;
}

add_filter('posts_where', 'edition_number_porfolio_where');
$keyword = 'my search';
$posts = get_posts(array(
	'posts_per_page' => -1,
	'suppress_filters' => false, // important!
	'post_type' => 'portfolio',
	'post_status' => 'publish'
));
remove_filter('posts_where', 'edition_number_porfolio_where');

print_r(array_map(function ($el) {
	return $el->post_title;
}, $posts));
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

## Widget Areas

To register widgets, in **functions.php**, action `widgets_init`:
```php
register_sidebar(array(
     'name' => __( 'Widget Area', 'twentyfifteen' ),
     'id' => 'sidebar-1',
     'description' => __( 'Add widgets here to appear in your sidebar.', 'twentyfifteen' ),
     'before_widget' => '<aside id="%1$s" class="widget %2$s">',
     'after_widget' => '</aside>',
     'before_title' => '<h2 class="widget-title">',
     'after_title' => '</h2>',
));
```

To display the widgets:
```php
<?php if (is_active_sidebar('sidebar-1')) { ?>
     <div id="widget-area-1" class="widget-area">
          <?php dynamic_sidebar('sidebar-1'); ?>
     </div>
<?php } ?>
```

## Custom Post Types (TODO: expand)

https://www.wpbeginner.com/wp-tutorials/how-to-create-custom-post-types-in-wordpress/

## Enqueue scripts and styles

```php
function themename_scripts() {
    // Note: Use get_stylesheet_directory_uri() for the path to the child theme instead
    wp_enqueue_style('themename-css', get_template_directory_uri() . '/css/style.css', array(), '20180101');
    wp_enqueue_script('themename-script', get_template_directory_uri() . '/js/script.js', array('jquery'), '20180101', true);
    // Pass arguments to script if needed (variable myVars.ajaxurl will be available)
    wp_localize_script('themename-script', 'myVars', array('ajaxurl' => admin_url('admin-ajax.php')));
}
add_action('wp_enqueue_scripts', 'themename_scripts');
```

## AJAX Requests

```javascript
var req = new XMLHttpRequest();
var url = '<?= admin_url('admin-ajax.php'); ?>';
var params = 'action=my_request&nonce=<?= wp_create_nonce('my_request_nonce'); ?>';
req.open('POST', url);
req.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
		
req.onreadystatechange = function(e) {
    if (req.readyState == 4) {
        if (req.status == 200) {
            console.log('Request success:', req.responseText);
        } else {
            console.log('Request error status:', req.status);
        }
    }
};

req.send(params);
```

```php
function ajax_my_request() {
    if (!wp_verify_nonce($_REQUEST['nonce'], 'my_request_nonce')) {
        // wp_send_json_error('Invalid nonce', 401);   // Will output {success: false, data: 'Invalid nonce'}
        // To customize further:
        status_header(401);
        echo json_encode(array('status' => 'error', 'error' => 'Invalid nonce'));
        exit;
    }
    echo json_encode(array('status' => 'success'));
    exit;
}
add_action('wp_ajax_my_request', 'ajax_my_request');
add_action('wp_ajax_nopriv_my_request', 'ajax_my_request'); // When logged out users call the service
```

## Translations

Install Poedit to manage translations. Open the plugin's `.po` file, set the language to the desired one and save it somewhere else with the language suffix (e.g. `plugin-name-pt_PT.po`). Edit the translations in the file and save it. The corresponding `.mo` should be created alongside the `.po`.

Alternative to create .po file:
```bash
cd wp-content/themes/mytheme/languages/
find ../ -name '*.php' -exec xgettext --from-code=UTF-8 --keyword=__ --keyword=_e -o en_US.po '{}' ';'
```

To override plugin translations, place your files in `wp-content/languages/plugins`.

Register a language for a child theme:
```php
function my_language_setup() {
	load_child_theme_textdomain('theme-name', get_stylesheet_directory() . '/languages');
}
add_action('after_setup_theme', 'my_language_setup');
```

## Child themes

Instead of modifying a downloaded theme, it is advisable to create a child theme of that one. In it, you can
override functions by redeclaring them in your files (typically mirroring the structure of the original theme).

## Find out which template file is rendering each page

Add to `functions.php`:
```php
/* Show template file that drew each page */
add_filter('template_include', 'var_template_include', 1000);
function var_template_include($t) {
	$GLOBALS['current_theme_template'] = basename($t);
	return $t;
}

function get_current_template() {
	if (!isset($GLOBALS['current_theme_template'])) {
		return false;
	}
	return $GLOBALS['current_theme_template'];
}

function show_template() {
    if( is_super_admin() ){
		echo("<strong>Template file: </strong>");
		echo(get_current_template());
		global $template;
		print_r($template);
	}
}
add_action('wp_footer', 'show_template');
```
