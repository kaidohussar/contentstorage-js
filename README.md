# @contentstorage/js

Runtime JS SDK for Contentstorage with support for advanced features including variations, images, and A/B testing.

## Overview

This package provides runtime functions for accessing content in your application with advanced features like:
- **Variations** - A/B testing and content variations
- **Images** - Image management with CDN URLs
- **Live Editor** - Real-time content editing in development
- **Runtime Fetching** - Fetch content dynamically at runtime
- **Type Safety** - Full TypeScript support with auto-completion

## Installation

```bash
npm install @contentstorage/js
```

## Basic Usage

### Initialize the SDK

```typescript
import { initContentStorage, fetchContent, getText } from '@contentstorage/js';

// Initialize with your config
initContentStorage({
  contentKey: 'your-content-key',
  languageCodes: ['EN', 'FR', 'DE']
});

// Fetch content for a language
await fetchContent('EN');

// Get text content
const { text } = getText('HomePage.title');
console.log(text); // "Welcome to our website"
```

### With Variables

```typescript
const { text } = getText('HomePage.greeting', { name: 'John' });
console.log(text); // "Hello, John!"
```

### Using Images

```typescript
import { getImage } from '@contentstorage/js';

const { data } = getImage('HomePage.heroImage');
console.log(data.url); // "https://cdn.contentstorage.app/..."
console.log(data.altText); // "Hero image"
```

### Using Variations (A/B Testing)

```typescript
import { getVariation } from '@contentstorage/js';

const { text } = getVariation('HomePage.ctaButton', 'variantB');
console.log(text); // "Get Started Now" (variant B text)

// Falls back to 'default' if variant not found
const { text: defaultText } = getVariation('HomePage.ctaButton', 'nonexistent');
console.log(defaultText); // "Sign Up" (default text)
```

## API Reference

### `initContentStorage(config)`

Initializes the SDK with your configuration.

**Parameters:**
- `config.contentKey` (string) - Your ContentStorage content key
- `config.languageCodes` (string[]) - Array of language codes to support
- `config.pendingChanges` (boolean, optional) - Enable pending changes API

### `fetchContent(language?, options?)`

Fetches content from ContentStorage CDN or API.

**Parameters:**
- `language` (string, optional) - Language code to fetch
- `options.withPendingChanges` (boolean, optional) - Fetch pending/draft changes
- `options.contentKey` (string, optional) - Override content key

### `getText(contentId, variables?)`

Retrieves text content with optional variable substitution.

**Returns:** `{ contentId: string, text: string }`

### `getImage(contentId)`

Retrieves image data including URL and alt text.

**Returns:** `{ contentId: string, data: { url: string, altText: string } }`

### `getVariation(contentId, variationKey?, variables?)`

Retrieves variation content for A/B testing.

**Returns:** `{ contentId: string, text: string }`

### `setContentLanguage({ languageCode, contentJson })`

Manually set content for a specific language.

### `liveEditorReady(retries?, delay?)`

Initializes the live editor (automatically called in iframe context).

## TypeScript Support

Augment the `ContentStructure` interface for type-safe content IDs:

```typescript
declare module '@contentstorage/js' {
  interface ContentStructure {
    HomePage: {
      title: string;
      greeting: { text: string; variables: { name: string } };
      heroImage: ImageObject;
      ctaButton: VariationObject;
    };
  }
}
```

Now you get autocomplete and type checking:

```typescript
getText('HomePage.title'); // ✅ Valid
getText('InvalidPage.title'); // ❌ TypeScript error
```

## When to Use This SDK

Use this SDK when you need:
- **Variations** - Test different content variations
- **Image Management** - Centralized image hosting and management
- **Live Editor** - Real-time content editing during development
- **Runtime Fetching** - Dynamic content loading based on user actions

For simple i18n needs, consider using popular i18n libraries with ContentStorage CLI:
- `@contentstorage/plugin-i18next` - i18next integration
- `@contentstorage/plugin-react-intl` - react-intl integration
- `@contentstorage/plugin-vue-i18n` - Vue i18n integration

## License

MIT

## Links

- [ContentStorage Website](https://contentstorage.app)
- [Documentation](https://contentstorage.app/docs)
- [GitHub](https://github.com/kaidohussar/contentstorage-sdk)
