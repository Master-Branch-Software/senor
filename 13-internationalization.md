# Internationalization

Internationalization (i18n) is the architectural work that makes a product adaptable to different languages and regions without code changes per locale. Localization (l10n) is the subsequent adaptation — translation, cultural adjustment, date/number formatting — for a specific market. Retrofitting i18n into an existing product costs 3–5× more than building it from the start. Plan for it on day one.

Over 75% of internet users do not speak English as their primary language. Companies with localized products consistently report higher international revenue, lower bounce rates, and greater user trust. These are measurable outcomes.

## i18n vs. l10n

**Internationalization** is engineering. Externalize strings, adopt logical CSS properties, support Unicode, structure code so locale-specific data is a variable, not a constant.

**Localization** is adaptation. Translation, cultural image choices, locale-aware number and date formatting, compliance with local norms (RTL languages, currency, legal text).

Without i18n, l10n is a painful, error-prone retrofit. With it, adding a new locale is a content and translation exercise, not an engineering one.

## The Intl API

The ECMAScript Internationalization API (`Intl`) is built into every modern JavaScript environment. It handles locale-sensitive formatting natively, with zero bundle cost. Use it instead of libraries for formatting tasks.

### `Intl.DateTimeFormat` — dates and times

```js path=null start=null
const date = new Date(2026, 3, 23); // April 23, 2026

// US format
new Intl.DateTimeFormat('en-US', { dateStyle: 'long' }).format(date);
// "April 23, 2026"

// German format
new Intl.DateTimeFormat('de-DE', { dateStyle: 'long' }).format(date);
// "23. April 2026"

// Japanese format with time
new Intl.DateTimeFormat('ja-JP', {
  dateStyle: 'long',
  timeStyle: 'short',
  timeZone: 'Asia/Tokyo',
}).format(date);
// "2026年4月23日 12:00"
```

Use `dateStyle` and `timeStyle` shorthand for common patterns. Use the granular options (`year`, `month`, `day`, `hour`...) when precise control is required. Never concatenate date parts manually — locales differ in order, separators, and scripts.

### `Intl.NumberFormat` — numbers and currency

```js path=null start=null
const price = 1234567.89;

new Intl.NumberFormat('en-US').format(price);          // "1,234,567.89"
new Intl.NumberFormat('de-DE').format(price);          // "1.234.567,89"
new Intl.NumberFormat('en-IN').format(price);          // "12,34,567.89"

// Currency
new Intl.NumberFormat('en-US', {
  style: 'currency', currency: 'USD',
}).format(99.95);
// "$99.95"

new Intl.NumberFormat('de-DE', {
  style: 'currency', currency: 'EUR',
}).format(99.95);
// "99,95 €"

// Units
new Intl.NumberFormat('en-GB', {
  style: 'unit', unit: 'kilometer-per-hour',
}).format(100);
// "100 km/h"

// Compact notation
new Intl.NumberFormat('en-US', {
  notation: 'compact', compactDisplay: 'short',
}).format(2_500_000);
// "2.5M"
```

Always pass the ISO 4217 currency code explicitly. Symbol position and spacing are locale-dependent — let `Intl` decide them.

### `Intl.RelativeTimeFormat` — relative time

```js path=null start=null
const rtf = new Intl.RelativeTimeFormat('en', { numeric: 'auto' });

rtf.format(-1, 'day');   // "yesterday"
rtf.format(2, 'week');   // "in 2 weeks"
rtf.format(-3, 'month'); // "3 months ago"

const rtfFr = new Intl.RelativeTimeFormat('fr', { numeric: 'auto' });
rtfFr.format(-1, 'day'); // "hier"
rtfFr.format(1, 'day');  // "demain"
```

`numeric: 'auto'` uses words like "yesterday" when they exist in the locale; `numeric: 'always'` forces numbers.

### `Intl.ListFormat` — natural language lists

