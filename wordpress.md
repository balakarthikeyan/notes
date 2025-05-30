## Wordpress Features
- Custom Header, Logo & Background
- Menus
- Widgets
- Theme Options & Framework
- Child Theme
- Functions
- Shortcode
- Custom Taxonomies
- Custom Post Types
- Threaded Comments
- Featured Image
- Sticky / Featured Post
- Breadcrumbs
- Microformats
- Canonical
- Web Fonts
- Sliders

## Wordpress snippets for templates
- `the_content(); `Content of the posts
- `have_posts(); `Check if there are posts
- `the_post(); `Shows posts if posts are available
- `get_header(); `Header.php file's content
- `get_sidebar(); `Sidebar.php file's content
- `get_footer(); `Footer.php file's content
- `the_time('m-d-y'); `The date in 'month-day-year′ format
- `comments_popup_link(); `Link for the comments on the post
- `the_title(); `Title of a specific post or page
- `the_permalink(); `URL of a specific post or page
- `the_category(); `Category of a specific post or page
- `the_author(); `Author of a specific post or page
- `the_ID(); `ID of a specific post or page
- `edit_post_link(); `Link to edit a specific post or page
- `get_links_list(); `Links from the blogroll
- `comments_template(); `Comment PHP file's content
- `wp_list_pages(); `List of pages of the site
- `wp_list_cats(); `List of categories for the site
- `next_post_link('%link'); `URL to the next post
- `previoust_post_link('%link'); `URL to the previous post
- `get_calendar(); `The built-in calendar
- `wp_get_archives(); `List of archives for the site
- `posts_nav_link(); `Next and previous post links
- `bloginfo('description'); `Site's description
- `wp_meta(); `Meta for administrators
- `timer_stop(1); `Time to load the page
- `echo get_num_queries(); `Queries to load the page
- `include(TEMPLATEPATH . '/file_name'); `Include any file
- `the_search_query(); `Value for search form
- `_e('Message'); `Prints out message
- `wp_register(); `Displays the register link
- `wp_loginout(); `Displays the login/logout link
- `<!--next page--> `Divides the content into pages Cuts off the content and adds a read more link

## Functions:

```php
wp_list_pages('sort_column=menu_order&depth;=1&title;_li='); // Displays the menus 
wp_list_categories('title_li=&orderby;=id'); // Displays the categories
```

## Wordpress User roles:

- `Super Admin:` The profile that has access to the entire website, including network administrative features.
- `Administrator:` The profile(s) that has all administrative privileges.
- `Editor:` The profile(s) that can create, edit, publish theirs, and other users' posts.
- `Author:` The profile(s) that can create, edit, publish their posts only.
- `Contributor:` The profile(s) that can create, edit their posts but cannot publish them.
- `Subscriber:` The profile(s) that can only manage their profiles.

## Customer Header & Logo
```php
function mytheme_custom_header_setup() {
    add_theme_support( 'custom-header', apply_filters( 'mytheme_custom_header_args', array(
        'default-image' => '',
        'header-text' => true,
        'default-text-color' => '000',
        'width' => 200,
        'height' => 90,
        'flex-width' => true,
        'flex-height' => true,
        'wp-head-callback' => false,
    ) ) );
    add_theme_support( 'custom-logo', array(
        'height'      => 100,
        'width'       => 400,
        'flex-height' => true,
        'flex-width'  => true,
        'header-text' => array( 'site-title', 'site-description' ),
    ) );    
}
add_action( 'after_setup_theme', 'mytheme_custom_header_setup' );
```

## Register Custom Navigation Walker
```php
function register_navwalker(){
    if ( ! file_exists( get_template_directory() . '/lib/bootstrap-navwalker.php' ) ) {
        return new WP_Error( 'bootstrap-navwalker-missing', __( 'It appears the bootstrap-navwalker.php file may be missing.', 'bootstrap-navwalker' ) );
    } else {
        require_once get_template_directory() . '/lib/bootstrap-navwalker.php';
    }    
}
add_action( 'after_setup_theme', 'register_navwalker' );
```

