# An HTML Markup Extension for AI Agents (aid-markup)

**Edition**: Draft

**Author**: Konstantin Kolomeitsev

**Date**: January 25, 2025

## Summary

This document describes a proposal to extend HTML capabilities with additional markup aimed at interaction with AI agents. The new markup (hereafter referred to as _aid-markup_) is intended to provide a more explicit and structured description of a web page’s content so that artificial agents (bots, assistants, analytical systems) can effectively interpret and process the information on those pages.

The foundation of _aid-markup_ is the introduction of several custom attributes and conventions for their values, which help AI agents better understand the role and state of various interface elements, as well as the semantics of the content.

## Introduction and Motivation

### The Problem

Modern AI agents actively analyze web pages to accomplish a wide range of tasks—from searching for and extracting information to navigating complex user interfaces. However, current HTML specifications and popular structured data schemas (e.g., Microdata, RDFa, JSON-LD) do not always provide convenient enough means to describe:

1. **Structural roles** of interface elements (header, sidebar, content blocks, etc.);
2. **States** of interactive components (loading, waiting, completed, etc.);
3. **Additional content characteristics**, related, for example, to products, services, reviews, etc.

To overcome these limitations and give AI agents more detailed information about interface elements, we propose a set of attributes and types that help formalize the content.

### Goals

1. Create a **unified set** of common indicators for structural elements, enabling AI agents to more quickly understand the hierarchy and logic of the interface.
2. Provide a **simple and universal** way to describe states and additional metadata.
3. Improve **accessibility** and **clarity** of web applications for intelligent systems without breaking existing compatibility with browsers and other tools.

### Advantages

- **Simplified analysis**: Clear semantics of structural and content elements helps AI systems find the necessary data more quickly.
- **Flexibility**: Additional attributes are easily extensible and integrate with existing HTML5 tags and the custom data attribute technology.
- **Compatibility**: When used correctly, _aid-markup_ does not disrupt the operation of existing rendering and SEO mechanisms, as it relies on the widely supported practice of “custom attributes” and does not conflict with native HTML elements or attributes.

## Core Functionality

### 1. Structural Types (the `aid-type` attribute)

The `aid-type` attribute describes the **structural role** or purpose of an element in the interface. The following values are proposed:

- `header` – for top headers (usually includes a logo, menu, brief information).
- `footer` – for bottom footers (contacts, copyright, supplementary links).
- `sidebar` – for side panels (navigation, ads, info blocks).
- `content` – for the main page content (text, articles, products).
- `section` – for logically isolated content blocks inside the main content.
- `nav` – for areas with navigation links or menus.
- `form` – for forms (login, registration, search, etc.).
- `modal` – for pop-up modal windows (confirming an action, providing additional info).
- `tooltip` – for pop-up hints or tooltips.
- `interactive` – for interactive elements: buttons, input fields, switches, links, etc.

#### Usage

```html
<div aid-type="header">
  <h1>My Website</h1>
  <nav aid-type="nav">
    <!-- Menu links -->
  </nav>
</div>

<div aid-type="sidebar">
  <!-- Sidebar blocks -->
</div>

<div aid-type="content">
  <!-- Main content -->
</div>

<div aid-type="footer">
  <!-- Website footer -->
</div>
```

### 2. Descriptive Types

#### 2.1. `aid-desc`

The `aid-desc` attribute is intended for a **brief description** of the structural type or purpose of an element. This can help an agent get a more detailed interpretation of “what” this element is used for.

```html
<div aid-type="content" aid-desc="Main section with travel articles">
  <!-- Articles content -->
</div>
```

#### 2.2. `aid-state`

The `aid-state` attribute describes the **current state** of an element or component. Possible values:

- `idle` – the element is inactive, with no ongoing actions;
- `loading` – data is being loaded;
- `processing` – a request is being processed;
- `wait` – waiting for user action or another event;
- `done` – an operation or process has successfully completed.

```html
<button aid-type="interactive" aid-state="processing">Processing...</button>
```

### 3. Extended Types for Content

#### 3.1. `aid-cnt-type`

The `aid-cnt-type` attribute indicates the **type of content** (for example, a product, a service, an informational article). Three main values are proposed:

- `product` – for a product;
- `service` – for a service;
- `info` – for informational content (article, news, post).

```html
<div aid-type="content" aid-cnt-type="product">
  <!-- Product card -->
</div>
```

#### 3.2. `aid-cnt-kind` and Related Labels

The `aid-cnt-kind` attribute introduces additional classification related to specific content characteristics. Values can be anything; however, for consistency we recommend the following keys:

- `aid-cnt-id` — unique content ID (SKU, article number, product ID).
- `price` — the content price (e.g., for products, subscriptions).
- `char` — characteristics (color, size, technical features).
- `terms` — conditions (contracts, terms of use, shipping conditions, etc.).
- `desc` — description (usually more detailed than `aid-desc`).
- `opt` — additional options (configuration or equipment variants).
- `pic` — links to pictures or the images themselves.
- `vid` — links to videos related to the content.
- `sound` — links to audio or sound files.
- `rating` — rating or review information.
- `comment` — a comment block for this content.
- `statistic` — statistical data (counters, likes, views).
- `sim` — a list of or link to similar products/services/articles.