```js path=null start=null
const items = ['apples', 'oranges', 'bananas'];

new Intl.ListFormat('en-US', { type: 'conjunction' }).format(items);
// "apples, oranges, and bananas"

new Intl.ListFormat('de-DE', { type: 'conjunction' }).format(items);
// "Äpfel, Orangen und Bananen"

new Intl.ListFormat('en-US', { type: 'disjunction' }).format(items);
// "apples, oranges, or bananas"
```

### `Intl.PluralRules` — pluralization

Different languages have different plural categories. English has two (one / other). Arabic has six (zero, one, two, few, many, other). Russian has four. `Intl.PluralRules` tells you which category a number falls into for a given locale:

```js path=null start=null
const pr = new Intl.PluralRules('en-US');
pr.select(1);   // "one"
pr.select(2);   // "other"
pr.select(0);   // "other"

const prAr = new Intl.PluralRules('ar');
prAr.select(0); // "zero"
prAr.select(1); // "one"
prAr.select(2); // "two"
prAr.select(5); // "few"
prAr.select(11);// "many"
```

Use `Intl.PluralRules` with ICU MessageFormat (see below) rather than writing plural logic by hand.

### `Intl.Collator` — locale-aware sorting

```js path=null start=null
const names = ['Émilie', 'Zoe', 'Elodie', 'Åsa'];

// Default sort (wrong for accented characters)
[...names].sort();
// ["Elodie", "Zoe", "Åsa", "Émilie"]  ← incorrect alphabetical order

// Locale-aware sort
const frCollator = new Intl.Collator('fr');
names.sort(frCollator.compare);
// ["Åsa", "Elodie", "Émilie", "Zoe"]  ← correct French order

// Numeric sort
const fileCollator = new Intl.Collator(undefined, { numeric: true });
['item 10', 'item 2'].sort(fileCollator.compare);
// ["item 2", "item 10"]  ← correct numeric order
```

### `Intl.Segmenter` — text segmentation

```js path=null start=null
// Word segmentation respects Unicode rules per locale
const segmenter = new Intl.Segmenter('ja-JP', { granularity: 'word' });
const text = '東京は大きな都市です。';
const words = [...segmenter.segment(text)].filter(s => s.isWordLike);
// Correctly segments Japanese text into words
```

## ICU MessageFormat: plurals and interpolation

Simple key-value string replacement breaks down with pluralization and gender. ICU MessageFormat handles these cases and is supported by all major i18n libraries.

```json path=null start=null
{
  "items": "{count, plural, =0 {No items} one {# item} other {# items}}",
  "inbox": "You have {unread, plural, =0 {no new messages} one {# new message} other {# new messages}} in {folders, plural, one {# folder} other {# folders}}.",
  "assigned": "{gender, select, male {He is assigned} female {She is assigned} other {They are assigned}}"
}
```

```js path=null start=null
t('items', { count: 0 })  // "No items"
t('items', { count: 1 })  // "1 item"
t('items', { count: 5 })  // "5 items"
```

Never concatenate translated fragments with variables: "You have " + count + " messages" is untranslatable because word order differs across languages. Interpolate within the message.

## Locale detection and routing

Detection order (most apps):

1. Saved user preference (cookie, `localStorage`, user account setting).
2. URL path segment (`/de/`, `/fr/`).
3. `Accept-Language` header (server-side) or `navigator.language` (client-side).
4. Default locale fallback.

```js path=null start=null
function detectLocale(supportedLocales, defaultLocale) {
  // 1. Saved preference
  const saved = localStorage.getItem('preferred-locale');
  if (saved && supportedLocales.includes(saved)) return saved;

  // 2. Browser language
  const browserLang = navigator.language.split('-')[0];
  if (supportedLocales.includes(browserLang)) return browserLang;

  // 3. Fallback
  return defaultLocale;
}
```

Persist the user's explicit choice. Do not silently redirect users based on IP — this confuses users on shared devices and is an accessibility issue.

### URL structure

Three patterns for multilingual URLs:

| Pattern | Example | Best for |
| --- | --- | --- |
| Subdirectory | `example.com/de/` | Most sites — unified domain authority |
| Subdomain | `de.example.com` | Separate hosting configs |
| ccTLD | `example.de` | Fully separate regional operations |

