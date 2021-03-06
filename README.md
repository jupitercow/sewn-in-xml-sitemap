# Sewn In XML Sitemap

Simple way to automatically generate XML Sitemaps when a page or post is saved. Very simple, no cruft or extra features you won't use. There are two main customizations available.

*	Choose which post types are added (posts and pages by default)
*	Adds a meta box to all included post types to remove single posts from being added to the sitemap

It also works well with our [Sewn In Simple SEO](https://github.com/jupitercow/sewn-in-simple-seo) plugin. When both are installed, they integrate together.

## Control what post types are added

```php
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
```

## Remove a specific post from the sitemap

A checkbox is added to each post type that is included in the sitemap. Checking it will remove that specific item from the sitemap.

This checkbox also removes posts from wp_list_pages, you can turn that off using this filter:

```php
add_filter( 'sewn/sitemap/wp_list_pages', '__return_false' );
```

```php
/**
 * This filter arrived in 2.0.3
 *
 * Remove specific posts programatically. This could go into functions.php or a plugin.
 *
 * This example removes all posts that have post meta field of "test" set.
 */
add_filter( 'sewn/sitemap/post', 'custom_remove_test_posts', 10, 2 );
function custom_remove_test_posts( $value, $post ) {
	$status = get_metadata( 'post', $post->ID, 'test', true );
	if ( $status ) {
		$value = false;
	}
	return $value;
}
```

## Change frequency for a post type, could also be used an a single post basis by checking the the $post-ID or $post->post_name

```php
/**
 * Change sitemap frequency in XML, default is "monthly"
 *
 * options: always, hourly, daily, weekly, monthly, yearly, never
 */
add_filter( 'sewn/sitemap/frequency', 'custom_sitemap_frequency', 10, 2 );
function custom_sitemap_frequency( $frequency, $post ) {
	if ( 'news' == get_post_type($post) ) {
		$frequency = 'daily';
	}
	return $frequency;
}
```

## Compatibility

Works with the our [Sewn In Simple SEO](https://github.com/jupitercow/sewn-in-simple-seo) plugin. When installed, the XML sitemap checkbox integrates with the SEO fields and this plugin will use the SEO post types. The goal is to keep things very simple and integrated.