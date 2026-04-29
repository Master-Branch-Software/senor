## Inline code docs

Two kinds of writing live inside source files. Docstrings are the API contract — what callers need to know to use a symbol without reading its body. Comments are notes to the next reader of this file — what the code does not already say. Conflating them produces files where every function has a paragraph restating its name and no function explains why a particular branch exists.

This chapter covers when to write each, the canonical format per language, and what to leave out. For external reference docs that are generated from these comments, see [`docs-site-structure.md`](docs-site-structure.md). For the voice rules of long-form documentation, see `copywriting/technical-writing.md`.

## Docstrings vs comments

|                        | Docstring                                                     | Comment                            |
| ---------------------- | ------------------------------------------------------------- | ---------------------------------- |
| Audience               | Callers of this symbol                                        | Readers of this file               |
| Lives on               | Module / class / function / method                            | Any line, any block                |
| Question it answers    | "How do I use this?"                                          | "Why is this written this way?"    |
| Survives refactor when | The signature is preserved                                    | The line still exists              |
| Tooling                | Extracted by doc generators (TypeDoc, Sphinx, rustdoc, godoc) | Ignored by tooling, read by humans |

A function's docstring covers the contract. A comment inside it explains the trick on line 47. Different audiences, different lifetimes, different rules. Do not collapse them.

## When to write a docstring

Write one when at least one is true:

- The symbol is part of the public API. Any caller outside the file may use it.
- The contract is non-obvious. Preconditions, postconditions, side effects, exceptions, ownership transfers.
- Behavior depends on a value the type system cannot express. "Returns `None` when the queue is empty," "raises `LookupError` only when running in strict mode."
- A reader is expected to call the symbol from search or autocomplete, without opening the file.

Skip when:

- The symbol is private and short and the name fully describes it. `def _normalize_email(addr: str) -> str:` does not need "normalizes the email."
- The symbol is a trivial wrapper that adds nothing the wrapped function does not already document.
- The docstring would only restate the function name in different words. The signature plus a useful name is the contract.

A wall of identical docstrings ("Gets the foo. Returns the foo.") trains readers to ignore docstrings everywhere. Write fewer, write them when they earn it.

## When to write a comment

Write one when at least one is true:

- The code looks wrong but is correct. Surprising loop bound, off-by-one, ordered comparisons that exploit a NaN trick, a `// fall through` in a switch.
- The code references an external constraint. Spec section, RFC clause, hardware quirk, regulatory rule, vendor bug ID, link to a GitHub issue.
- The code is a deliberate workaround. State what it works around and the condition for removing it.
- A reasonable reader would change the code and break it. The comment names the invariant they would violate.

Do not write comments that:

- Restate the code. `i = i + 1  # increment i` is noise.
- Narrate ("now we loop over the items"). The code already shows the loop.
- Mark the date or author. Git tracks both.
- Reference the current task or PR. It rots the moment the PR merges. Cite specs, issue numbers tracked in the issue tracker, or external documentation — not the in-flight branch.
- Mark "removed" or "TODO before merge." If the code is gone, delete the line. If the work is unfinished, do not merge.

A good comment changes when the comment's claim changes, not when the code changes. If a comment is obsolete the moment the next refactor lands, it was a comment about implementation, not about the unchanging _why_.

## Format per language

Each ecosystem has a canonical format. The format is the contract with the doc generator. Deviating from the format means the generated reference omits or mangles the symbol.

### Python — PEP 257 + Google or NumPy style

PEP 257 sets the rules: triple double-quoted strings, summary as a one-liner ending in a period, in imperative mood, blank line before any further detail.

```python
def normalize_email(address: str) -> str:
    """Return the address lowercased, with whitespace stripped.

    Does not validate the address. Use `validate_email` for that.

    Args:
        address: The email address as supplied by the user.

    Returns:
        The normalized form, suitable for equality comparison and
        database lookup.

    Raises:
        TypeError: If `address` is not a string.
    """
```

