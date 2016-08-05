# Documentation

Just a spot to keep style guides and the like that I have spun up for various teams/projects.  Style guides are generally
inspired by, if not lifted (forked) wholesale from [AirBnB](https://github.com/airbnb/javascript).

## Pandoc Conversions

*Note*: Specifying "markdown_github" as our input format appears to get the best syntax highlighting, except for ReactJS code.

### PDF

``` pandoc -f markdown_github Sass.Style.Guide.md -o Sass.Style.Guide.pdf ```

### Word

``` pandoc -f markdown_github -t docx Sass.Style.Guide.md -o Sass.Style.Guide.docx ```

### HTML (syntax highlighting doesn't appear to work)

``` pandoc -f markdown_github Sass.Style.Guide.md -o Sass.Style.Guide.html ```

### Use a different highligher - say Zenburn for a dark background

``` pandoc -f markdown_github -t docx Sass.Style.Guide.md --highlight-style zenburn -o Sass.Style.Guide.docx ```

Available highlighters:

- pygments
- kate
- monochrome
- espresso
- haddock
- tango
- zenburn
