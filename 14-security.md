# Security

Security is not a feature added at the end; it is a baseline requirement baked into every technical decision. A single vulnerability can expose user data, cause regulatory fines, and permanently damage trust. The majority of breaches exploit known, preventable vulnerabilities. This chapter covers the defences that a professional web application implements by default.

## HTTP security headers

Browsers are permissive by default. Without explicit security headers, they will execute scripts from any domain, allow pages to be embedded in foreign iframes, and downgrade HTTPS connections. Security headers flip this model: restrict everything, then allow only what is explicitly needed. They cost nothing to configure and defend against entire attack classes.

Set these on every response from your origin server.

### Minimal header set

```
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'; upgrade-insecure-requests;
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=()
```

With Express and Helmet:

```js path=null start=null
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:", "https:"],
      fontSrc: ["'self'"],
      connectSrc: ["'self'"],
      frameAncestors: ["'none'"],
      baseUri: ["'self'"],
      formAction: ["'self'"],
      upgradeInsecureRequests: [],
    },
  },
  hsts: { maxAge: 63072000, includeSubDomains: true, preload: true },
  referrerPolicy: { policy: 'strict-origin-when-cross-origin' },
}));
```

### Content Security Policy (CSP)

CSP is the most powerful defence against Cross-Site Scripting. It defines an allowlist of content sources for every resource type. Any resource not explicitly permitted is blocked.

**Key directives:**

| Directive | Controls |
| --- | --- |
| `default-src` | Fallback for all resource types |
| `script-src` | JavaScript sources — most critical for XSS |
| `style-src` | CSS sources |
| `img-src` | Image sources |
| `connect-src` | fetch, XHR, WebSocket, EventSource destinations |
| `font-src` | Web font sources |
| `frame-ancestors` | Who can embed your page (replaces X-Frame-Options) |
| `base-uri` | Restricts `<base>` tag (prevents base tag hijacking) |
| `form-action` | Restricts form submission targets |

**Avoid `'unsafe-inline'` for scripts** — it negates most of CSP's XSS protection. For SSR applications with inline scripts, use nonces:

```js path=null start=null
import crypto from 'crypto';

app.use((req, res, next) => {
  const nonce = crypto.randomBytes(16).toString('base64');
  res.locals.nonce = nonce;

  res.setHeader('Content-Security-Policy',
    `default-src 'self'; ` +
    `script-src 'self' 'nonce-${nonce}'; ` +
    `style-src 'self' 'nonce-${nonce}'; ` +
    `img-src 'self' data: https:; ` +
    `connect-src 'self'; ` +
    `frame-ancestors 'none';`
  );
  next();
});
```

```html path=null start=null
<!-- Only scripts with the matching nonce will execute -->
<script nonce="<%= nonce %>">initializeApp();</script>
```

**Deployment workflow:**

1. Start with `Content-Security-Policy-Report-Only` — logs violations without blocking. Monitor for one to two weeks.
2. Adjust the policy to cover all legitimate resources.
3. Switch to enforcing `Content-Security-Policy`.

### HTTP Strict Transport Security (HSTS)

HSTS tells browsers to only connect over HTTPS, even if the user types `http://`. This prevents SSL stripping attacks.

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

- `max-age=63072000` — two years in seconds.
- `includeSubDomains` — apply to all subdomains. Only add this if every subdomain supports HTTPS.
- `preload` — signals intent to join the browser HSTS preload list. Once preloaded, HTTPS is enforced before the first visit.

Start with `max-age=300` during testing, increase to two years once verified.

### X-Content-Type-Options

Prevents MIME type sniffing — browsers guessing content type from file content rather than the `Content-Type` header. An uploaded `.jpg` that contains JavaScript could execute if the browser decides to interpret it as a script.

```
X-Content-Type-Options: nosniff
```

### X-Frame-Options

Prevents clickjacking by blocking your page from being embedded in an iframe on another domain.

```
X-Frame-Options: DENY
```

`DENY` blocks all framing. Use `SAMEORIGIN` if your own pages legitimately iframe each other. For modern browsers, prefer `frame-ancestors 'none'` in CSP — it is more flexible and overrides `X-Frame-Options`.

