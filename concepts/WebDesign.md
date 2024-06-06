# Web Design

## Process

1. Stategy session (with client)
2. Sitemap (if scope is known)
3. Wireframe (if scope is known)
4. Moodboard
5. UI Design
   - Establish Systems
   - Design iterations

## Strategy session with client

What problem are we trying to solve with this website?

Site type
1. E-commerce
2. Marketing (ad)
    - Variant: Portfolio
3. Content (magazine)
    - Variant: Educational
4. App (complex)

Site goal
1. Selling products
2. Building trust
3. Generating leads (reaching people with the objective of eventually turning them into customers)
4. Providing value for the end user

Project Brief
- Overview / Context
- Description / Summary
- Target Audience
- Scope (what will the site include)
- Desired attributes / values / emotions. Personality.

Reference websites (align expectations between client and designer)

## Sitemap

Type
1. Pages
   - Home
   - About
   - Product
   - Contact
   - ...
2. Sections
   - Navigation (header / sidebar)
   - Hero Section
   - Featured
   - Call to action (CTA)
   - Testimonials
   - Pricing
   - Gallery
   - Contact
   - ...
   - Footer

Sometimes it may be better to start with a single feature, and worry about the navigation and structure afterwards, when more of the problem is known.


## Wireframe

When the overall scope is known.
- Very low fidelity design / layout.
- No color (shades of gray).
- Placeholder images (gray squares).
- Final/meaningful text where possible (avoid lorem ipsum).


## Moodboard

1. Collect reference screenshots and inspiration photos.
2. Curate the references that speak most to you and identify possible overarching concepts.
3. Create a moodboard with the curated selection.


## UI Design

- Iterative design (design-code-design).
  - Don't include nice-to-have features in the beginning.
- Avoid color in the beginning (only shades of gray).
  - Forces you to use spacing, contrast and size, improving hierarchy.

## Systems

Systems are collections of predefined values that are reused throughout the site, to avoid creating a new value each time.

### Spacing and sizing

Spacing and sizing systems should be exponential (not linear).

Example system having 16px as a base value (because 16 is the default base font-size in browsers):
- 4px (0.25rem)
- 8px (0.5rem)
- 12px (0.75rem)
- 16px (1rem)
- 24px (1.5rem)
- 32px (2rem)
- 48px (3rem)
- 64px (4rem)
- 96px (6rem)
- 128px (8rem)
- 192px (12rem)
- 256px (16rem)
- 384px (24rem)
- 512px (32rem)
- 640px (40rem)
- 768px (48rem)

### Color palette

You will need:
1. 8-10 shades of gray
   - Pick your darkest grey by choosing a color for the darkest text, and your lightest grey by choosing a subtle off-white background.
   - You can saturate the grays with a blue or yellow hue to better fit the intended warmth.
2. 5-10 shades of a primary
3. 5-10 shades of a few accent colors
4. 5-10 shades of semantic colors (green, yellow, red)

For the non-grayscale colors:
1. Start by choosing the base colors.
   - For primary/accent colors, a good rule of thumb is that they work correctly as button backgrounds.
2. Determine the lighter and darker variations of each color.
   - Think of the darker shade as text on a lighter shade background (e.g. error toast message, success/pending/failed state tag).
   - Use HSL instead of Hex values.
   - Keep the same hue but adjust saturation and lightness.
   - For lighter colors, consider adjusting hue to change the perceived lightness instead.
3. Fill in the gaps.
   - Visually find the colors mid-way between the base and the edges.
   - Repeat between any pair of colors where you still need a shade (e.g. until you have 9 shades).

### Typography

1. Typefaces
   - Choose one for headlines and another for body text.
   - Rule of thumb: filter font choosing to fonts with 5+ weights (10 styles including italics), they tend to have been crafted with care.
2. Sizes
   - Preferably based on the spacing and sizing system.
   - e.g. 12, 14 (0.875rem), 16, 18 (1.125rem), 20 (1.25rem), 24, 30 (1.875rem), 36 (2.25rem), 48, 60 (3.75rem), 72 (4.5rem)
3. Weights
   - Stick to about 2, usually normal (400/500) and bold (600/700).
4. Line-height

### Buttons

- Primary
- Secondary (outlined button)
- Tertiary (link)

