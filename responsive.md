### Barrel Development Best Practices

# Responsive Best Practices

- [Mobile Performance](#mobile-performance)
- [Retina](#retina)
- [Responsive Layout](#responsive-layout)

## Mobile Performance

*	**Visual Assets**

	When reviewing a design, evaluate each visual element and decide if an image asset is necessary.
	
	If not, the following techniques should be considered in order of priority:
	
	**1. Typographic symbols**
	
	Some basic iconography and visual elements can be accurately portrayed using system font text. For example, a circle (e.g. slideshow pager) can be portrayed using a "bullet" character (∙), and styled via 'font-size' and 'color', etc. Similarly certain arrows can be portrayed using angle quotes (‹ or ›).
	
	This method does not require the downloading of an asset, is retina friendly, and renders extremely quickly.
		
	**2. CSS**
	
	Many vector graphics, icons and logos can be constructed from pure CSS. By combining CSS triangles, circles, and quadrilaterals, and using psuedo-elements and CSS transforms we can create fairly complex vector graphics from CSS alone. 
		
	Again this method does not require the downloading of any assets, is retina friendly, and renders quickly.
		
	**3. SVG**
	
	SVG graphics should be used only when a CSS solution is not possible or would be overly verbose. SVGs can be either loaded as an &lt;img&gt;, background image, parsed from HTML, or constructed via JavaScript.
		
	This method may or may not require the downloading of any assets, and is retina friendly. Rendering performance depends on the complexity of the visual element, which must be parsed and then inserted into the DOM.
		
	**4. Raster Image**
	
	Raster images (.JPG, .PNG, .GIF) should only be used when none of the above solutions are possible (e.g. a photograph or texture). File-sizes are extremely large by comparison. This method is not retina friendly as raster images degrade as they are scaled. For retina compatibility, raster assets should be served at twice the size they would occupy in CSS pixels.
	
## Retina, Pixel-Density

"Retina" is relatively new for the web. Previously most monitors and displays output data rendered to the screen at a pixel density of 72-96ppi. This is no longer a standard in the industry as many other devices being introduced to market have vastly different pixel densities. 

*	Use SVG assets whenever possible, and include PNG fallbacks for supported legacy browsers.
*	Append the @2x to image files (png, jpg, gif, etc) and include a media query to target known retina (or high pixel density) devices. It's also possible to use a shim to retroactively parse images and replace them on the fly with a higher resolution image, but this is often javascript-based and will require a separate image asset to be requested and served on each load, which is not ideal.

## Responsive Layout

All Barrel sites are responsive unless otherwise indicated. Usage of media queries at various break points must be utilized to achieve a scalable and responsive website. More details TBD.