### Referrer-Policy

Controls how much URL information is included in the `Referer` header on outgoing requests:

```
Referrer-Policy: strict-origin-when-cross-origin
```

This sends the full URL for same-origin requests, and only the origin for cross-origin requests (never the path and query, which may contain sensitive data).

### Permissions-Policy

Restricts which browser APIs your page can access. Any API not explicitly permitted is denied, even if an injected script requests it:

```
Permissions-Policy: camera=(), microphone=(), geolocation=(), payment=(), usb=()
```

Empty parentheses deny the feature entirely. Use `(self)` to allow only your own origin.

## OWASP Top 10 (2026)

The most critical web application security risks:

1. **Broken Access Control** — users acting outside their permissions.
2. **Cryptographic Failures** — sensitive data exposed via weak or missing encryption.
3. **Injection** — untrusted data sent to an interpreter (SQL, XSS, OS commands).
4. **Insecure Design** — missing security controls in the architecture itself.
5. **Security Misconfiguration** — default credentials, unnecessary features, verbose errors.
6. **Vulnerable Components** — libraries and frameworks with known vulnerabilities.
7. **Authentication Failures** — broken login, session management, or identity verification.
8. **Data Integrity Failures** — unverified software updates, insecure CI/CD pipelines.
9. **Logging and Monitoring Failures** — insufficient logging makes breaches undetectable.
10. **SSRF** — server fetches a URL supplied by the attacker, accessing internal systems.

## Cross-Site Scripting (XSS) prevention

XSS remains the most common web vulnerability. An attacker injects malicious scripts that execute in users' browsers with access to their sessions, cookies, and data.

### Principle: never insert untrusted data without encoding

Modern frameworks (React JSX, Vue templates, Angular templates, Svelte) auto-escape output. Use the framework's native rendering — never set raw HTML from untrusted content:

```jsx path=null start=null
// ✅ Safe — React escapes by default
function Comment({ text }) {
  return <p>{text}</p>;
}

// ❌ Dangerous — arbitrary HTML executes
function Comment({ html }) {
  return <p dangerouslySetInnerHTML={{ __html: html }} />;
}
```

When you must render user HTML (rich-text editors, CMS content), sanitize with DOMPurify before rendering:

```js path=null start=null
import DOMPurify from 'dompurify';

const clean = DOMPurify.sanitize(userProvidedHTML);
element.innerHTML = clean;
```

Never use `eval()`, the `Function` constructor, or `setTimeout(string)` with untrusted input.

### Trusted Types

The Trusted Types API enforces sanitization of data passed into dangerous DOM sinks:

```
Content-Security-Policy: require-trusted-types-for 'script'; trusted-types default dompurify;
```

This causes the browser to reject any attempt to assign raw strings to `innerHTML`, `outerHTML`, or `script.src`. Production applications with strict security requirements should adopt it.

## CSRF protection

Cross-Site Request Forgery exploits the browser's automatic inclusion of cookies to perform unauthorized actions on behalf of authenticated users.

### SameSite cookies (primary defence)

```js path=null start=null
res.cookie('session', sessionId, {
  httpOnly: true,
  secure: true,
  sameSite: 'Lax',  // or 'Strict' for maximum protection
  maxAge: 3_600_000,
});
```

- `SameSite: Strict` — cookie not sent with any cross-site request. Maximum protection, but breaks some legitimate cross-site navigation.
- `SameSite: Lax` — cookie sent with top-level navigations (clicking a link) but not with embedded requests (img, iframe). Good default.
- `SameSite: None` — sent with all requests; requires `Secure`.

### CSRF tokens (for forms)

```html path=null start=null
<form method="POST" action="/transfer">
  <input type="hidden" name="csrf_token" value="<%= csrfToken %>" />
  <input type="text" name="amount" />
  <button type="submit">Transfer</button>
</form>
```

```js path=null start=null
// Server: generate a token tied to the session
// Validate on every state-changing request
if (req.body.csrf_token !== req.session.csrfToken) {
  return res.status(403).json({ error: 'CSRF token mismatch' });
}
```