## For `<title>` load from Wordpress
```php
add_theme_support( 'title-tag' );
```
	
## Enqueue scripts and styles.
```php
function mytheme_scripts() {
    $query_args = array( 'family' => 'Open+Sans:400,600,700,800' );
    wp_enqueue_style( 'google-fonts', add_query_arg( $query_args, "//fonts.googleapis.com/css" ) );
    // wp_register_style('OpenSans', 'http://fonts.googleapis.com/css?family=Open+Sans:400,600,700,800');
    // wp_enqueue_style( 'OpenSans');
    wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/css/bootstrap.min.css', array(), '3.3.7' );
    wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/css/bootstrap-theme.min.css', array(), '3.3.7' );
    wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/js/bootstrap.min.js', array("jquery"), '3.3.7', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery.min.js', array(), '2.2.4', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery-migrate.js', array(), '1.4.1', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery/jquery-noconflict.js', array(), '', true );
    // Custom styles & script for this template
    wp_enqueue_script( 'theme-js', get_template_directory_uri() . '/scripts/theme.js', array(), '', true );
    wp_enqueue_style( 'theme-style', get_template_directory_uri() . '/style.css' );
    wp_enqueue_style( 'theme-css', get_template_directory_uri() . '/css/theme.css' );
}
add_action( 'wp_enqueue_scripts', 'mytheme_scripts' );
```

## Custom Settings for Social Media
```php
function custom_settings_add_menu() {
    add_menu_page( 
        'Custom Settings',	// Title of the page 
        'Custom Settings',	// Text to show on the menu link 
        'manage_options',	// Capability requirement to see the link 
        'custom-settings',	// The 'slug' - file to display when clicking the link 
        'custom_settings_page', 
	null, 
	99			//Ordeing
    );
}
add_action( 'admin_menu', 'custom_settings_add_menu' );
```

## Create Custom Global Settings
```php
function custom_settings_page() { ?>
	<div class="wrap">
		<h1>Custom Settings</h1>
		<form method="post" action="options.php">
            <?php
                settings_fields( 'section' );
                do_settings_sections( 'theme-options' );
                submit_button();
            ?>
		</form>
	</div>
<?php }

function setting_facebook_url() { ?>
    <input type="text" name="facebook" id="facebook" value="<?php echo get_option('facebook'); ?>" />
<?php }

function setting_twitter_url() { ?>
	<input type="text" name="twitter" id="twitter" value="<?php echo get_option( 'twitter' ); ?>" />
<?php }

function setting_linkedin_url() { ?>
	<input type="text" name="linkedin" id="linkedin" value="<?php echo get_option( 'linkedin' ); ?>" />
<?php }

function custom_settings_page_setup() {
	add_settings_section( 'section', 'All Settings', null, 'theme-options' );
	add_settings_field( 'twitter', 'Twitter URL', 'setting_twitter_url', 'theme-options', 'section' );
	add_settings_field( 'facebook', 'Facebook URL', 'setting_facebook_url', 'theme-options', 'section' );
	add_settings_field( 'linkedin', 'Linkedin URL', 'setting_linkedin_url', 'theme-options', 'section' );
	register_setting('section', 'twitter');
	register_setting( 'section', 'facebook' );
	register_setting( 'section', 'linkedin' );
}
add_action( 'admin_init', 'custom_settings_page_setup' );
```

## Remove Widget title
```php
function remove_widget_title( $widget_title ) {
    return $widget_title == '&nbsp;' ? '' : $widget_title;
}
add_filter( 'widget_title', 'remove_widget_title' );
```

