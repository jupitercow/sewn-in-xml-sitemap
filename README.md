# Sewn In XML Sitemap

A nice and simple way to create XML Sitemaps when a page or post is saved. Very simple, no cruft or extra features you won't use.

## Control what post types are added

By default only pages and posts are added, but you can remove either of those and/or add more using this filter:

```php
/**
 * Add a post type to the XML sitemap
 *
 * Takes the default array('post','page') and adds 'news' and 'event' post types to it. Returns: array('post','page','news','event')
 *
 * @param	array	$post_types	List of post types to be added to the XML Sitemap
 * @return	array	$post_types	Modified list of post types
 */
add_filter( 'sewn_seo/post_types', 'custom_sitemap_post_types' );
function custom_sitemap_post_types( $post_types )
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
 * The result is to remove 'post' and 'page' post types and to add 'news' and 'event' post types
 *
 * @param	array	$post_types	List of post types to be added to the XML Sitemap
 * @return	array	$post_types	Modified list of post types
 */
add_filter( 'sewn_seo/post_types', 'custom_sitemap_post_types' );
function custom_sitemap_post_types( $post_types )
{
	$post_types = array('news','event');
	return $post_types;
}
```

## Remove a specific post from the sitemap

A checkbox is added to each post type that is included in the sitemap. Checking it will remove that specific item from the sitemap.

This checkbox also removes posts from wp_list_pages, you can turn that off using this filter:

```php
add_filter( 'sewn_xml_sitemap/wp_list_pages', '__return_false' );
```

## Customize WordPress SEO plugin

This works with the our simple SEO plugin. When installed, the XML sitemap checkbox integrates with the SEO fields and this plugin will use the SEO post types. The goal is to keep things very simple and integrated.

## Separate Post Types from SEO plugin

This plugin will also use the SEO post_types filter to keep them the same, you can use that filter (from the examples above) as a base to maintain flexibility and to set post types for both plugins at once. If you decide you would like them to be different, you can change the XML Sitemap post types using this filter:

```php
add_filter( 'sewn_xml_sitemap/post_types', 'custom_sitemap_post_types' );
```

Just keep in mind that you are now filtering the `sewn_seo/post_types` if it is set, not the default. So this can be used to add or remove from the sewn_seo filter to customize as needed. If you are not using the seo filter, then this will be filter the default: `array('post','page')`. We generally recommend just using the `sewn_seo/post_types` filter.