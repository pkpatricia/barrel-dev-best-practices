### Barrel Development Best Practices

# User Experience
 
## DOM Visibility

*Please elaborate!*

- Content should be immediately accessible.
- DOM should always be usable without javascript.
- Javascript should be treated as an enhancement.

## Image Loading

*Add examples!*

- Use inline javascript and css for loading.
- Always put images in a wrapper div (allows for loading animations, forcing aspect ratios).
- Option: Load from data-src and or src-set (when serving size based off media-width).
- Option: Hide image and then move content into wrapper background.
- Lazy loading using javascript to throttle number of images.

## Image Sizes

*Elaborate on size benefits. Best practices for shrinking images and goal image sizes! (Under 200k? Why?)*

- Use jpeg for photographs.
- Use pngs and/or svg for sprites (keep on a single sheet, SVG id helps make this possible).
- Force WP to serve appropriate sizes, never original.

## History

*Add examples!*

- Listen to state change to trigger changes rather than triggering changes with click. This keeps track of changes with back/forward buttons.

## First Hop (Assets)

*Elaborate and explain the reasons behind each of these. Examples should be shown. Call out expensive JS libraries.*

- Check final minified sizes.
- Keep under 64kb (when possible).
- Use webfont loader script to help manage font loading even with local fonts.
- Shrink fonts by removing unnecessary character sets.
- For icon fonts like FontAwesome, pull only the glyphs you need, create new icon font.
- Look at the size of your vendor scripts and use the smallest possible version.
