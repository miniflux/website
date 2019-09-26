title: Rewrite and Scraper Rules
description: How to write custom scraper and rewrite rules
template: doc
uri: docs/rules.html
---
<h2 id="rewrite-rules">Rewrite Rules <a class="anchor" href="#rewrite-rules" title="Permalink">¶</a></h2>

To improve the reading experience, it's possible to alter the content of feed items.

For example, if you are reading a popular comic website like [XKCD](https://xkcd.com/),
it's nice to have the image title (the `alt` attribute) added under the image.
Especially on mobile devices where there is no `hover` event.

| Rule  | Description  |
|---|---|
| `add_dynamic_image` | Tries to add the highest quality images from sites that use JavaScript to load images (e.g. either lazily when scrolling or based on screen size). |
| `add_image_title` | Add each image's title as a caption under the image. |
| `add_youtube_video` | Insert Youtube video inside the article (automatic for Youtube.com). |
| `add_mailto_subject` | Insert mailto links subject into the article. |

Miniflux includes a set of default rules for some websites, but you could define your own rules.

On the feed edit page, enter your custom rules in the field "Rewrite Rules" like this:

```
rule1,rule2
```

Separate each rule by a comma.

<h2 id="scraper-rules">Scraper Rules <a class="anchor" href="#scraper-rules" title="Permalink">¶</a></h2>

When an article contains only an extract of the content, you could fetch
the original web page and apply a set of rules to get relevant contents.

Miniflux uses CSS selectors for custom rules. These custom rules can be
saved in the feed properties (Select a feed and click on edit).

| CSS Selector  | Description  |
|---|---|
| `div#articleBody` | Fetch a `div` element with the ID `articleBody` |
| `div.content` | Fetch all `div` elements with the class `content` |
| `article, div.article` | Use a comma to define multiple rules |

Miniflux includes a list of predefined rules for popular websites.
You could contribute to the project to keep them up to date.

Under the hood, Miniflux uses the library [Goquery](https://github.com/PuerkitoBio/goquery).