- Summary line is a complete sentence in the imperative mood. "Return the …" not "Returns the …" and not "This function returns the …".
- Module docstrings list public symbols in one or two sentences each.
- Class docstrings cover the role and the invariants. Constructor parameters live on `__init__`'s docstring, not the class's.
- Pick **Google** style or **NumPy** style for the structured sections. Both are recognized by Sphinx via `napoleon`. Pick one per project and stay consistent.

### TypeScript and JavaScript — TSDoc

TSDoc (microsoft/tsdoc) is the standard. JSDoc tags work where they overlap, but TSDoc resolves several JSDoc ambiguities and is what TypeDoc consumes by default.

```ts
/**
 * Normalize an email address for storage and comparison.
 *
 * @param address - The address as supplied by the user.
 * @returns The lowercased, whitespace-stripped address.
 * @throws TypeError when `address` is not a string.
 *
 * @example
 * normalizeEmail("  Alice@Example.COM ");
 * // => "alice@example.com"
 *
 * @public
 */
export function normalizeEmail(address: string): string {
```

- Do not duplicate type information that TypeScript already expresses. `@param {string}` is noise on a typed parameter.
- `@public` / `@internal` / `@beta` / `@alpha` mark API stability. Generators use them to filter what ships in public reference.
- `@deprecated` requires a replacement pointer. "Use `normalize` instead" is the contract; the bare tag is not.
- Block comments inside function bodies are JSDoc-syntax noise — use `// line comments` for in-body reasoning.

### Rust — rustdoc

`///` for outer doc comments (above an item), `//!` for inner doc comments (top of a module). Markdown body. Code blocks in doc comments are tested by `cargo test` by default — write them as runnable.

