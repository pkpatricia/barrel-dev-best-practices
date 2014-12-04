### Barrel Development Best Practices

# User Experience
 
## DOM Visibility
DOM should always be usable without javascript; that is, javascript should be treated as an enhancements. What the user sees, should be immediately accessible.

## Image Loading

- Use inline javascript and css for loading.
- Always wrap in wrapper div.
- Maintain aspect ratio.
- Loading animation.
- Use display: none on images.
- Lazy loading using javascript to throttle load.

## Image Sizes

- Use jpeg for photographs.
- Use pngs and/or svg (IDs) for sprites (one sheet).
- Force WP to serve appropriate sizes, never original.

## History

- Listen to state change to trigger changes rather than triggering changes with click. This keeps track of changes with back/forward buttons.

## First Hop (assets)

- Check final minified sizes.
- Keep under 64kb (when possible).
- Use webfont loader script to help manage font loading even with local fonts.
- Shrink fonts by removing unnecessary character sets.
- For icon fonts like FontAwesome, pull only the glyphs you need.
- Look at the size of your vendor scripts and use the smallest possible version.