### Other systems

1. Margin / padding
2. Width / Height
3. Box shadows
4. Border width / radius
5. Opacity
8. Buttons
   
9. Icons
   - Don't scale icons intended for smaller/larger sizes.
   - If needed, surround the icon in another larger shape (e.g. circle, rounded square) with a background color while keeping the icon size.
10. ...

## Hierarchy

Text
- Don't use font size only.
- Font weight (bold for important text).
  
- Color (softer color for secondary/supporting text).
  - Stick to 2 or 3.
  - Note: Don't use gray text on colored backgrounds. Lighter gray on white background for supporting text is equivalent on colored backgrounds to: same hue, different saturation/lightness.
- When you can no longer add emphasis to an element that needs it, consider de-emphasizing the others.
- Semantic hierarchy is different from Visual hierarchy
  - h1 is important for SEO but may be de-emphasized visually.
  - A *delete* button should only be big and red if it's the primary action, unobtrusive otherwise.
- Balance weight and contrast: when a darker color feels too harsh, consider keeping it lighter but heavier (e.g. 2px light separator border).

Labels
- Avoid labels when the data speaks for itself (e.g. name, age, email, phone).
- Incorporate necessary labels in the value itself (e.g. *12 left in stock* instead of *Stock: 12*).
- When you do need a label, treat it as secondary text, not primary.
  - Except for spec-dense pages, where user will actively look for the label.

Icons
- If the icon has the same color as the text, it may steal attention to it (it's more dense).
  - Consider giving it a softer color.

Spacing
- Start with too much white space.
  - When we remove the obvious excess, we arrive at a good balance.
  - When we add until it's not obviously cramped, it's still lacking space.
- Use a larger margin to separate groups than the one used to separate their internal elements.

Scaling
- Large items should scale faster for larger screens (e.g. headlines are proportionally bigger on Desktop than on Mobile).
- Button padding should scale faster than button text (small buttons need almost no padding).
  - For this reason, avoid button padding in **em**. Use a deliberate value.

Consider replacing borders with:
- Spacing
- Box-shadow
- Background-color

## Readability

- Limit paragraphs to about 8-13 words, or 45-75 characters per line (20-35em).
- Long lines need taller line-height.
- Large text size needs shorter line-height.
- Don't center-align long paragraphs.
- Right-align numbers.
- Tighten headlines letter-spacing in fonts designed for body text.
- Increase letter-spacing in all-caps text.

## Visual Design touches

Depth
- Elements on different layers
  - Overlapping elements with different z-index (e.g. containers over the edge between sections, containers taller than their parents, illustrations under/over portions of text).
  - Use shadows to convey elevation (e.g. modal dialogs, dropdown menus).
- Simulate light source from above.
  - Light top border/inset box-shadow and dark bottom border/box-shadow for raised elements (e.g. buttons).
  - Dark top border/inset box-shadow and light bottom border/box-shadow for sunken elements (e.g. inputs, checkboxes).

Texture
- Colored backgrounds.
- Gradient backgrounds.
- Patterned backgrounds.
- Full image backgrounds.
  - Tips to increase text contrast:
    1. Add an overlay / lower the image contrast.
    2. Colorize the image.
    3. Add text shadow.

Flourishes
- Link style.
- Custom checkboxes and radio buttons.
  - Or even consider using switches, toggle buttons, selectable cards.
- Custom selects.
- Replace bullets with icons if it makes sense (checkmarks, locks).
- Promote the quotes of testimonials to larger visual elements.
- Accent borders (thick border on one edge to add color to a bland element, or highlight selected tab/navigation).

Empty states
- If the app depends on user-added content, the first thing the user will see will be an empty state (gallery, list, ...).
- Make it exciting by adding an illustration and/or a call-to-action.

# Tools

- [Relume](https://www.relume.io/) - AI powered Description -> Sitemap -> Wireframe
- [Figma](https://www.figma.com/) - Interface Design Tool

# Sources

- [Refactoring UI](https://www.refactoringui.com/)
- [Flux Academy](https://www.youtube.com/@FluxAcademy)
  - [Crash course](https://www.youtube.com/watch?v=j6Ule7GXaRs)
  - [Fixing a boring design](https://www.youtube.com/watch?v=TaGO9l4EYOk)