````rust
/// Normalizes an email address for storage and comparison.
///
/// Returns the lowercased, whitespace-stripped form. Does not validate
/// the address.
///
/// # Examples
///
/// ```
/// use mycrate::normalize_email;
/// assert_eq!(normalize_email("  Alice@Example.COM "), "alice@example.com");
/// ```
///
/// # Panics
///
/// Never. This function does not panic.
///
/// # Errors
///
/// Returns `Err(InvalidEmail)` if the address contains a null byte.
pub fn normalize_email(address: &str) -> Result<String, InvalidEmail> {
````

- Section headings are conventional: `# Examples`, `# Panics`, `# Errors`, `# Safety` (for `unsafe` items). rustdoc renders them with anchor links.
- Doc tests double as runnable examples. A broken example fails CI.
- Public items without doc comments are flagged by `#![warn(missing_docs)]`. Adopt the lint at the crate level.

### Go — godoc convention

Go has no tag syntax. The convention is prose comments above each exported identifier, with the first sentence repeating the identifier name.

```go
// NormalizeEmail returns address lowercased with whitespace stripped.
// It does not validate the address; use ValidateEmail for that.
func NormalizeEmail(address string) string {
```

- Comments must directly precede the declaration, no blank line.
- First sentence is consumed by `go doc` as the summary; write it as a complete sentence starting with the identifier name.
- Package comments live on a `doc.go` file or above any one of the files in the package.
- `go vet` flags missing comments on exported identifiers when `-vetstaticcheck` or `golint`-style rules are enabled.

### Other ecosystems

- **Ruby — YARD** (`@param`, `@return`) on top of RDoc. Pick YARD for any project that ships hosted reference; RDoc only for the standard library convention.
- **Java — Javadoc.** `/** ... */` with `@param`, `@return`, `@throws`, `@since`, `@deprecated`. First sentence is the summary; all subsequent text is detail.
- **C# — XML doc comments.** `///` with `<summary>`, `<param>`, `<returns>`, `<exception>`. Consumed by IntelliSense and by DocFX.
- **C / C++ — Doxygen.** `/** ... */` or `/// ...` with `@brief`, `@param`, `@return`. Doxygen is the default; some projects use `clang-doc` for C++ instead.

## Examples in docstrings

Examples are the part of the docstring readers actually use. Hold them to the same standard as code in docs:

- Runnable. Imports included. No `...` placeholders that fail when pasted.
- Realistic identifiers. `customer_id`, `email`. Not `foo`, `x`.
- Show the output where the output is the point. `// => "alice@example.com"`.
- Test in CI where the language supports it (Rust doctests, Python `doctest`, Elixir doctests, Sphinx-extracted examples).
- Keep examples tight. Three lines that show the call and the result beat a fifteen-line setup.

## Comment voice

- Imperative or declarative present tense. "Normalize before hashing." Not "We normalize before hashing."
- Lead with the why. "RFC 5321 requires the local-part to be case-preserving in the wire form, but every real mailbox treats it as case-insensitive — we normalize at the boundary." beats "// normalize."
- Cite the constraint. "See RFC 5321 § 2.4." "Bug #4127." Always specific. The reader follows the citation when the rule changes.
- Drop polite hedging. No "I think," "probably," "should be fine." If the code is correct, say so. If you do not know, the comment belongs in a TODO with an owner and a tracking issue.
- One claim per comment. Multi-paragraph comments inside function bodies are usually the wrong shape — they want to be a docstring, an explanation page, or a refactor.

## Docstring voice

- One sentence summary, complete, ending in a period.
- Imperative mood ("Return the value." Not "Returns" or "This function returns"). PEP 257, Rust API guidelines, and Google style guides all agree on imperative summary lines.
- Code voice for every parameter, return value, exception, file path, and config key.
- Reference parameter names exactly as they appear in the signature.
- State what is _not_ included when callers might assume it is. "Does not validate." "Does not retry." "Not thread-safe."

## Anti-patterns

- **Docstring restating the function name.** "GetUser: gets the user." Delete or rewrite.
- **Type-only docstrings on a typed language.** `@param {string} name` on a TypeScript parameter typed `string`. Noise.
- **Author and date stamps.** `// Created by Alice on 2023-04-12`. Git carries this. Stamps rot the moment Alice leaves.
- **TODO without an owner or issue.** `// TODO: fix this`. A TODO that does not name who and where (the issue tracker) is a permanent fixture of the file.
- **Commented-out code.** Delete it. Git remembers. Comments that say `// kept for reference, do not delete` are worse — they freeze a snapshot that drifts from reality.
- **Marketing in docstrings.** "A blazing-fast, intuitive cache implementation that empowers developers." Strip every word that would not survive a code review.
- **Long block comments at the top of a function explaining the algorithm.** Either the algorithm is non-obvious enough to warrant a docstring section ("Implements Knuth's Algorithm X; see Knuth TAOCP Vol 4A § 7.2.2.1") or it is obvious enough that the comment is restating the code.
- **`// HACK:` without context.** Says nothing the next reader can use. State what is wrong, what would be right, and the issue tracking the fix.
- **Stale `@since` and `@deprecated` tags.** A `@deprecated` from three majors ago that points to a function also deprecated since. Audit yearly.
- **AI fingerprints.** "This function leverages a robust, scalable approach to seamlessly handle email normalization." Strip.

## Pre-commit checklist

Run before every code review:

- [ ] Every public symbol has a docstring, or a documented exemption (`#![allow(missing_docs)]`, `// eslint-disable-next-line jsdoc/require-jsdoc`, etc.).
- [ ] No docstring restates the symbol name without adding information.
- [ ] No type-only `@param` annotations on typed languages.
- [ ] Examples in docstrings are runnable and tested in CI where supported.
- [ ] Every TODO has an owner and a linked issue.
- [ ] No commented-out code.
- [ ] No author-and-date stamps.
- [ ] Comments explain _why_, not _what_.
- [ ] No `simply`, `just`, `seamless`, `delve`, `leverage` in either docstrings or comments.
- [ ] `@deprecated` items name the replacement and the removal target version.

## Self-review

Before opening the PR, name three comments you deleted and one comment you added. The deletions are the cleanup the diff earned; the addition is the rule the next reader will need.
