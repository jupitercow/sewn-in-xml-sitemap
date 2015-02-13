# Sewn In XML Sitemap

A nice and simple way to create XML Sitemaps when a page or post is saved. Very simple, no cruft or extra features you won't use. It also works well with our Sewn In Simple SEO plugin. When both are installed, they integrate together.

## Control what post types are added

By default only pages and posts are added, but you can remove either of those and/or add more using this filter:

```php
/**
 * Add a post type to the XML sitemap
 *
 * Takes the default array('post','page') and adds 'news' and 'event' post types 
 * to it. Returns: array('post','page','news','event')
 *
 * @param	array	$post_types	List of post types to be added to the XML Sitemap
 * @return	array	$post_types	Modified list of post types
 */
add_filter( 'sewn/seo/post_types', 'custom_seo_post_types' );
function custom_seo_post_types( $post_types )
{
	$post_types[] = 'news';
	$post_types[] = 'event';
	return $post_types;
}
```

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

## Customize WordPress SEO plugin

Works with the our Sewn In Simple SEO plugin. When installed, the XML sitemap checkbox integrates with the SEO fields and this plugin will use the SEO post types. The goal is to keep things very simple and integrated.