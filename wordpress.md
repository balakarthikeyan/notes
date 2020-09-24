<?php

// Customer Header & Logo
function acetheme_custom_header_setup() {
    add_theme_support( 'custom-header', apply_filters( 'acetheme_custom_header_args', array(
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
add_action( 'after_setup_theme', 'acetheme_custom_header_setup' );

/**
 * Register Custom Navigation Walker
 */
function register_navwalker(){
    if ( ! file_exists( get_template_directory() . '/lib/bootstrap-navwalker.php' ) ) {
        return new WP_Error( 'bootstrap-navwalker-missing', __( 'It appears the bootstrap-navwalker.php file may be missing.', 'bootstrap-navwalker' ) );
    } else {
        require_once get_template_directory() . '/lib/bootstrap-navwalker.php';
    }    
}
add_action( 'after_setup_theme', 'register_navwalker' );

// for <title> load from Wordpress
add_theme_support( 'title-tag' );

// Enqueue scripts and styles.
function acetheme_scripts() {
    $query_args = array( 'family' => 'Open+Sans:400,600,700,800' );
    // wp_register_style('OpenSans', 'http://fonts.googleapis.com/css?family=Open+Sans:400,600,700,800');
    // wp_enqueue_style( 'OpenSans');
    wp_enqueue_style( 'google-fonts', add_query_arg( $query_args, "//fonts.googleapis.com/css" ) );
    wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/css/bootstrap.min.css', array(), '3.3.7' );
    wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/css/bootstrap-theme.min.css', array(), '3.3.7' );
    wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/scripts/bootstrap/js/bootstrap.min.js', array("jquery"), '3.3.7', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery.min.js', array(), '2.2.4', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery-migrate.js', array(), '1.4.1', true );
    wp_enqueue_script( 'jq', get_template_directory_uri() . '/scripts/jquery/jquery/jquery-noconflict.js', array(), '', true );
    wp_enqueue_script( 'scrollify', get_template_directory_uri() . '/scripts/jquery/jquery.scrollify.js', array(), '1.0.20', true );
    // Custom styles & script for this template
    wp_enqueue_script( 'theme-js', get_template_directory_uri() . '/scripts/theme.js', array(), '', true );
    wp_enqueue_style( 'theme-style', get_template_directory_uri() . '/style.css' );
    wp_enqueue_style( 'theme-css', get_template_directory_uri() . '/css/theme.css' );
}
add_action( 'wp_enqueue_scripts', 'acetheme_scripts' );

// Custom Settings for Social Media
function custom_settings_add_menu() {
    add_menu_page( 
        'Custom Settings', // Title of the page 
        'Custom Settings', // Text to show on the menu link 
        'manage_options', // Capability requirement to see the link 
        'custom-settings', // The 'slug' - file to display when clicking the link 
        'custom_settings_page', null, 99 );
}
add_action( 'admin_menu', 'custom_settings_add_menu' );

// Create Custom Global Settings
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

//Remove Widget title
function remove_widget_title( $widget_title ) {
    return $widget_title == '&nbsp;' ? '' : $widget_title;
}
add_filter( 'widget_title', 'remove_widget_title' );

// Register a new sidebar for Header Widget
function add_headerbar_widget() {
    register_sidebar(
      array(
        'name'          => __( 'Header Content', 'acetheme' ),
        'description'   => __( 'Widget for Header Content', 'acetheme' ),
        'id'            => 'headerbar-widget',
        'before_widget' => '<div class="ace_custom_header_widget">',
        'after_widget'  => '</div>',
        'before_title'  => '<span>',
        'after_title'   => '</span>',
      )
    );
}

// Hook the widget initiation and run our function
add_action( 'widgets_init', 'add_headerbar_widget' );

// register sidebar widgets
function acetheme_widgets_init() {
	
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
	
	// First footer widget area, located in the footer. Empty by default.
	register_sidebar( array(
		'name' => __( 'First Footer Widget Area', 'compass' ),
		'id' => 'first-footer-widget-area',
		'description' => __( 'The first footer widget area', 'compass' ),
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );

	// Second Footer Widget Area, located in the footer. Empty by default.
	register_sidebar( array(
		'name' => 'Second Footer Widget Area',
		'id' => 'second-footer-widget-area',
		'description' => 'The second footer widget area',
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );

	// Third Footer Widget Area, located in the footer. Empty by default.
	register_sidebar( array(
		'name' => 'Third Footer Widget Area',
		'id' => 'third-footer-widget-area',
		'description' => 'The third footer widget area',
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );

	// Fourth Footer Widget Area, located in the footer. Empty by default.
	register_sidebar( array(
		'name' => 'Fourth Footer Widget Area',
		'id' => 'fourth-footer-widget-area',
		'description' => 'The fourth footer widget area',
		'before_widget' => '<div id="%1$s" class="widget-container %2$s">',
		'after_widget' => '</div>',
		'before_title' => '<h3 class="widget-title">',
		'after_title' => '</h3>',
	) );
}
add_action( 'widgets_init', 'acetheme_widgets_init' );

// Register a new Navigation menu
function add_theme_menu_navigation() {
    register_nav_menu( 'side-menu', __( 'Side Menu', 'acetheme' ) );
    register_nav_menu( 'header-menu', __( 'Header Menu', 'acetheme' ) );
    register_nav_menu( 'main-menu', __( 'Main Menu', 'acetheme' ) );
}

// Hook to the init action hook, run our function
add_action( 'init', 'add_theme_menu_navigation' );

function hide_admin_bar() { return false; }
add_filter( 'show_admin_bar', 'hide_admin_bar' );

function remove_wp_logo( $wp_admin_bar ) {
    $wp_admin_bar->remove_node( 'wp-logo' );
}
add_action( 'admin_bar_menu', 'remove_wp_logo', 999 );


<?php
/**
 * Plugin Name: A Simple Widget
 * Plugin URI: https://www.github.com/balakarthikeyan/wp-plugins
 * Description: A widget that displays authors name.
 * Version: 1.0.0
 * Author: Balakarthikeyan
 * Author URI: https://www.github.com/balakarthikeyan
 * License: GPLv2 or later
 */

/**
 * Class Simple_Widget
 */
class Simple_Widget extends WP_Widget {

	/**
	 * Initializing the widget
	 */
	public function __construct() {
		$widget_ops = array( 'classname' => 'example', 'description' => __('A widget that displays the authors name', 'example') );
		
		$control_ops = array( 'width' => 300, 'height' => 350, 'id_base' => 'example-widget' );
		
		parent::__construct( 'example-widget', __('Example Widget', 'example'), $widget_ops, $control_ops );
	}

	/**
	 * Displaying the widget on the back-end
	 * @param  array $instance An instance of the widget
	 */
	function form( $instance ) {

		//Set up some default widget settings.
		$defaults = array( 'title' => __('Example', 'example'), 'name' => __('Balakarthikeyan', 'example'), 'show_info' => true );
		$instance = wp_parse_args( (array) $instance, $defaults ); ?>

		<!--Widget Title: Text Input.-->
		<p>
			<label for="<?php echo $this->get_field_id( 'title' ); ?>"><?php _e('Title:', 'example'); ?></label>
			<input id="<?php echo $this->get_field_id( 'title' ); ?>" name="<?php echo $this->get_field_name( 'title' ); ?>" value="<?php echo stripslashes_deep ( esc_attr ($instance['title']) ); ?>" class="widefat" />
		</p>

		<!--Text Input.-->
		<p>
			<label for="<?php echo $this->get_field_id( 'name' ); ?>"><?php _e('Your Name:', 'example'); ?></label>
			<input id="<?php echo $this->get_field_id( 'name' ); ?>" name="<?php echo $this->get_field_name( 'name' ); ?>" value="<?php echo stripslashes_deep ( esc_attr ($instance['name']) ); ?>" class="widefat" />
		</p>

		
		<!--Checkbox.-->
		<p>
			<input class="checkbox" type="checkbox" <?php checked( $instance['show_info'], true ); ?> id="<?php echo $this->get_field_id( 'show_info' ); ?>" name="<?php echo $this->get_field_name( 'show_info' ); ?>" /> 
			<label for="<?php echo $this->get_field_id( 'show_info' ); ?>"><?php _e('Display info publicly?', 'example'); ?></label>
		</p>

	<?php
	}

	/**
	 * Making the widget updateable
	 * @param  array $new_instance New instance of the widget
	 * @param  array $old_instance Old instance of the widget
	 * @return array An updated instance of the widget
	 */
	function update( $new_instance, $old_instance ) {
		$instance = $old_instance;

		//Strip tags from title and name to remove HTML 
		$instance['title'] = strip_tags( $new_instance['title'] );
		$instance['name'] = strip_tags( $new_instance['name'] );
		$instance['show_info'] = $new_instance['show_info'];

		return $instance;
	}

	/**
	 * Displaying the widget on the front-end
	 * @param  array $args     Widget options
	 * @param  array $instance An instance of the widget
	 */			
	public function widget( $args, $instance ) {
		extract( $args );

		//Our variables from the widget settings.
		$title = apply_filters('widget_title', $instance['title'] );
		$name = $instance['name'];
		$show_info = isset( $instance['show_info'] ) ? $instance['show_info'] : false;

		echo $before_widget;

		// Display the widget title 
		if ( $title )
			echo $before_title . $title . $after_title;

		//Display the name 
		if ( $name )
			printf( '<p>' . __('My name is %1$s.', 'example') . '</p>', $name );

		
		if ( $show_info )
			printf( $name );

		
		echo $after_widget;
	}
}

function Simple_Widget() {
	register_widget( 'Simple_Widget' );
}

add_action( 'widgets_init', 'Simple_Widget' );

<?php
/**
 * @package Hello Plugin
 * @author Balakarthikeyan
 * @license GPL-2.0+
 * @link https://www.github.com/balakarthikeyan/wp-plugins
 * @copyright 2020. All rights reserved.
 *
 *            @wordpress-plugin
 *            Plugin Name: Hello Plugin
 *            Plugin URI: https://www.github.com/balakarthikeyan/wp-plugins
 *            Description: Hello Plugin is the simplest WordPress plugin for beginner.
 *            Version: 1.0.0
 *            Author: Balakarthikeyan
 *            Author URI: https://www.github.com/balakarthikeyan
 *            Text Domain: hello-plugin
 *            Contributors: 
 *            License: GPL-2.0+
 *            License URI: http://www.gnu.org/licenses/gpl-2.0.txt
 */
 
/**
 * Adding Submenu under Settings Tab
 *
 * @since 1.0
 */
function hello_plugin_add_menu() {
	add_submenu_page ( "options-general.php", "Plugin", "Plugin", "manage_options", "hello-plugin", "hello_plugin_page" );
}
add_action ( "admin_menu", "hello_plugin_add_menu" );

/**
 * Setting Page Options
 * - add setting page
 * - save setting page
 *
 * @since 1.0
 */
function hello_plugin_page() {
?>
<div class="wrap">
	<h1> Hello Plugin </h1>
	<form method="post" action="options.php">
		<?php
		settings_fields ( "hello_plugin_config" );
		do_settings_sections ( "hello_plugin" );
		submit_button ();
		?>
    </form>
</div>
<?php
}
 
/**
 * Init setting section, Init setting field and register settings page
 *
 * @since 1.0
 */
function hello_plugin_settings() {
	add_settings_section ( "hello_plugin_config", "", null, "hello_plugin" );
	add_settings_field ( "hello-plugin-text", "This is sample Textbox", "hello_plugin_options", "hello_plugin", "hello_plugin_config" );
	register_setting ( "hello_plugin_config", "hello-plugin-text" );
}
add_action ( "admin_init", "hello_plugin_settings" );
 
/**
 * Add simple textfield value to setting page
 *
 * @since 1.0
 */
function hello_plugin_options() {
	?>
<div class="postbox">
	<input type="text" name="hello-plugin-text" value="<?php echo stripslashes_deep ( esc_attr ( get_option ( 'hello-plugin-text' ) ) );?>" /> Provide any text value here for testing<br />
</div>
<?php
}
 
/**
 * Append saved textfield value to each post
 *
 * @since 1.0
 */
add_filter ( 'the_content', 'hello_plugin_content' );
function hello_plugin_content($content) {
	return $content . stripslashes_deep ( esc_attr ( get_option ( 'hello-plugin-text' ) ) );
}

add_shortcode( 'show_today_posts', 'test_plugin_check' );
function test_plugin_check() {

	$args = array(
		'posts_per_page' => 3,
		'post_type' => 'any',
		'post_status' => 'publish',
		'date_query' => array(
			//'after' => array(
				'year' => date( 'Y' ),
				'month' => date( 'm' ),
			//),
			'day' => date ('d'),
		),
		'orderby' => 'date',
		'order' => 'DESC',
	);
	// var_dump( $args );
	$query = new WP_Query( $args );
	// var_dump( $query );
	ob_start();

	while( $query->have_posts() ) :
		$query->the_post(); ?>
	
		<h2><?php the_title(); ?></h2> By <?php the_author(); ?> on <?php the_date(); ?>
	
	<?php endwhile;
	
	wp_reset_postdata();
	
	return ob_get_clean();
}

