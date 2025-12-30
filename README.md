# MenuScan

An advanced, single-file restaurant menu viewer with offline-first architecture, immersive card-based UI, and live sync capabilities.

## Features

### 🚀 Offline-First Architecture
- **First visit**: Fetches `menu.md` from the same origin (GitHub Pages compatible)
- **Instant loading**: Subsequent visits load from IndexedDB with zero network blocking
- **Silent sync**: Automatically checks for menu updates in the background and syncs changes without page reload
- **Works offline**: Full functionality after initial load, even without internet

### 🎨 Immersive UI
- **Horizontal snap-scroll**: Swipe through dishes like a filmstrip, each card fills the viewport
- **Ken Burns effect**: Subtle zoom and pan animation on snapped cards for cinematic feel
- **3D card flip**: Tap any card to flip and reveal detailed description, emoji tags, and calorie visualization
- **Dynamic color palette**: Each card automatically extracts colors from dish images for unique styling
- **Smooth animations**: GPU-accelerated CSS transforms with reduce-motion support

### ⭐ Interactive Features
- **Double-tap to favorite**: Add stars to your favorite dishes
- **Live counter**: Header displays total star count across all dishes
- **5-star unlock**: Reach 5 favorites to reveal hidden "Chef's Hack" Easter eggs
- **Keyboard navigation**: Arrow keys to scroll, Enter to flip, F to favorite
- **Persistent state**: Favorites and color palettes saved to IndexedDB

### 📱 Technical Excellence
- **Single file**: Zero dependencies, no build step, deploy anywhere
- **13.5 KB**: Ultra-lightweight (< 30 KB requirement)
- **Accessible**: ARIA labels, keyboard navigation, WCAG AA color contrast
- **Performance**: Lazy loading, IntersectionObserver, passive event listeners
- **GitHub Pages ready**: Works out of the box when deployed

## Quick Start

### Deployment to GitHub Pages

1. Enable GitHub Pages in your repository settings (Settings → Pages → Source: main branch)
2. Access your menu at: `https://yourusername.github.io/yourrepo/menu.html`

### Local Development

```bash
# Start a local server
python3 -m http.server 8080

# Open in browser
open http://localhost:8080/menu.html
```

### Updating the Menu

Edit `menu.md` following this format:

```markdown
# Restaurant Menu

## Dish Name | $Price | Description with emoji 🍕 | https://images.unsplash.com/photo-...
```

**Required fields per line:**
- `##` - Line must start with double hash
- `Dish Name` - Name of the dish
- `$Price` - Price with dollar sign
- `Description` - Text description (emojis will be extracted and displayed separately)
- `Image URL` - Full HTTPS URL to dish image (Unsplash recommended)

Push changes to your repository, and visitors will automatically see updates on their next visit.

## Menu Format

Each dish entry follows this structure:

```
## [Name] | [Price] | [Description] | [Image URL]
```

**Example:**
```
## Margherita Pizza | $12 | Fresh mozzarella, basil, tomato sauce 🍕 | https://images.unsplash.com/photo-1604068549290-dea0e4a305ca?w=1200
```

## Architecture

### Data Flow
1. **Initial Load**: Fetch menu.md → Parse markdown → Store in IndexedDB → Render UI
2. **Subsequent Loads**: Load from IndexedDB → Render UI → Silent background sync
3. **Background Sync**: Fetch menu.md → Compare ETag/Last-Modified → Update if changed

### Storage Schema
- **Database**: `MenuDB`
- **Object Store**: `dishes` (keyPath: `id`)
  - Fields: `id`, `name`, `price`, `description`, `emoji`, `imageUrl`, `favorites`, `color`
- **Object Store**: `meta` (keyPath: `key`)
  - Stores: `etag` for cache validation

### Color Extraction
- Samples 5 strategic pixels from each image (corners + center)
- Averages RGB values and adjusts brightness
- Applies unique color to card accents, borders, and backgrounds
- Pre-computed on image load and cached in IndexedDB

## Browser Support

- **Modern browsers**: Chrome 80+, Firefox 75+, Safari 13+, Edge 80+
- **Required APIs**: IndexedDB, IntersectionObserver, CSS Scroll Snap, Canvas 2D
- **Mobile**: Full touch support, optimized for iOS Safari and Android Chrome

## Performance

- **File size**: 13.5 KB (uncompressed)
- **First paint**: < 1s (after menu.md fetch)
- **Subsequent loads**: < 100ms (from IndexedDB)
- **Image lazy loading**: Off-screen images loaded on-demand
- **Smooth 60fps**: GPU-accelerated animations

## Accessibility

- ✅ Keyboard navigation (Arrow keys, Enter, F)
- ✅ ARIA labels and live regions
- ✅ Screen reader support
- ✅ Reduced motion support
- ✅ Touch and click support
- ✅ Semantic HTML

## Customization

### Change Menu Source
Edit line 51 in `menu.html`:
```javascript
const APP={db:null,dishes:[],menuUrl:'https://your-custom-url.com/menu.md',lastEtag:null};
```

### Adjust Animation Speed
Edit line 19 in `menu.html`:
```css
.dish-img{transition:transform 20s ease-out} /* Ken Burns duration */
.card-inner{transition:transform .6s cubic-bezier(.4,0,.2,1)} /* Flip speed */
```

### Modify Color Palette
Edit the `extractPalette()` function (line 145) to change sampling strategy.

## License

MIT License - Feel free to use, modify, and distribute.

## Credits

- Built with vanilla JavaScript, CSS, and HTML
- Images from Unsplash (example URLs)
- No external dependencies or frameworks