## Input validation

Client-side validation is a UX feature, not a security control. Always validate on the server.

```ts path=null start=null
import { z } from 'zod';

const signupSchema = z.object({
  email: z.string().email('Invalid email format'),
  username: z
    .string()
    .min(3)
    .max(20)
    .regex(/^[a-zA-Z0-9_]+$/, 'Only letters, numbers, and underscores'),
  password: z
    .string()
    .min(8)
    .regex(/[A-Z]/, 'Needs an uppercase letter')
    .regex(/[0-9]/, 'Needs a number'),
});

function validateSignup(data: unknown) {
  const result = signupSchema.safeParse(data);
  if (!result.success) {
    return { error: result.error.flatten() };
  }
  return { data: result.data };
}
```

Use Zod (or Valibot) to define schemas once and derive both TypeScript types and runtime validation from them. Never trust client-side validation alone.

### SQL injection

Use parameterized queries or an ORM. Never concatenate user input into SQL strings:

```js path=null start=null
// ❌ Never do this
const query = `SELECT * FROM users WHERE email = '${userInput}'`;

// ✅ Parameterized query (pg driver)
const result = await pool.query(
  'SELECT * FROM users WHERE email = $1',
  [userInput]
);

// ✅ ORM (Drizzle) — parameterized automatically
const user = await db.query.users.findFirst({
  where: eq(users.email, userInput),
});
```

## Authentication

### Session cookies

```js path=null start=null
res.cookie('session', sessionToken, {
  httpOnly: true,  // inaccessible to JavaScript — prevents XSS cookie theft
  secure: true,    // HTTPS only
  sameSite: 'Lax', // CSRF protection
  maxAge: 7 * 24 * 60 * 60 * 1000, // 1 week
});
```

Never store session tokens or JWTs in `localStorage` — it is accessible to any JavaScript on the page and vulnerable to XSS.

### Password storage

Use Argon2id (preferred) or bcrypt. Never MD5, SHA-1, or unsalted SHA-256:

```js path=null start=null
import argon2 from 'argon2';

// Hash on registration
const hash = await argon2.hash(password, { type: argon2.argon2id });

// Verify on login
const valid = await argon2.verify(hash, submittedPassword);
```

For bcrypt, use a work factor of at least 12 (targeting ~250 ms on current hardware):

```js path=null start=null
import bcrypt from 'bcrypt';

const hash = await bcrypt.hash(password, 12);
const valid = await bcrypt.compare(submittedPassword, hash);
```

### Prefer platform auth

Rolling custom authentication is a common source of vulnerabilities. Prefer managed auth services (Clerk, Auth.js, Supabase Auth, Firebase Auth) that handle token rotation, session management, MFA, and account recovery correctly. Reserve custom auth for constrained environments where managed services are unavailable.

### OAuth / OIDC

When integrating with third-party providers (Google, GitHub):
- Use the Authorization Code flow with PKCE, not the implicit flow.
- Validate the `state` parameter to prevent CSRF.
- Verify the `id_token` signature and `aud` claim.
- Use short-lived access tokens with refresh token rotation.

## CORS (Cross-Origin Resource Sharing)

CORS is not a security mechanism you add — it is a relaxation of the browser's default Same-Origin Policy. Misconfigured CORS is a common vulnerability.

```js path=null start=null
// ❌ Never wildcard when credentials are involved
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true  // This combination is invalid and rejected by browsers

// ✅ Whitelist specific origins
const allowedOrigins = new Set([
  'https://app.example.com',
  'https://admin.example.com',
]);

app.use((req, res, next) => {
  const origin = req.headers.origin;
  if (origin && allowedOrigins.has(origin)) {
    res.setHeader('Access-Control-Allow-Origin', origin);
    res.setHeader('Access-Control-Allow-Credentials', 'true');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type, Authorization');
    res.setHeader('Access-Control-Max-Age', '86400');
  }
  if (req.method === 'OPTIONS') return res.sendStatus(204);
  next();
});
```

Never blindly reflect the incoming `Origin` header without validation — it is equivalent to `*` with credentials.

