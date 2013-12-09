### Barrel Development Best Practices

Shopify Best Practices
----------------------

###Tools###

- There is a Shopify Desktop Editor app that can be useful if you're using a git and/or a compiler. When you use the app, your saved files get pushed directly to the server. **However, there may be issues with OSX Mavericks.**

###Template Conditionals###

- Class or ID your body tag based on the template shown (**template-product, template-collection, template-page**, etc.)
- Use template conditionals to change the page title and SEO/OG tags in the theme header:

```
{% if template contains "product" %}
	<meta property="og:title" content="{{ product.title }}" />
	<meta property="og:type" content="product" />
	<meta property="og:url" content="{{ shop.url }}{{ product.url }}" />
	<meta property="og:image" content="{{ product.featured_image | product_img_url: 'original' }}" />
{% else %}
	<meta property="og:title" content="{{ shop.name }}" />
	<meta property="og:url" content="{{ shop.url }}" />
	<meta property="og:image" content="{{ 'logo.png' | asset_url }}" />
{% endif %}
	
```


###Recycle Product "add to cart" code###

- If you anticipate having multiple add to cart buttons on a page (e.g. a "Quick View" or "Quick Add" button on the collections, related products, recently viewed, or search templates), you'll want your add to cart elements to use classes, not IDs. You'll also probably want to dump this code in a snippet of some kind. 


###Displaying Product Prices###

- Wrap any instance of your shop's price in some kind of classed tag -- chances are that its style or value will change, based on product variant options, sale prices, or other factors.
- Additionally, don't forget all types of price displays (sale prices with "compare at" values, prices with ranges based on variants)

###Including Price Scripts###

- Don't forget to use Shopify-hosted js assets (option_selection.js, selectCallback function) so that product option changes also reflect price changes properly on the front end.



```
{{ 'option_selection.js' | shopify_asset_url | script_tag }}
```

```
< script type="text/javascript">
 
   $(function() {
     var selectCallback = function(variant, selector) {
       $product = $('#product-' + selector.product.id);
 		
       if (variant && variant.available == true) {
         ...
       } else {
         ...
       }
     };
   });
 
</script >
```

###Uploaded Assets###
- When linking to uploaded assets, make sure to use Shopify's asset syntax (which links to the Shopify CDN-hosted version of the file):

```
{{ 'icons.png' | asset_url }}
{{ 'init.js' | asset_url | script_tag }}
{{ 'style.css' | asset_url | stylesheet_tag }}
```
**Be careful of Symbolset or other self-hosted font-face files, however -- the CDN files have ?123 versioning added, which doesn't always play well with fonts.*

- If you're using any liquid code in your js or css files -- useful when implementing user Theme Settings -- you'll need to append **.liquid** to the file name.

```
{{ style.css.liquid }}

< style type="text/css">
	body.index {
    	background: {{ settings.body_bg_color }}; 
	}
</style>

```
###Site Search###

- Most likely, you'll want to limit search results to products by adding a hidden field to your search form:

```
<form method="get" action="/search">
	<input type="hidden" name="type" value="product"/>
	<input type="text" name="q" placeholder=" Type to search" class="search_query" /> 
	<button type="submit">Submit</button>
</form>
```

###Checkout Pages###

- Because Shopify's checkout pages are hosted elsewhere (**checkout.shopify.com**), content on that page is not editable. It can only be styled (or rather, re-styled) by reading a theme's **checkout.css** file. In this file, you'll need to override the default checkout styles with ones closer to your theme's appearance.

###Secret collections template###

- By default, every Shopify store has a front-end template for **storename.com/collections/** which lists all of the collections in the store -- but weirdly enough there is no .liquid template for it. 
- If you want to call it in the template, you do something like this in your layout file (where list-collections is a **snippet**, not a template): 

```
{% if template == 'list-collections' %}
	{% include 'collection-listing' %}
{% else %}
	{{ content_for_layout }}
{% endif %}
```