## Register custom widgets
```php
function mytheme_widgets_init() {
	// Header Widget
	register_sidebar(
		array(
			'name'          => __( 'Header Content', 'mytheme' ),
			'description'   => __( 'Widget for Header Content', 'mytheme' ),
			'id'            => 'headerbar-widget',
			'before_widget' => '<div class="my_custom_header_widget">',
			'after_widget'  => '</div>',
			'before_title'  => '<span>',
			'after_title'   => '</span>',
		)
	);

	// Sidebar widget area, located in the sidebar. Empty by default.
	register_sidebar( array(
		'name' => 'Sidebar Widget Area',
		'id' => 'sidebar-widget-area',
		'description' => 'The sidebar widget area',
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );
	
	// Footer widget area, located in the footer. Empty by default.
	register_sidebar( array(
		'name' => __( 'Footer Widget Area', 'compass' ),
		'id' => 'first-footer-widget-area',
		'description' => __( 'The footer widget area', 'compass' ),
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );
}
// Hook the widget initiation and run our function
add_action( 'widgets_init', 'mytheme_widgets_init' );
```

## Register a new Navigation menu
```php
function add_theme_menu_navigation() {
    register_nav_menu( 'side-menu', __( 'Side Menu', 'mytheme' ) );
    register_nav_menu( 'header-menu', __( 'Header Menu', 'mytheme' ) );
    register_nav_menu( 'main-menu', __( 'Main Menu', 'mytheme' ) );
}

// Hook to the init action hook, run our function
add_action( 'init', 'add_theme_menu_navigation' );
```

## Hide Admin Bar in site
```php
function hide_admin_bar() { return false; }
add_filter( 'show_admin_bar', 'hide_admin_bar' );
```

## Remove WP Logo in Admin
```php
function remove_wp_logo( $wp_admin_bar ) {
    $wp_admin_bar->remove_node( 'wp-logo' );
}
add_action( 'admin_bar_menu', 'remove_wp_logo', 999 );
```

## Disable wp-cron
To disable WordPress default wp-cron behaviour by adding following line to wp-config.php file: `define('DISABLE_WP_CRON', true);`

Open crontab:
`crontab -e`

Then add a line like below in it.

`*/10 * * * * curl http://example.com/wp-cron.php?doing_wp_cron > /dev/null 2>&1`
OR
`*/10 * * * * cd /var/www/example.com/htdocs; php /var/www/example.com/htdocs/wp-cron.php?doing_wp_cron > /dev/null 2>&1`

you can also use WP-Cli

`*/10 * * * * cd /var/www/example.com/htdocs; wp cron event run --due-now > /dev/null 2>&1`

## WordPress Transients
Transients are similar in all but one property; they have an expiration time. This makes them uniquely suited to act as a cache for appropriate data. In addition to reducing the number of queries our website utilizes, they can be further speed up by caching plugins which may make WordPress store transients in fast memory (like Memcached) instead of the database.

```php
set_transient( 'my_mood', 'Pretty Awesome', 28800 ); // Site Transient
set_site_transient( 'my_mood', 'Pretty Awesome', 28800 ); // Multisite Transient

$my_mood = get_transient( 'my_mood' ); // Site Transient
$my_mood = get_site_transient( 'my_mood' ); // Multisite Transient

delete_transient( 'my_mood' ); // Site Transient
delete_site_transient( 'my_mood' ); // Multisite Transient
```

## Sanitization Function