Subdirectories are the pragmatic default. They keep domain authority unified, are straightforward with modern frameworks, and simplify SSL management.

## Right-to-left (RTL) support

Arabic, Hebrew, Farsi, and Urdu read right-to-left. RTL is not a CSS afterthought — plan for it from the start.

### Use logical properties

```css path=null start=null
/* ❌ Physical properties — do not flip for RTL */
.element {
  margin-left: 1rem;
  text-align: left;
  padding-left: 2rem;
  border-left: 2px solid;
}

/* ✅ Logical properties — flip automatically with dir="rtl" */
.element {
  margin-inline-start: 1rem;
  text-align: start;
  padding-inline-start: 2rem;
  border-inline-start: 2px solid;
}
```

Logical properties are the reason `04-css-and-layout.md` recommends them universally, not just for i18n. Once you use `margin-inline-start` everywhere, adding RTL support is nearly free.

### Setting document direction

```html path=null start=null
<html lang="ar" dir="rtl">
```

```js path=null start=null
// Switch direction when locale changes
function updateDirection(locale) {
  const rtlLocales = ['ar', 'he', 'fa', 'ur'];
  document.documentElement.lang = locale;
  document.documentElement.dir = rtlLocales.includes(locale) ? 'rtl' : 'ltr';
}
```

Test with real RTL content, not placeholder text. Icons with directional meaning (arrows, back buttons, progress bars) must mirror. Images showing directional actions (writing, reading) should have RTL variants.

### Special cases for RTL

- Navigation menus flip horizontal position.
- Directional icons (arrows, chevrons) should mirror. Universal icons (close ×, trash, star) should not.
- Numbers and code remain LTR even inside RTL text.
- Use `dir="auto"` for user-generated content of unknown direction.

## Text expansion

Translated text changes length. German is routinely 30–35% longer than English. Finnish can expand further. Chinese and Japanese are often shorter. Design for this:

- Avoid fixed-width containers for text elements.
- Use `min-width` rather than `width` on buttons.
- Test every component with the longest likely locale string, not just English.
- Use `hyphens: auto` for languages that benefit from hyphenation.

## Translation workflow and file structure

External all user-facing strings from source code. Two common patterns:

**Centralized (smaller projects):**
```
locales/
  en/common.json
  en/dashboard.json
  de/common.json
  de/dashboard.json
```

**Feature-colocated (larger projects):**
```
src/features/auth/i18n/en.json
src/features/auth/i18n/de.json
src/shared/i18n/en.json
```

**Key naming conventions:**
- Hierarchical: `checkout.button.place_order` not `string_42`.
- Context-descriptive: `user.greeting` not `hello`.
- Stable: do not change a key when only copy changes — translation memory tools reuse matched keys.
- Do not use the English string as the key — it prevents changing the default without touching code.

## Framework integration

### Next.js with next-intl

```ts path=null start=null
// i18n/request.ts
import { getRequestConfig } from 'next-intl/server';

export default getRequestConfig(async ({ locale }) => ({
  messages: (await import(`../locales/${locale}.json`)).default,
}));
```

```tsx path=null start=null
// app/[locale]/page.tsx
import { useTranslations } from 'next-intl';

export default function HomePage() {
  const t = useTranslations('HomePage');
  return <h1>{t('title')}</h1>;
}
```

Type-safe key autocompletion by extending `IntlMessages`:
```ts path=null start=null
type Messages = typeof import('./locales/en.json');
declare global { interface IntlMessages extends Messages {} }
```

### React with react-i18next

```tsx path=null start=null
import { useTranslation } from 'react-i18next';

function Welcome({ name }) {
  const { t } = useTranslation();
  return <p>{t('greeting', { name })}</p>;
}
```

## hreflang: international SEO

`hreflang` tells search engines which language version of a page to serve to each audience:

```html path=null start=null
<link rel="alternate" hreflang="en"    href="https://example.com/en/product/" />
<link rel="alternate" hreflang="de"    href="https://example.com/de/product/" />
<link rel="alternate" hreflang="fr"    href="https://example.com/fr/product/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/product/" />
```

