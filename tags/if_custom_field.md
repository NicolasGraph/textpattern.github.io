---
layout: document
category: Tags
published: true
title: If custom field
description: The if_custom_field tag will execute the contained statements if custom fields for a given article have content.
tags:
  - Article tags
  - Conditional tags
---

# If custom field

On this page:

* [Syntax](#syntax)
* [Attributes](#attributes)
* [Examples](#examples)
* [Genealogy](#genealogy)

## Syntax

~~~ html
<txp:if_custom_field>
~~~

The **if_custom_field** tag is a *conditional* tag and always used as an opening and closing pair, like this...

~~~ html
<txp:if_custom_field>
    ...conditional statement...
</txp:if_custom_field>
~~~

The tag will execute the contained statements if one or more custom fields for a given article have content. The contents of a custom field can be displayed with the [custom_field](custom_field) tag.

## Attributes

Tag will accept the following attributes (**case-sensitive**):

`match="match type"`
: How you wish your value to be tested. Choose from: \\
`exact`: value text must exactly match the custom field. \\
`any`: checks if any of the given comma-separated list of `value`s occur anywhere in the custom field. \\
`all`: checks if all of the given comma-separated list of `value`s occur anywhere in the custom field. \\
`pattern`: allows you to specify a regular expression in your `value` attribute to match against the custom field.
: **Default:** `exact`.

`name="field name"`
: The custom field name you wish to check.

`separator="character"`
: If you wish to treat your custom field as a list of items - so that each item is a discrete entity and tested separately when using `any` or `all` matching - specify the delimiter that you use in the custom field. This attribute is ignored if using `exact` or `pattern` matching.

`value="field value"`
: The custom field content you want to check for a match.

## Examples

### Example 1: Display contents of custom fields

~~~ html
<txp:if_custom_field name="subtitle">
    <txp:custom_field name="subtitle" />
</txp:if_custom_field>
~~~

Checks if a custom field has any content (at all) and display it.

Why might you do it? Say, you are publishing book reviews on your site and you use custom fields to enter the author, title, publisher and year of publication. Some of the books have a subtitle, others don't, so the conditional checks if the custom field you named `subtitle` holds any content and if it does, it will be displayed. If it's empty, the field won't turn up on the page.

The whole set of custom fields could look like this:

~~~ html
<h4>
    <txp:custom_field name="author" />:
    <txp:custom_field name="title" />
</h4>
<txp:if_custom_field name="subtitle">
    <p>
        <txp:custom_field name="subtitle" />
    </p>
</txp:if_custom_field>
<p>
    Published by
    <txp:custom_field name="publisher" />
    in
    <txp:custom_field name="year" />
</p>
~~~

For a **book that has a subtitle*, this may be seen:

~~~ html
<h4>Stephen Covey: The Seven Habits of Highly Effective People</h4>
<p>Powerful lessons in personal change</p>
<p>Published by Simon &amp; Schuster in 2002.</p>
~~~

For a **book without a subtitle*, this might be shown:

~~~ html
<h4>J.R.R. Tolkien: The Lord of the Rings</h4>
<p>Published by HarperCollins in 2004.</p>
~~~

Other tags used: [custom_field](custom_field).

### Example 2: Check custom field value

A mood indicator:

~~~ html
<txp:if_custom_field name="mood" value="happy">
    <img src="happy-face.png" alt="Happy">
</txp:if_custom_field>

<txp:if_custom_field name="mood" value="sad">
    <img src="sad-face.png" alt="Sad">
</txp:if_custom_field>
~~~

Checks the content of the custom field named `mood` to see if it matches the text `happy` or `sad`. Depending which one it matches determines which of the emoticon images is displayed.

Why might you do it? If you define a custom field `mood`, you can enter a word to indicate your mood while writing an article. You enter either `happy` or `sad`.

### Example 3: Use the tag with else

~~~ html
<txp:if_custom_field name="website">
    <p>
        <txp:custom_field name="website" />
    </p>
<txp:else />
    <p>Unfortunately, this band hasn't got a website.</p>
</txp:if_custom_field>
~~~

If the custom field named 'website' has some content, display it, otherwise display a standard message.

Why might you do it? If you publish music reviews and you've set up some custom fields for the band name, the album title and the band's website. But not all bands have a website and you want to display a standard message if a band hasn't got one.

Other tags used: [custom_field](custom_field), [else](else).

### Example 4: Display a conditional statement based on a comma separated value

~~~ html
<txp:if_custom_field name="animals" separator="," match="any" value="monkeys">
    <p>The monkeys are eating bananas.</p>
<txp:else />
    <p>The bears ate all the monkeys. How sad.</p>
</txp:if_custom_field>
~~~

Checks the content of the custom field named `animals` which has a comma separated list of animals. It checks if it contains the text `monkeys`, and displays a conditional statement if it does.

Other tags used: [else](else).

## Genealogy

### Version 4.3.0

`val` attribute deprecated and renamed to `value`. \\
`matc` and `separator` attributes added.