## File upload security

File uploads are a common attack vector. Validate on both client and server:

```js path=null start=null
import fileType from 'file-type';

async function validateUpload(file) {
  // 1. Check MIME type from actual bytes (not the file extension)
  const detected = await fileType.fromBuffer(file.buffer);
  const allowed = ['image/jpeg', 'image/png', 'image/webp', 'application/pdf'];

  if (!detected || !allowed.includes(detected.mime)) {
    throw new Error('Invalid file type');
  }

  // 2. Size limit
  if (file.size > 10 * 1024 * 1024) {
    throw new Error('File exceeds 10 MB limit');
  }

  // 3. Store with a sanitized, random filename — never use the original
  const ext = detected.ext;
  const safeName = `${crypto.randomUUID()}.${ext}`;

  return safeName;
}
```

Never:
- Trust the file extension or the client-provided MIME type.
- Serve uploaded files from the same origin as the application (use a separate domain or CDN).
- Execute or include uploaded file content server-side.

## Dependency security

Vulnerabilities in third-party packages are a primary attack vector (OWASP #6):

```bash path=null start=null
# Audit for known vulnerabilities
npm audit

# Automatically fix non-breaking vulnerabilities
npm audit fix

# Check dependencies weekly in CI
npx update-browserslist-db@latest
```

- Enable Dependabot (GitHub) or Renovate for automated dependency updates.
- Pin exact versions in lock files and commit them.
- Audit the dependency tree quarterly — remove unused packages aggressively.
- Never install packages with `--ignore-scripts` vulnerabilities.

## Secret management

Never commit secrets to source control:

```bash path=null start=null
# .gitignore
.env
.env.local
.env.*.local
```

Load secrets at runtime from environment variables or a secrets manager:

```js path=null start=null
// ✅ Read from environment
const apiKey = process.env.STRIPE_SECRET_KEY;

// ❌ Never
const apiKey = 'sk_live_abc123...';
```

Rotate secrets on a schedule. If a secret is accidentally committed, rotate it immediately — consider the committed version permanently compromised regardless of whether it was in git history only briefly.

## HTTPS and TLS

- Serve everything over HTTPS. Use Let's Encrypt for free certificates with auto-renewal.
- Redirect all HTTP traffic to HTTPS.
- Enable TLS 1.3 (or minimum 1.2). Disable 1.0 and 1.1.
- Enable OCSP stapling for faster certificate validation.
- Enable HTTP/2 (or HTTP/3) — HTTPS is required anyway.

```nginx path=null start=null
server {
  listen 80;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl http2;
  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_prefer_server_ciphers on;
  ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-CHACHA20-POLY1305';
  add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
}
```

## Security headers audit

Test your headers using `securityheaders.com`. The target is a grade of A. Run this check on every significant release.

```bash path=null start=null
# Quick local check
curl -I https://yoursite.com | grep -i "content-security\|x-frame\|strict-transport\|referrer\|permissions\|x-content"
```

## Logging and monitoring

Security events must be logged to detect and respond to breaches:

- Log authentication attempts (success and failure) with IP and timestamp.
- Log authorization failures.
- Log input validation failures (may indicate probing).
- Never log passwords, tokens, credit card numbers, or PII.
- Alert on anomalous authentication patterns (brute force, credential stuffing).
- Retain logs for a minimum of 90 days (compliance requirements vary).

## The security header checklist

Before launch, verify every item:

- CSP deployed in enforcement mode (not report-only).
- HSTS with at least one year `max-age` and `includeSubDomains`.
- `X-Content-Type-Options: nosniff` on every response.
- `Referrer-Policy: strict-origin-when-cross-origin`.
- `Permissions-Policy` restricts unused browser APIs.
- Session cookies use `HttpOnly`, `Secure`, and `SameSite`.
- No secrets in source code or client-side bundles.
- `npm audit` reports zero high or critical vulnerabilities.
- File uploads validated server-side with `file-type` or equivalent.
- HTTPS enforced with valid certificate and auto-renewal.
- Error responses reveal no stack traces, file paths, or version numbers.