**Note**: In the _aid-markup_ proposal, it is possible to freely combine these labels. Developers can add them to elements as needed.

```html
<div aid-cnt-type="product" aid-cnt-id="SKU123">
  <img aid-cnt-kind="pic" src="images/smartphone.jpg" alt="Smartphone X" />
  <p aid-cnt-kind="char">Color: Black</p>
  <p aid-cnt-kind="char">RAM: 6 GB</p>
  <p aid-cnt-kind="terms">Warranty: 12 months</p>
  <div>
    <!-- Comments section -->
    <p aid-cnt-kind="comment">Excellent smartphone, very satisfied!</p>
  </div>
  <div>
    <!-- Similar products section -->
    <a href="/xl-phone" aid-cnt-kind="sim">Smartphone XL</a>
  </div>
</div>
```

## Application for AI Agents

1. **Structural analysis**: By using the `aid-type` attribute, AI can quickly determine where the site’s key areas are located—header, footer, sidebar, main content.
2. **State tracking**: `aid-state` lets bots and agents understand when a component is currently loading data or waiting for user action.
3. **Metadata extraction**: With the help of `aid-cnt-type`, `aid-cnt-kind`, and corresponding keys, AI agents can parse products, services, and any other content types more precisely, without resorting to cumbersome heuristics.

## Usage Example

Below is a simplified example of a web page where _aid-markup_ helps structure and describe content for AI agents.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Example of aid-markup Usage</title>
  </head>
  <body>
    <header aid-type="header" aid-desc="Main header with a logo and menu">
      <img src="logo.png" alt="Logo" />
      <nav aid-type="nav" aid-desc="Main navigation menu">
        <ul>
          <li><a aid-type="interactive" href="/">Home</a></li>
          <li><a aid-type="interactive" href="/catalog">Catalog</a></li>
          <li><a aid-type="interactive" href="/contacts">Contacts</a></li>
        </ul>
      </nav>
    </header>

    <aside aid-type="sidebar" aid-desc="Sidebar panel with filter links">
      <!-- Filters, additional information -->
    </aside>

    <main aid-type="content" aid-desc="Main content area">
      <section aid-type="section" aid-desc="Popular products">
        <article aid-cnt-type="product" aid-cnt-id="PRD001">
          <img aid-cnt-kind="pic" src="notebook.jpg" alt="Laptop" />
          <p aid-cnt-kind="desc">A laptop suitable for work and study</p>
          <p aid-cnt-kind="price">$500</p>
        </article>
        <article aid-cnt-type="product" aid-cnt-id="PRD001">
          <img aid-cnt-kind="pic" src="notebook.jpg" alt="Gaming Mouse" />
          <p aid-cnt-kind="desc">Gaming mouse with LED lighting</p>
          <p aid-cnt-kind="price">$80</p>
        </article>
      </section>

      <section aid-type="section" aid-desc="Services section">
        <div aid-type="content" aid-cnt-type="service" aid-cnt-id="SVC001">
          <p aid-cnt-kind="desc">Repair and maintenance services</p>
          <p aid-cnt-kind="price">$50</p>
          <p aid-cnt-kind="terms">On-site technician within 1 day</p>
        </div>
      </section>
    </main>

    <footer
      aid-type="footer"
      aid-desc="Site footer with contacts and copyright"
    >
      <p>Copyright © 2025</p>
    </footer>
  </body>
</html>
```

## Compatibility, Privacy, and Security Concerns

1. **Browser compatibility**: This proposal is based on the use of **custom attributes**, which **do not break** the existing DOM model and are correctly ignored by older browsers. Compatibility remains intact.
2. **Possible SEO impact**: By default, search engines will not react negatively to new attributes (they are ignored as _unknown_), but in the future, once such schemes are supported, there may be improvements in ranking or data extraction convenience.
3. **Privacy**: This markup does not require developers to specify personal information. Any personalized or confidential information should be labeled with caution.
4. **Security**: Since this is only a set of attributes, _aid-markup_ does not run scripts and does not affect CORS or content security policies. The risks associated with its use are minimal and equivalent to those of using ordinary HTML attributes.

## Conclusion

_aid-markup_ is a concept that can make it easier for AI agents to perform structured analysis of web content. Thanks to a simple yet expressive set of custom attributes, developers can explicitly indicate the semantic role of elements, the current state of interactive components, and key content characteristics.

This significantly improves how automated systems interact with user interfaces, simplifies writing web-testing scripts and parsing robots, and opens up new possibilities for enhancing accessibility.

Since this document is a **draft**, we invite the community to discuss, test, and further refine the specification based on real use cases.

## Additional. Tools

Currently, I am working on a Chrome extension that will dynamically add aid HTML tags from a knowledge base to websites I have manually annotated.

The markup files will be stored in this repository in the [/markups](./markups) folder.