Rules:
- Every page must include a self-referencing `hreflang`.
- Implementation must be reciprocal: if page A references page B, page B must reference page A.
- Always include `x-default` for the fallback version.
- For large sites (1000+ pages), implement via XML sitemap rather than HTML `<link>` tags.
- The `lang` attribute on `<html>` must match the `hreflang` value.

Common mistakes: missing return links, incorrect language codes (use ISO 639-1 two-letter codes: `en`, `de`, `fr`), missing `x-default`, inconsistent URLs between sitemap and `hreflang`.

## Language switcher design

- Place in the header (preferred for discoverability) or footer.
- Label in the target language: show "Deutsch" not "German." Users scanning for their language should not need to read the current language to find the option.
- Use language names, not flags. Flags represent countries, not languages. Canadian French and metropolitan French speakers both use the French tricolour.
- Persist the choice across sessions.
- Do not auto-redirect on every visit — store the preference so returning users land on their chosen language.

## Performance: loading translations

Avoid bundling all locales upfront. Lazy-load the active locale:

```js path=null start=null
const loadTranslations = async (locale) => {
  try {
    const response = await fetch(`/i18n/${locale}.json`);
    return response.json();
  } catch {
    return import(`./locales/en.json`); // bundled fallback
  }
};
```

For large applications, split by namespace and load only what the current page needs:

```js path=null start=null
const loadCritical = async (locale) => {
  const [header, footer] = await Promise.all([
    import(`./locales/${locale}/header.json`),
    import(`./locales/${locale}/footer.json`),
  ]);
  return { ...header.default, ...footer.default };
};
```

## Testing i18n

### Pseudo-localization in development

Replace characters with accented equivalents to expose hardcoded strings and layout overflow before translations arrive:

```js path=null start=null
// Dev-only pseudo locale
const pseudo = Object.fromEntries(
  Object.entries(englishMessages).map(([key, value]) => [
    key,
    value.replace(/[aeiou]/g, (c) => ({ a: 'à', e: 'é', i: 'î', o: 'ô', u: 'ü' })[c] || c),
  ])
);
```

Pseudo-localization catches:
- Hardcoded strings that were never externalized.
- Containers that overflow with longer text.
- Font issues with non-Latin characters.

### Automated tests

```js path=null start=null
// Verify all locales have the same keys as the base locale
function validateTranslations(baseLocale, targetLocale) {
  const baseKeys = Object.keys(flatten(baseLocale));
  const targetKeys = new Set(Object.keys(flatten(targetLocale)));
  const missing = baseKeys.filter(k => !targetKeys.has(k));
  if (missing.length) throw new Error(`Missing keys: ${missing.join(', ')}`);
}
```

Run this check as a CI step. Nothing should ship with missing translation keys — they always silently display the key name in production.

### Manual testing

- Test with a native speaker, not machine translation, before releasing to a new market.
- Test RTL on real content with realistic lengths — short placeholder text never reveals layout problems.
- Test at 200% zoom to verify reflow works in each locale.
- Test date and number formatting in the actual locale context, not just in isolation.

## Accessibility and i18n

Language identification is a WCAG Level A requirement (1.3.2). Always set `lang` on the `<html>` element and on any inline text in a different language:

```html path=null start=null
<p>The French word for "cheese" is <span lang="fr">fromage</span>.</p>
```

Screen readers use the `lang` attribute to select the correct speech synthesizer. Without it, French words will be mispronounced by an English voice engine.

Set `aria-label` values in the correct locale language, not in the site's base language.

## The cost of delayed i18n

Adding i18n to an existing application requires: extracting hardcoded strings from every file, refactoring layouts that assume English text length, fixing date and number formatting scattered throughout the codebase, and updating test suites. Teams that delay consistently report it taking 3–5× more effort than building it from the start. Even if international expansion is not an immediate roadmap item, the architectural decisions made in the first six months determine the cost later.