- `absint();` // - converts value to positive integer, useful for numbers, IDs, etc.
- `esc_url_raw();` // - for inserting URL in database safely
- `sanitize_email();` // - strips out all characters that are not allowable in an email address
- `sanitize_file_name();` // - removes special characters that are illegal in filenames on certain operating system
- `sanitize_hex_color();` // - returns 3 or 6 digit hex color with #, or nothing
- `sanitize_hex_color_no_hash();` // - the same as above but without a #
- `sanitize_html_class();` // - sanitizes an HTML classname to ensure it only contains valid characters
- `sanitize_key();` // - lowercase alphanumeric characters, dashes and underscores are allowed
- `sanitize_mime_type();` // - useful to save mime type in DB, e.g. uploaded file's type
- `sanitize_option();` // - sanitizes values like update_option() and add_option() does for various option types.
- `sanitize_sql_orderby();` // - ensures a string is a valid SQL order by clause
- `sanitize_text_field();` // - removes all HTML markup, as well as extra whitespace, leaves nothing but plain text
- `sanitize_title();` // - returned value intented to be suitable for use in a URL
- `sanitize_title_for_query();` // - used for querying the database for a value from URL
- `sanitize_title_with_dashes();` // - same as above but it does not replace special accented characters
- `sanitize_user();` // - sanitize username stripping out unsafe characters
- `wp_filter_post_kses();` wp_kses_post(); // - it keeps only HTML tags which are allowed in post content as well
- `wp_kses();` // - allows only HTML tags and attributes that you specify
- `wp_kses_data();` // - sanitize content with allowed HTML Kses rules
- `wp_rel_nofollow();` // - adds rel nofollow string to all HTML A elements in content


composer create-project roots/bedrock
git add composer.json composer.lock
 
git commit -m "Update WordPress core, themes, and plugins"

To use WPackagist, our composer.json file must include the following information:
```json
{
    "repositories":[
        {
            "type":"composer",
            "url":"https://wpackagist.org"
        }
    ]
}
```

Hooks are piece of code to interact/modify another piece of code

`What Are WordPress Hooks?`
WordPress hooks allow users to manipulate WordPress without modifying its core. Learning about hooks is essential for any WordPress user as it helps them edit the default settings of themes or plugins and, ultimately, create new functions.

Hooks are divided into two categories:

- `Action` – allows users to add data or change how their websites work. An action hook will run at a specific time when the WordPress core, plugins, and themes are executing.
- `Filter` – can change data during WordPress core, plugins, and theme execution.

The WordPress Plugin API powers the functionality of WordPress hooks. You use hooks by calling certain WordPress functions called Hook Functions at specific instances during the WordPress runtime.

## What Is an Action Hook?
Actions are used to do something in the process of WordPress runtime. An action will modify the default behavior of a specific function. Actions provides a way for running your custom code at a particular point in the execution of WordPress Core, plugins, or themes.

These built-in functions are most frequently used with actions:

`add_action` attaches a certain callback function to a certain hook or hook name (as in the example in the paragraph above).
`Syntax: add_action( $hook, $function_to_add, priority, accepted_args );`
`remove_action` detaches the callback function that has been attached to the hook earlier with add_action.
`Syntax: remove_action( $hook, $function_to_remove, priority );`
`do_action` is used for creating custom action hooks; it calls the callback function added to the matching action hook.
`Syntax: do_action( $hook_name, $arg )`

```php
// Add some text after the header
add_action('__after_header', 'add_promotional_text');
function add_promotional_text()
{
	// If we're not on the home page, do nothing
	if (!is_front_page())
		return;
	// Echo the html
	echo "<div>Special offer! June only: Free chocolate for everyone!</div>";
}
//Add in header.php
do_action ( '__after_header' );
```
The default value of `$priority` is 10. the callback function with the lowest priority number will run first and the one with the highest number will run last.
`$accepted_args` is responsible for defining the number of arguments the function accepts. By default, the system will set it as 1.
`add_action()` tells WordPress to do something when it arrives at the specified `do_action()` hook.

## Custom action hooks
```php
add_action( 'my_website_link', 'my_link_echo', 10, 1 );
function my_link_echo( $url ){
    echo "Look, this is my favorite URL -- <a href= {'$url'}>$url</a>";
}
```

## Unhooking Actions and Filters
If you want to disable a command from add_action() or add_filter() in your WordPress code, use remove_action() and remove_filter().

