---
title: Filter, Rewrite and Scraper Rules
description: How to write custom scraper and rewrite rules
url: /docs/rules.html
---

- [Filtering Rules](#filtering-rules)
- [Global Filtering Rules](#global-filtering-rules)
- [Rewrite Rules](#rewrite-rules)
- [Scraper Rules](#scraper-rules)
- [URL Rewrite Rules](#rewriteurl-rules)

<h2 id="filtering-rules">Feed Filtering Rules <a class="anchor" href="#filtering-rules" title="Permalink">¶</a></h2>

Miniflux has a basic filtering system that allows you to ignore or keep articles.

Location `/feed/XXX/edit`

### Block Rules

Block rules ignore articles with a title, an url, a tag or an author that match the regex ([RE2 syntax](https://golang.org/s/re2syntax)).

For example, the regex `(?i)miniflux` will ignore all articles with a title that contains the word Miniflux (case insensitive).

Ignored articles won't be saved into the database.

### Keep Rules

Keep rules keeps only articles that matches the regex ([RE2 syntax](https://golang.org/s/re2syntax)).

For example, the regex `(?i)miniflux` will keep only the articles with a title that contains the word Miniflux (case insensitive).

<h2 id="global-filtering-rules">Global Filtering Rules <a class="anchor" href="#global-filtering-rules" title="Permalink">¶</a></h2>

User level article filters that are applied to all articles from all feeds.

Each rule is required to be on a different line.

Field Options can be re-used. Example: There can be multiple `EntryTitle` rules.

The provided RegEx should be in the [RE2 syntax](https://golang.org/s/re2syntax).

Rule Format: 
```
FieldName=RegEx
FieldName=RegEx
FieldName=RegEx
```

Field Options:
- EntryTitle
- EntryURL
- EntryCommentsURL
- EntryContent
- EntryAuthor
- EntryTag

Location: `/settings`

**Note**: Order of rules will matter as the processor stops on the first match, for both Block & Keep rules.

### Block Rules

Block rules ignores articles that match a single rule.

For example, the rule `EntryTitle=(?i)miniflux` will ignore all articles with a title that contains the word Miniflux (case insensitive).

### Keep Rules

Keep rules will keep articles that match a single rule.

For example, the rule `EntryTitle=(?i)miniflux` will keep only the articles with a title that contains the word Miniflux (case insensitive).

### Order of Global Rules & Feed Rules

The order of how all rules get applied to articles: 

`Global Block Rules > Feed Block Rule > Global Keep Rules > Feed Keep Rule`

<h2 id="rewrite-rules">Rewrite Rules <a class="anchor" href="#rewrite-rules" title="Permalink">¶</a></h2>

To improve the reading experience, it's possible to alter the content of feed items.

For example, if you are reading a popular comic website like [XKCD](https://xkcd.com/),
it's nice to have the image title (the `alt` attribute) added under the image.
Especially on mobile devices where there is no `hover` event.

<dl>
    <dt><code>add_dynamic_image</code></dt>
    <dd>
        Tries to add the highest quality images from sites that use JavaScript to load images (e.g. either lazily when scrolling or based on screen size).
    </dd>
    <dt><code>add_dynamic_iframe</code></dt>
    <dd>
        Tries to add embedded videos from sites that use JavaScript to load iframes (e.g. either lazily when scrolling or after the rest of the page is loaded).
    </dd>
    <dt><code>add_image_title</code></dt>
    <dd>
        Add each image's title as a caption under the image.
    </dd>
    <dt><code>add_youtube_video</code></dt>
    <dd>
        Insert Youtube video to the article (automatic for Youtube.com).
    </dd>
    <dt><code>add_youtube_video_from_id</code></dt>
    <dd>
        Insert Youtube video to the article based on the video ID.
    </dd>
    <dt><code>add_invidious_video</code></dt>
    <dd>
        Insert Invidious player to the article (automatic for https://invidio.us).
    </dd>
    <dt><code>add_youtube_video_using_invidious_player</code></dt>
    <dd>
        Insert Invidious player to the article for Youtube feeds.
    </dd>
    <dt><code>add_castopod_episode</code></dt>
    <dd>
        Insert Castopod episode player.
    </dd>
    <dt><code>add_mailto_subject</code></dt>
    <dd>
        Insert mailto links subject into the article.
    </dd>
    <dt><code>base64_decode</code></dt>
    <dd>
        This rewrite rule decode base64 content.
        It can be used with a selector: <code>base64_decode(".base64")</code>, but can also be used without argument: <code>base64_decode</code>. In this case it'll try to convert all TextNodes and always fallback to original text if it can decode.
    </dd>
    <dt><code>nl2br</code></dt>
    <dd>
        Convert new lines <code>\n</code> to <code>&lt;br&gt;</code> (useful for non-HTML contents).
    </dd>
    <dt><code>convert_text_links</code></dt>
    <dd>
        Convert text link to HTML links (useful for non-HTML contents).
    </dd>
    <dt><code>fix_medium_images</code></dt>
    <dd>
        Attempt to fix Medium's images rendered in Javascript.
    </dd>
    <dt><code>use_noscript_figure_images</code></dt>
    <dd>
        Use <code>&lt;noscript&gt;</code> content for images rendered with Javascript.
    </dd>
    <dt><code>replace("search term"|"replace term")</code></dt>
    <dd>
        Search and replace text.
    </dd>
    <dt><code>remove(".selector, #another_selector")</code></dt>
    <dd>
        Remove DOM elements.
    </dd>
    <dt><code>parse_markdown</code></dt>
    <dd>
        Convert Markdown to HTML.
    </dd>
    <dt><code>remove_tables</code></dt>
    <dd>
        Remove any tables while keeping the content inside (useful for email newsletters).
    </dd>
    <dt><code>remove_clickbait</code></dt>
    <dd>
        Remove clickbait titles (Convert uppercase titles).
    <dd>
    <dt><code>replace_title("search-term"|"replace-term")</code></dt>
    <dd>
        Rewrite rule to adjust entry titles.
    <dd>
    <dt><code>add_hn_links_using_hack</code></dt>
    <dd>
        Open HN comments with Hack.
    <dd>
    <dt><code>add_hn_links_using_opener</code></dt>
    <dd>
        Open HN comments with Opener.
    <dd>
</dl>

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

<h2 id="rewriteurl-rules">Rewrite URL Rules <a class="anchor" href="#rewriteurl-rules" title="Permalink">¶</a></h2>

Sometimes it might be required to rewrite an URL in a feed to fetch better
suited content. For example, for  some users the URL
https://www.npr.org/sections/money/2021/05/18/997501946/the-case-for-universal-pre-k-just-got-stronger
displays a cookie consent dialog instead of the actual content and it would
be preferred to fetch the URL https://text.npr.org/997501946 instead. The
following rules does this:

````
rewrite("^https:\/\/www\.npr\.org\/\d{4}\/\d{2}\/\d{2}\/(\d+)\/.*$"|"https://text.npr.org/$1")
````

This will rewrite all URLs from the original feed to URLs pointing to _text.npr.org_
when the article  content is fetched. I also had to add my own scraper rule, because
the default rule will try to fetch #storytext.

Another example is the german page
`https://www.heise.de/news/Industrie-ruestet-sich-fuer-Gasstopp-Forscher-vorsichtig-optimistisch-7167721.html`
which splits the article into multiple pages. The full text can be read on
`https://www.heise.de/news/Industrie-ruestet-sich-fuer-Gasstopp-Forscher-vorsichtig-optimistisch-7167721.html?seite=all`

The URL rewrite rule for that would be

````
rewrite("(.*?\.html)"|"$1?seite=all")
````
