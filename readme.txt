=== Sewn In XML Sitemap ===
Contributors: jcow, ekaj
Donate link: https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=jacobsnyder%40gmail%2ecom&lc=US&item_name=Jacob%20Snyder&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donate_SM%2egif%3aNonHosted
Tags: xml sitemap,sitemap,seo
Requires at least: 3.6.1
Tested up to: 4.8.1
Stable tag: 2.0.6
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Simple way to automatically generate XML Sitemaps when a page or post is saved. Very simple, no cruft or extra features you won't use.

== Description ==

= 2.0.3 changed default post types used from only 'post' and 'page' to ALL public post types except "attachment". This is a cleaner approach, but may require customization. There is more info on customization below. =

This site doesn't notify of new support threads, so send a [contact](http://jcow.com/) through our form to notify of a new thread for a quicker response.

Simple way to automatically generate XML Sitemaps when a page or post is saved. Very simple, no cruft or extra features you won't use. There are two main customizations available.

*	Choose which post types are added (posts and pages by default)
*	Adds a meta box to all included post types to remove single posts from being added to the sitemap

It also works well with our [Sewn In Simple SEO](https://wordpress.org/plugins/sewn-in-simple-seo/) plugin. When both are installed, they integrate together.

= Control what post types are added =

`
/**
 * Completely replace the post types in the XML sitemap
 *
 * This will replace the default completely. Returns: array('news','event')
 *
 * The result is to remove 'post' and 'page' post types and to add 'news' and 
 * 'event' post types
 *
 * @param	array	$post_types	List of post types to be added to the XML Sitemap
 * @return	array	$post_types	Modified list of post types
 */
add_filter( 'sewn/seo/post_types', 'custom_seo_post_types' );
function custom_seo_post_types( $post_types )
{
	$post_types = array('news','event');
	return $post_types;
}
`

= Remove a specific post from the sitemap =

A checkbox is added to each post type that is included in the sitemap. Checking it will remove that specific item from the sitemap.

This checkbox also removes posts from wp_list_pages, you can turn that off using this filter:

`
add_filter( 'sewn/sitemap/wp_list_pages', '__return_false' );
`

`
/**
 * This filter arrived in 2.0.3
 *
 * Remove specific posts programatically. This could go into functions.php or a plugin.
 *
 * This example removes all posts that have post meta field of "test" set.
 */
add_filter( 'sewn/sitemap/post', 'custom_remove_test_posts', 10, 2 );
function custom_remove_test_posts( $value, $post )
{
	$status = get_metadata( 'post', $post->ID, 'test', true );
	if ( $status ) {
		$value = false;
	}
	return $value;
}
`

= Change frequency for a post type, could also be used an a single post basis by checking the the $post-ID or $post->post_name =

`
/**
 * Change sitemap frequency in XML, default is "monthly"
 *
 * options: always, hourly, daily, weekly, monthly, yearly, never
 */
add_filter( 'sewn/sitemap/frequency', 'custom_sitemap_frequency', 10, 2 );
function custom_sitemap_frequency( $frequency, $post )
{
	if ( 'news' == get_post_type($post) ) {
		$frequency = 'daily';
	}
	return $frequency;
}
`

= Compatibility =

Works with the [Sewn In Simple SEO](https://wordpress.org/plugins/sewn-in-simple-seo/) plugin. When installed, the XML sitemap checkbox integrates with the SEO fields and this plugin will use the SEO post types. The goal is to keep things very simple and integrated.


== Installation ==

*   Copy the folder into your plugins folder, or use the "Add New" plugin feature.
*   Activate the plugin via the Plugins admin page

== Frequently Asked Questions ==

= No questions yet. =

== Screenshots ==

1. The XML Sitemap panel with checkbox to remove posts in the backend.
1. The [Sewn In Simple SEO](https://wordpress.org/plugins/sewn-in-simple-seo/) panel with XML Sitemap & [Sewn In Simple Social Optimization](https://wordpress.org/plugins/sewn-in-simple-social/) installed.


== Changelog ==

= 2.0.6 - 2017-09-02 =

*   Removed array short syntax, updated meta style.

= 2.0.5 - 2017-08-30 =

*   Updated meta boxes, images.

= 2.0.4 - 2016-01-29 =

*   Fixed bug in meta boxes. Updated readme.

= 2.0.3 - 2015-07-27 =

*   Updated the default post types to ALL public post types except 'attachment'.
*	Add filter for posts on sitemap creation to granularly control which posts are added when needed.
*	Added filter for "frequency".

= 2.0.2 - 2015-02-13 =

*   Fixed problem with post_types in new system.

= 2.0.1 - 2015-02-13 =

*   Bug with the get_field method and connecting to SEO plugin.

= 2.0.0 - 2015-02-12 =

*   Updated to remove ACF dependency and cleanup functionality.

= 1.0.3 - 2014-08-03 =

*   Added to the repo.

== Upgrade Notice ==

= 2.0.6 =
Removed array short syntax, so that we can now support WordPress PHP requirements (5.2.4).

= 2.0.0 =
There are some basic compatibility issues with some of the filters and actions from 1.0.x.

= 1.0.3 =
This is the first version in the Wordpress repository.