```php
remove_action( 'wp_print_footer_scripts', 'hostinger_custom_footer_scripts');
add_action( 'wp_print_footer_scripts', 'hostinger_custom_footer_scripts_theme');

remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
remove_action( 'wp_print_styles', 'print_emoji_styles' );
remove_action( 'admin_print_styles', 'print_emoji_styles' );
remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
add_filter( 'tiny_mce_plugins', 'disable_emojis_tinymce' );
add_action( 'init', 'disable_emojis' );
```

## What is a Filter Hook?
A filter will modify the default behavior of a specific function. It does this by manipulating the data it receives and returning that data to WordPress before it is displayed in the browser. Filter hooks always return something: because filters are literally filtering and modifying content, they should return filtered/modified content.

These built-in functions are most frequently used with filters:

`add_filter` hooks a callback function to the certain filter hook.
`Syntax: add_filter( $hook_name, $callback, priority, accepted_args );`
`remove_filter` detaches a callback function from the certain hook.
`Syntax: remove_filter( $hook_name, $callback, priority );`
`apply_filters` calls a callback attached to the filter hook. 
`Syntax: apply_filters( $hook_name, $value_to_be_filtered, $args ).`

Creating a Filter Hook
You can create a filter hook by utilizing the add_filter() function. The filter hook modifies, filters, or replaces a value with a new one. Similar to an action hook, it filters a value with the associated filter hook functions like apply_filter.

```
function wpb_custom_excerpt( $output ) {
  if ( has_excerpt() && ! is_attachment() ) {
    $output .= wpb_continue_reading_link();
  }
  return $output;
}
add_filter( 'get_the_excerpt', 'wpb_custom_excerpt' );
```


creating a plugin. create a new folder in your /wp-content/plugins/ directory. you can name it anything you want. As per WordPress guidelines, you need to create a PHP file with the same name inside your plugin directory.
```
/*
Plugin Name:  MyPlugin
Version    :  1.0
Description:  Demonstrating WordPress Hooks (Actions and Filters) with multiple examples.
Author     :  Balakarthikeyan
Author URI :  https://www.example.com/
License    :  GPLv2 or later
License URI:  https://www.gnu.org/licenses/gpl-2.0.html
Text Domain:  myplugin
*/

//=================================================
// Security: Abort if this file is called directly
//=================================================
if ( !defined('ABSPATH') ) { 
    die;
}
```
Save this file and then activate the plugin in your WordPress dashboard


WordPress includes a built-in action called `admin_init`. It fires while the admin screen is being initialized, while the `init` action fires only after WordPress has finished loading and authenticated the user, but before any headers are sent.


How WordPress Core wp_head action?
WordPress Core itself uses many of its built-in actions to perform various functions.

Take the wp_head action for example. It’s fired when WordPress is outputting the header section of the webpages (the code that goes between <head> and </head>).

```php
add_action( 'wp_head', 'rest_output_link_wp_head', 10, 0 );
add_action( 'wp_head', '_wp_render_title_tag', 1 );
add_action( 'wp_head', 'wp_enqueue_scripts', 1 );
add_action( 'wp_head', 'wp_resource_hints', 2 );
add_action( 'wp_head', 'feed_links', 2 );
add_action( 'wp_head', 'feed_links_extra', 3 );
add_action( 'wp_head', 'rsd_link' );
add_action( 'wp_head', 'wlwmanifest_link' );
add_action( 'wp_head', 'adjacent_posts_rel_link_wp_head', 10, 0 );
add_action( 'wp_head', 'locale_stylesheet' );
add_action( 'wp_head', 'noindex', 1 );
add_action( 'wp_head', 'print_emoji_detection_script', 7 );
add_action( 'wp_head', 'wp_print_styles', 8 );
add_action( 'wp_head', 'wp_print_head_scripts', 9 );
add_action( 'wp_head', 'wp_generator' );
add_action( 'wp_head', 'rel_canonical' );
add_action( 'wp_head', 'wp_shortlink_wp_head', 10, 0 );
add_action( 'wp_head', 'wp_custom_css_cb', 101 );
add_action( 'wp_head', 'wp_site_icon', 99 );
add_action( 'wp_head', 'wp_no_robots' );
```

