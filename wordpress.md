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
- `Editor:` The profile(s) that can create, edit, publish theirs, and other users’ posts.
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
Transients are similar in all but one property; they have an expiration time. This makes them uniquely suited to act as a cache for appropriate data. In addition to reducing the number of queries our website utilizes, they can be further sped up by caching plugins which may make WordPress store transients in fast memory (like Memcached) instead of the database.

```php
set_transient( 'my_mood', 'Pretty Awesome', 28800 ); // Site Transient
set_site_transient( 'my_mood', 'Pretty Awesome', 28800 ); // Multisite Transient

$my_mood = get_transient( 'my_mood' ); // Site Transient
$my_mood = get_site_transient( 'my_mood' ); // Multisite Transient

delete_transient( 'my_mood' ); // Site Transient
delete_site_transient( 'my_mood' ); // Multisite Transient
```

What is a child theme?
The extension of a parent theme is a child theme. In case you make changes to the parent theme, then any update will undo the changes. When working on a child theme, the customizations are preserved on an update.

What is usermeta function in Wordpress?
The usermeta function is used to retrieve the metadata of users. It can return a single value or an array of metadata.

Syntax: get_user_meta( int $user_id, string $key = '', bool $single = false )

$user_id is the required user id parameter
$key is the optional parameter which is the meta key to retrieve. By default, it returns data for all key values.
$single is an optional parameter that tells whether the single value will return. By default, it is false.


/////////////////////////////////////////////
//LIST OF WORDPRESS SANITIZATION FUNCTIONS //
/////////////////////////////////////////////

- absint(); // - converts value to positive integer, useful for numbers, IDs, etc.
- esc_url_raw(); // - for inserting URL in database safely
- sanitize_email(); // - strips out all characters that are not allowable in an email address
- sanitize_file_name(); // - removes special characters that are illegal in filenames on certain operating system
- sanitize_hex_color(); // - returns 3 or 6 digit hex color with #, or nothing
- sanitize_hex_color_no_hash(); // - the same as above but without a #
- sanitize_html_class(); // - sanitizes an HTML classname to ensure it only contains valid characters
- sanitize_key(); // - lowercase alphanumeric characters, dashes and underscores are allowed
- sanitize_mime_type(); // - useful to save mime type in DB, e.g. uploaded file's type
- sanitize_option(); // - sanitizes values like update_option() and add_option() does for various option types. Here is the list of - avaliable options: https://codex.wordpress.org/Function_Reference/sanitize_option#Notes
- sanitize_sql_orderby(); // - ensures a string is a valid SQL order by clause
- sanitize_text_field(); // - removes all HTML markup, as well as extra whitespace, leaves nothing but plain text
- sanitize_title(); // - returned value intented to be suitable for use in a URL
- sanitize_title_for_query(); // - used for querying the database for a value from URL
- sanitize_title_with_dashes(); // - same as above but it does not replace special accented characters
- sanitize_user(); // - sanitize username stripping out unsafe characters
- wp_filter_post_kses(); wp_kses_post(); // - it keeps only HTML tags which are allowed in post content as well
- wp_kses(); // - allows only HTML tags and attributes that you specify
- wp_kses_data(); // - sanitize content with allowed HTML Kses rules
- wp_rel_nofollow(); // - adds rel nofollow string to all HTML A elements in content

What is REST API?
REST API (Representational State Transfer Application Programming Interface) is a newer and lightweight mode using which the developers enjoy the convenience of connecting WordPress with other applications.

WordPress has come across various vulnerability issues such as –

- Authenticated File Delete.
- Authenticated Post Type Bypass.
- PHP Object Injection via Metadata.
- Authenticated Cross-Site Scripting (XSS).
- Cross-Site Scripting (XSS) that could affect plugins.
- User Activation Screen Search Engine Indexing.
- File Upload to XSS on Apache Web Servers.


When a typical AJAX request to admin-ajax.php is made, it loads a few other core WordPress files to make sure the core functions are loaded.

- /wp-load.php
- /wp-config.php
- /wp-settings.php (loads most core files, all active plugins and themes, and the REST API)
- /wp-admin/includes/admin.php
- /wp-admin/includes/ajax-actions.php
After loading these files, WordPress calls the admin_init hook

The below core functions are registered on this hook in WordPress 6.3:

- handle_legacy_widget_preview_iframe
- wp_admin_headers
- default_password_nag_handler
- WP_Privacy_Policy_Content::text_change_check
- WP_Privacy_Policy_Content::add_suggested_content
- register_setting
- add_privacy_policy_content
- send_frame_options_header
- register_admin_color_schemes
- _wp_check_for_scheduled_split_terms
- _wp_check_for_scheduled_update_comment_type
- _wp_admin_bar_init
- wp_schedule_update_network_counts
- _maybe_update_core
- _maybe_update_plugins
- _maybe_update_themes


REST endpoints are handled by the WordPress Rewrite API, the request is passed to /index.php, which then loads the rest of WordPress normally.

- /index.php
- /wp-blog-header.php
- /wp-load.php
- /wp-config.php
- /wp-settings.php (loads most core files, all active plugins and themes, and the REST API)

Hooks, which are basically placeholders developers, to insert their own custom code for processing as a page is loading.

Filter hooks, let you intercept and modify data as it’s being processed. Basically, filters let you manipulate data coming out of the database before going to the browser, or coming from the browser before going into the database

Action hooks, lets you add extra functionality at specific points in the processing and loading of a page.

A function is a piece of custom code that specifies how something will happen



Noodp = No Open Directory Project
Noydir = No Yahoo Directory

The Open Directory Project and Yahoo Directory used to be massive online directories that contained links to websites along with their titles and descriptions.

Noodp and noydir are meta tags that are added to the html code of pages.

Meta robots tags are code that is hidden from your site’s visitors but provides some instructions to search engine bots.

Noodp looks like this:

<meta name="robots" content="noodp" />
Noydir looks like this:

<meta name="robots" content="noydir" />