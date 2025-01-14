# Solana Developer Content Contributing Guide

We love your input! We want to make contributing to the
[Solana Developer](https://solana.com/developers) content as easy and
transparent as possible, whether it's:

- Reporting a bug
- Submitting a fix
- Discussing the current state of this repo
- Proposing new features

## Key points

- called the "developer content" repo or "developer content api"
- written in markdown with yaml frontmatter
- used yaml frontmatter, with structure enforced with Contentlayer
- all pages are grouped into "content types"
- supports some select custom components
- publicly displayed via the UI of [solana.com](https://solana.com) (located in
  a different repo)
- content translations are supported via Crowdin

## Content guidelines

Since this content within this repo is meant to be publicly displayed on
`solana.com`, we have very specific guidelines for the content that can be
merged into this repo (and eventually displayed on solana.com). If you would
like to submit changes to existing content or add new content, all of these
guidelines should be observed:

### Avoid official recommendations

Avoid any language around making "official recommendations" such as "I recommend
product X" or "product Y is the best". The content within this repo will be
publicly published on solana.com which is maintained by the
[Solana Foundation](https://solana.org). As such, any "product recommendations"
may appear as if coming directly the Solana Foundation. The Solana Foundation
does not make official recommendations for products, but rather help share what
options are available in the broader Solana ecosystem.

### Avoid "picking favorites"

Avoid language similar to "service X is my favorite". When talking about or
naming specific products within the Solana ecosystem, writers and maintainers
should make their best attempt to list multiple options that meet similar use
cases. As a general rule of thumb, try to share at least 3-4 options of similar
products or services, not just a single named one. For example, when talking
about wallet providers, a writer could list Phantom, Solflare, Backpack, and
Ultimate. When talking about RPC providers, a writer should link to
[solana.com/rpc](https://solana.com/rpc) instead of listing specific names.

### Use up-to-date code snippets

Write content that uses up-to-date code. Code bases change, functions get
deprecated, methods get removed. When submitting code snippets within content
here, use the most up-to-date code available for the given functionality being
used. Especially net new content, like adding a new guide.

## Style guidelines

To aid in keeping both consistent content and a high quality experience, all
code/content maintained within this repo shall use the style guidelines set
forth here.

Failure to adhere to these style guidelines will slow down the review process
and block approval/merging.

### Prettier

This repo uses prettier to help maintain the quality and consistency of the
content within. There is a master
[prettier configuration file](https://github.com/solana-foundation/developer-content/blob/main/.prettierrc)
(`.prettierrc`) in the root of this repo.

On all commits and PRs, their is a GitHub action that checks if your PR follows
the master prettier formatting. If your branch/code does NOT meet the prettier
formatting requirements, it will not be merged and it will delay its review.

If your editor is not configured to auto format on save using prettier, then you
can run the following command to auto format all files in your local repo/PR:

```shell
yarn prettier:fix
```

You can also run the prettier check command to see which files do not follow the
prettier formatting guidelines.

```shell
yarn prettier
```

### Heading styles

The content within a document should not start with a heading. It should start
with a paragraph of text. After each heading, should be non-heading content
(i.e. do not stack headings without putting a sentence/paragraph between them).

Content should NOT include a `h1` tag within it since solana.com will
automatically render a specific h1 based on the frontmatter's `title`. As such,
all markdown `h1` will be auto converted to a `h2`.

The text within a heading should NOT include inline code blocks (i.e. single
backtick). They should also NOT end with any special characters (i.e.
`:;!@#$%^&*`). Ending in a question mark is allowed.

All h2-h4 headings will be auto anchored, allowing anyone to directly link to
the specific section of content. A clickable anchor link will be rendered aside
the heading's text.

```md
## Simple, valid heading

The heading above is valid! Yay :)

## This is an `invalid` header

The heading above is invalid since it contains single backticks, aka this
character: `

### This is also invalid:

The heading above is invalid since it ends with a colon.

# this will become an h2

The heading above will be converted to a `h2` element

## This is okay?

The heading above is valid! Yay :)

#### this in not okay since it does not increment by only 1

The heading above is invalid since it skips the `h3` heading (`###`)
```

### Table of contents

When a content page is rendered on solana.com, a table of contents will be auto
generated based on the markdown headings within the document's content.
Specifically: all `h2`, `h3`, and `h4` (aka h2-h4) tags will be include in the
table of contents. All headings greater than `h4` will be ignored from the table
of contents.

The exact text within the heading will be rendered as a line item in the table
of contents. As such, thought should be put into the text within a heading,
especially for accessibility.

## Content types

This repo contains multiple different types of developer content records. Each
grouping of records (called a "content type") serve a different purpose and are
handled differently when displayed in the UI of `solana.com`.

Below is a table describing each active content type group, including the
corresponding path within this repo and webpage for viewing on solana.com:

| Content Type | Repo Path                                   | Webpage URL                                               |
| ------------ | ------------------------------------------- | --------------------------------------------------------- |
| `docs`       | [`/docs`](/docs/)                           | [View Docs](https://solana.com/docs)                      |
| `rpc`        | [`/docs/rpc`](/docs/rpc/)                   | [View Docs](https://solana.com/docs/rpc)                  |
| `guides`     | [`/content/guides`](/content/guides/)       | [View Guides](https://solana.com/developers/guides)       |
| `resources`  | [`/content/resources`](/content/resources/) | [View Resources](https://solana.com/developers/resources) |
| `cookbook`   | [`/content/cookbook`](/content/cookbook)    | [View Cookbook](https://solana.com/developers/cookbook)   |

> Note: Even though `rpc` is technically a different record type, it is treated
> effectively the same as the `docs` type. More details in the
> ["Frontmatter for Docs"](#frontmatter-for-docs) section.

## Written in markdown

Every piece of content within this repo is normally written in markdown with
yaml frontmatter. With some support for specific
[custom components](#components) using React and MDX.

We typically use GitHub flavor markdown (GFM) which you can
[learn more about here](https://github.github.com/gfm/).

## Frontmatter

The yaml frontmatter is used to provide additional metadata about a given piece
of content. Each content type has specifically supported frontmatter fields,
some required and some optional. All available frontmatter fields are enforced
via [Contentlayer](#why-contentlayer) (see more below).

Here is an
[example of the frontmatter](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L1-L25)
within a piece of content within this repo.

### Why Contentlayer?

[Contentlayer](https://contentlayer.dev/) offers yaml frontmatter structure
enforcement and type safety for the frontmatter fields on every content record,
including generating TypeScript types for each content type's grouping.

Contentlayer does this code generation by enabling us to define a custom data
schema for the frontmatter. You can view the current Contentlayer schema here:
[`contentlayer.config.ts`](/contentlayer.config.ts)

If any content records contains unsupported or misspelled fields in the
frontmatter, Contentlayer will throw an error, preventing misconfigured content
from being shipped to production.

### Default frontmatter fields

Each content type (i.e. `docs`, `guides`, `resources`, etc) has the ability to
support different custom metadata fields within the yaml frontmatter.

While each content group may support additional frontmatter fields, the
following are default supported by all content groups:

- [`title`](#title)
- [`description`](#description)
- [`tags`](#tags)
- [`keywords`](#keywords)
- [`date`](#date)
- [`updatedDate`](#updateddate)

#### title

The primary title of the individual piece of content

- name: `title`
- type: `string`
- required: `true`

Example:
[this line of text](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L4)

The `title` field will be used in auto-generating the social share card images
(aka OpenGraph images) that people will see when linking to the content page on
solana.com.

#### description

Brief description of the content (also used in the SEO metadata)

- name: `description`
- type: `string`
- required: `false`

Example:
[these lines of text](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L5-L7)

> This field will be used when a short description is needed to be displayed,
> like in typical blog style "overview cards" and for SEO related metadata.

#### tags

List of filterable tags for content

- name: `tags`
- type: `string`
- required: `false`

Example:
[these lines of text](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L8-L12)

#### keywords

List of keywords for the content, primarily used for SEO metadata when
generating the website.

- name: `keywords`
- type: `list` of `string`s
- required: `false`

Example:
[these lines of text](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L13-L22)

#### date

The date this content was published

- name: `date`
- type: `date` string in the format of `MMM DD, YYYY`
- required: `false`

Example:
[this line of text](https://github.com/solana-foundation/developer-content/blob/9bf2b82d684ad487c9282c529cd998f3a6249bd9/content/guides/getstarted/local-rust-hello-world.md?plain=1#L2)

#### updatedDate

The date this content was last updated

- name: `updatedDate`
- type: `date` string in the format of `MMM DD, YYYY`
- required: `false`

### Frontmatter for Docs

At this time, the `docs` content type does not have any additional frontmatter
fields. They only support the shared default fields

> Note: While technically, the `docs` content type is separate than the `rpc`
> content type, these are effectively treated the same. Therefore, assume all
> mentions of `docs` also apply to the `rpc` type unless otherwise noted.

### Frontmatter for Guides

At this time, the `guides` content type does not have any additional frontmatter
fields. They only support the shared default fields.

### Frontmatter for Resources

In addition to the default frontmatter fields, `resources` support the following
fields:

- [`category`](#category)
- [`repoUrl`](#repourl)

#### `category`

General category of the resource

- type: `enum`
- required: `true`
- values:
  - `documentation`
  - `framework`
  - `sdk`

#### `repoUrl`

Repository URL for the developer resources

- type: `string`
- required: `false`

## Components

In addition to the standard GitHub-flavored markdown text, the content records
within this repo support some extra functionality powered by custom React
components (or custom implementations of standard components). The following is
a list of available components

> Note: While markdown does normally support standard HTML tags being rendered
> with the rest of the markdown document's content, this should normally be
> avoided within this developer content repo. See more in the
> [Style Guidelines](#style) section.

- [html tags](#html-tags) - generally not to be used, with a few exceptions
- [headings](#headings) - details about how to include headings in a piece of
  content
- [images](#images) - details about how to include images in a piece of content
- [code blocks](#code-blocks) - additional functionality on top of standard
  markdown code blocks
- [blockquote](#blockquote) - additional functionality on top of the standard
  HTML `blockquote` element
- [Callout](#callout) - custom component used to render message to the reader in
  a more visually distinctive way (i.e. error messages, warnings, etc)
- [Embed](#embed) - embedding specific types of external content (i.e. YouTube
  videos)

### HTML tags

Since the content files in this repo use markdown, using HTML tags within should
be avoided. The markdown content render/parser will handle converting the text
blocks into the associated HTML code blocks when rendered on page.

A few HTML elements are acceptable to use, the rest will normally be rejected:

- `details` - see
  [this example](https://github.com/solana-foundation/developer-content/blob/7937bbfbff8c5d33991d7edfd147ab192402760e/docs/rpc/deprecated/getConfirmedBlock.mdx?plain=1#L65-L74)

### Headings

Headings (i.e. standard HTML tags of h1, h2, h3, etc) can be added into content
using standard markdown based headings.

Example:

```md
# this gives an h1 (and should not be used)

## this gives an h2

### this gives an h3

#### this gives an h4

##### this gives an h5
```

> Note: All markdown `h1`s will be auto converted to an `h2` tag.

See more details above about the [Heading styles](#heading-styles).

### Images

Images can be embedded directly within markdown using standard markdown
formatting. Each image should use the absolute path of the image file located
within this repo (i.e. `/public/assets/*`) and have a descriptive label
attached. This label will be rendered on the page with the image.

All images should be uploaded via a PR to this repo and stored within the
`/public/assets/` directory.

Embedding external images within markdown is generally not allowed.

```md
![descriptive label here](/public/assets/docs/transaction.svg)
```

### Code blocks

In addition to standard markdown
["fenced" code blocks](https://github.github.com/gfm/#fenced-code-blocks) (i.e.
using triple backticks), the developer content repo supports additional
functionality on top of code blocks.

> Code block syntax highlighting and some other features utilize
> [`rehype-pretty-code`](https://rehype-pretty.pages.dev/) and
> [`skiki`](https://github.com/shikijs/shiki) under the hood for some of the
> additional functionality supported.

#### Meta strings

Fenced code blocks support including additional metadata within their first
line, called "meta strings". These meta strings allow the markdown processor to
handle different logic of this user defined metadata.

While most are familiar with language meta string, which enables syntax
highlighting. The following example has a the language meta string of
`typescript`:

````md
```typescript
const example: string = "'typescript' is the language meta string";
```
````

This repo supports other meta strings as well:

- the standard syntax for the language value, which enables
  [syntax highlighting](#syntax-highlighting)
- [file names](#file-names)
- [line highlighting](#highlight-lines)
- [character/word highlighting](#highlight-characters)
- [diff lines](#diff-lines)

#### Syntax highlighting

Each code block can have syntax highlighting for its specific language by noting
the language used immediately following the triple backticks.

Most code languages are supported including these commonly used ones: `rust`,
`ts` or `typescript`, `js` or `javascript`, `toml`, `shell`

The code block's language should be in all lower case and have no space between
the language name and the backticks.

Examples:

````md
```ts
const example: string = "this is typescript";
```
````

````md
```shell
solana airdrop 2
```
````

#### File names

Code blocks can be have an optional header displayed on top the code block
element itself. This is commonly used to display the "file name" in which a code
snippet comes from.

To display the file name header on a code block, in the same line of the triple
backticks and language add the `filename=` field and set your desired name.

Examples:

````md
```ts filename="index.ts"
const example: string = "this is typescript";
```
````

````md
```rust filename="lib.rs"
use anchor_lang::prelude::*;

declare_id!("Bims5KmWhFne1m1UT4bfSknBEoECeYfztoKrsR2jTnrA");
```
````

#### Highlight lines

Code block can highlight lines using the
[syntax provided via `rehype-pretty-code`](https://rehype-pretty.pages.dev/#highlight-lines).

In the meta string, place a numeric range inside `{}`.

Examples:

````md
```js {1}
// this will be highlighted
// this will NOT
```
````

````md
```rust filename="lib.rs" {1,3}
// this will be highlighted
// this will NOT
// this will be highlighted
```
````

#### Highlight characters

Code block can highlight characters using the
[syntax provided via `rehype-pretty-code`](https://rehype-pretty.pages.dev/#highlight-chars).

In the meta string, place character segments between two `/` symbols. You can
also highlight multiple segments of characters.

Examples:

````md
```js /word/
// this will be highlighted: word
// this will partially highlight: hello-word
```
````

````md
```js /word/ /partially/
// this will be highlighted: word
// this will partially highlight: hello-word
```
````

````md
```rust filename="lib.rs" /word/ {1,3}
// this will be highlighted: word
// nothing highlighted
// this will partially highlighted word and line: hello-word
```
````

#### Diff lines

Within a code block you can create a "diff" style line (i.e. a line was added or
a line was removed) using the

At the end of a line, add one of the following notations to the end of a line:

- to show a red "line removed" diff line: `// [!code --]`
- to show a green "line added" diff line: `// [!code ++]`

Example:

```ts
const keep = "line that stays";
const updated = "example that was should be removed"; // [!code --]
const updated = "example that was should be added"; // [!code ++]
const didNotChange = "this line did not change";
```

### blockquote

Standard markdown quote blocks are rendered with our custom
[`Callout` component](#callout).

Nested blockquotes are not allowed.

```md
> This will be the `Callout` component with its default styles
```

Multiline blockquotes are allowed and will still be rendered as the default
`Callout`.

```md
> This blockquote will also be rendered as the `Callout` component with default
> styles. But since the text is much longer (and we enforce a Prettier
> configuration with a max line width) it should be wrapped to multiple lines.
```

```md
> > This is a nested blockquote and is not allowed
```

### Callout

The custom `Callout` component can be used to render message to the reader in a
more visually distinctive way (i.e. error messages, warnings, etc). Allowing a
write to draw more focus to a specific statement.

> Note: All standard [markdown blockquotes](#blockquote) will be auto converted
> to a `Callout` component with default styles.

The contents within the `Callout` (aka `children` in React land) will be
processed just like any other markdown. So you can put code blocks, lists, or
what ever inside.

```md
<Callout>
This will be the default "info" type callout
</Callout>
```

#### Callout props

The `Callout` component has the following props:

- name: `type`
  - required: `false`
  - type: `enum`
  - default: `info`
  - values:
    - `info` - a blue callout block (equivalent values: `note`, `blue`)
    - `warning` - a red callout block (equivalent values: `yellow`)
    - `caution` - a yellow callout block (equivalent values: `yellow`)
    - `success` - a green callout block (equivalent values: `green`)
- name: `title` - override the default title that is the `type`
  - required: `false`
  - type: `string`
  - default: the same text as the `type`'s enum variant

```md
<Callout type="caution">
  this will render as a yellow callout with a title of "Caution"
</Callout>
```

```md
<Callout type="success" title="You did it!">
  this will render as a green callout with a title of "You did it!"
</Callout>
```

Callouts can also render other markdown content within it, just like you might
expect. See this
[example here](https://github.com/solana-foundation/developer-content/blob/7937bbfbff8c5d33991d7edfd147ab192402760e/content/guides/getstarted/setup-local-development.md?plain=1#L242-L247)
or the one below:

```md
<Callout type="success" title="You did it!">
  
  - list item 1
  - list item 2

</Callout>
```

### Embed

The custom `Embed` component can be used to handle custom rendering of
specifically supported external content.

Currently supported external content:

- [YouTube videos](#youtube-video)
- [Whimsical diagram](#whimsical-diagram)

#### YouTube video

To embed a YouTube video:

```md
<Embed url="https://youtu.be/eudSSvyZF9o" />
```

Or

```md
<Embed url="https://www.youtube.com/watch?v=eudSSvyZF9o" />
```

#### Whimsical diagram

To embed a Whimsical diagram:

```mdx
<Embed url="https://whimsical.com/embed/PjBVAREV3DMyW5722tFKJ8" />
```

> Note: The preferred way to display a Whimsical diagram is to export the
> diagram as an SVG and directly [embed the image](#images) within a specific
> piece of content.

## Translations

Content translations are supported via the Crowdin platform.

All the markdown content committed to this repo is in the base language of
`English`. On each successful merge of content, all changes are uploaded to
Crowdin (via GitHub actions) for them to be translated into the supported
locales.

During production deployments of the developer content api, all the currently
translated content is downloaded from Crowdin and deployed for solana.com to
correctly render the user requested locale of content. Falling back to the base
language when no translated content was found.

## Local development

This developer content repo is a NextJS application that serves the markdown
based content as REST api for solana.com to consume. Think of it like a
microservice for content.

We call this microservice the "developer content api", or content api for short.

With this repo being only a "content api", there is no web app frontend to view
the content within this repo. The frontend UI that will render all the markdown
content within this repo is actually solana.com (which is a different repo).

### Setup locally

To setup the developer content api:

1. Clone the repo to your local machine:

```shell
git clone https://github.com/solana-foundation/developer-content.git
cd developer-content
```

2. Install the dependencies via `yarn`:

```shell
yarn install
```

3. Run the developer content api locally:

```shell
yarn dev
```

> Note: The developer content api normally runs locally on port `3001`