has_action() function checks whether an action has been hooked. It accepts two parameters. has_action( 'action_name', 'function_to_check' );

did_action() function to count the number of times any action is fired, to run a callback function only the first time an action is run and never again. did_action( 'action_name' );

doing_action()
This action function checks whether the specified action is being run or not. It returns a boolean value (true or false).


Show a Maintenance Message to Your Site Visitors
add_action( 'get_header', 'maintenance_message' );
function maintenance_message() {
    if (current_user_can( 'edit_posts' )) return;
    wp_die( '<h1>Stay Pawsitive!</h1><br>Sorry, we\'re temporarily down for maintenance right meow.' );
}

Hide Dashboard Menu Items from Non-Admin Users
add_action( 'admin_menu', 'hide_admin_menus' );
function hide_admin_menus() {
    if (current_user_can( 'create_users' )) return;
    if (wp_get_current_user()->display_name == "Bala") return; 
    remove_menu_page( 'plugins.php' ); 
    remove_menu_page( 'themes.php' ); 
    remove_menu_page( 'tools.php' ); 
    remove_menu_page( 'users.php' ); 
    remove_menu_page( 'edit.php?post_type=page' ); 
    remove_menu_page( 'options-general.php' );
}


add_filter( 'the_content', 'do_blocks', 9 );
add_filter( 'the_content', 'wptexturize' );
add_filter( 'the_content', 'convert_smilies', 20 );
add_filter( 'the_content', 'wpautop' );
add_filter( 'the_content', 'shortcode_unautop' );
add_filter( 'the_content', 'prepend_attachment' );
add_filter( 'the_content', 'wp_make_content_images_responsive' );
add_filter( 'the_content', 'do_shortcode', 11 ); // AFTER wpautop(). 


// hook into the 'comment_text' filter with the callback function
add_filter( 'comment_text', 'the_profanity_filter' );

// define a callback function to filter profanities in comments 
function the_profanity_filter( $comment_text ) {
    // define an array of profane words and count how many are there 
    $profaneWords = array('fudge', 'darn', 'pickles', 'blows', 'dangit');
    $profaneWordsCount = sizeof($profaneWords);
    
    // loop through the profanities in $comment_text and replace them with '*'
    for($i=0; $i < $profaneWordsCount; $i++) {
        $comment_text = str_ireplace( $profaneWords[$i], str_repeat('*', strlen( $profaneWords[$i]) ), $comment_text );
    } 
    
    return $comment_text;
}


A WordPress child theme is a theme that works parent theme from which it inherits all the functionality and styling.

## What is a child theme?
The extension of a parent theme is a child theme. In case you make changes to the parent theme, then any update will undo the changes. When working on a child theme, the customizations are preserved on an update.

Development best practices recommend using child themes to edit, update, or customize exiting WordPress themes as a safe way to maintain their design and code.

Every WordPress child theme must have two files as a minimum: a stylesheet and a functions file.

Your child theme doesn’t have to include any other files. if a template file doesn’t exist in the child theme, WordPress will use the file from the parent theme.

Template files to override the same file from the parent theme, such as page.php when you want to customize the display of static pages.
Template parts such as header.php or footer.php when you want to customize these parts of the site design.

When To Use a Child Theme in WordPress 
Edit one or more of the template files.
Add extra functions that are related to display and not functionality.
Override one or more functions from the parent theme.
Add extra template file(s).
Easy extending and customization: 
Hassle-free updates:

How to Create a WordPress Child Theme
Before you create your file, you need to create a folder to hold your theme. This goes in the wp-content/themes file of your WordPress installation.
```php
/*
Theme Name:  My Child Theme. Child for Twenty Nineteen.
Theme URI:  https://example.com
Description:  Theme to support tutsplus tutorial. Child theme for the Twenty Nineteen theme.
Author:  Balakarthikeyan
Textdomain:  mychildtheme
Author URI:  https://example.com/
Template:  twentynineteen
Version:  1.0
License:  GNU General Public License v2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html                 
*/
```

Theme Name: The unique name for the theme.
Theme URI: Where users can find the code or documentation for the theme.
Description: Descriptive text to help users understand what the theme does..
Author: your name
Textdomain: this is used for internationalization. Use the text domain as the second parameter in any internationalization functions.
Author URI: The author’s website.
Template: The folder where the parent theme is stored. Use the folder name and not the theme name. Without this line, your theme won’t work as a child theme.
Version: The version number
License: The license, which must be GNU. [link]
License URI: The link to information on the license.

The Functions File
The next step is to add a functions file to your child theme. You need this so that you can enqueue the stylesheet from the parent theme

/* enqueue script for parent theme stylesheeet */        
function childtheme_parent_styles() {
 
 // enqueue style
 wp_enqueue_style( 'parent', get_template_directory_uri().'/style.css' );                       
}
add_action( 'wp_enqueue_scripts', 'childtheme_parent_styles');
WordPress will know to use the files from the parent theme unless you add new files to the child theme that override them



What is the wp_options table?
The wp_options table contains all sorts of data for your WordPress site such as:

- Site URL, home URL, admin email, default category, posts per page, time format, etc
- Settings for plugins, themes, widgets
- Temporarily cached data

the wp_options table is the autoload field. This contains a yes or a no value (flag). This essentially controls whether or not it is loaded by the wp_load_alloptions() function.

Autoloaded data is data that is loaded on every page of your WordPress site. 

SELECT SUM(LENGTH(option_value)) as autoload_size FROM wp_options WHERE autoload='yes';


SELECT 'autoloaded data in KiB' as name, ROUND(SUM(LENGTH(option_value))/ 1024) as value FROM wp_options WHERE autoload='yes'
UNION
SELECT 'autoloaded data count', count(*) FROM wp_options WHERE autoload='yes'
UNION
(SELECT option_name, length(option_value) FROM wp_options WHERE autoload='yes' ORDER BY length(option_value) DESC LIMIT 10)

SELECT option_name, length(option_value) AS option_value_length FROM wp_options WHERE autoload='yes' ORDER BY option_value_length DESC LIMIT 10;

Clean up Transients
Unless you’re using an object cache, WordPress stores transient records in the wp_options table.
SELECT * 
FROM `wp_options` 
WHERE `autoload` = 'yes'
AND `option_name` LIKE '%transient%'

SELECT * 
FROM `wp_options` 
WHERE `option_name` LIKE '_wp_session_%'

Add an Index to Autoload



we can go a step further and add or remove plugins programmatically by taking advantage of the option_active_plugins filter. 

$request_uri = parse_url( $_SERVER['REQUEST_URI'], PHP_URL_PATH );

$is_admin = strpos( $request_uri, '/wp-admin/' );

if( false === $is_admin ){
	add_filter( 'option_active_plugins', function( $plugins ){

		global $request_uri;

		$is_contact_page = strpos( $request_uri, '/contact/' );

		$myplugins = array( 
			"contact-form-7/wp-contact-form-7.php", 
			"code-snippets/code-snippets.php",
			"query-monitor/query-monitor.php",
			"autoptimize/autoptimize.php" 
		);

		<!-- $k = array_search( $myplugin, $plugins );

		if( false !== $k && false === $is_contact_page ){
			unset( $plugins[$k] );
		} -->

		if( false === $is_contact_page ){
			$plugins = array_diff( $plugins, $myplugins );
		}

		return $plugins;

	} );
}