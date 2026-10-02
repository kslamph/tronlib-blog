# tronlib-blog

Source for **https://kslamph.github.io/tronlib-blog/** — practical notes on
building on TRON in Go with [kslamph/tronlib](https://github.com/kslamph/tronlib).

## Layout

```
index.html                              post index
posts/YYYY-MM-DD-slug.html              one file per post
style.css                               single stylesheet, no build step
.nojekyll                               serves files verbatim (no Jekyll)
```

## Publishing

There is no build step and no dependencies. To add a post:

1. Copy the `<head>` block of an existing post — it carries the title,
   description and OG tags.
2. Save it as `posts/YYYY-MM-DD-slug.html`.
3. Add a bullet to the list in `index.html`.
4. Commit and push; Pages publishes from the default branch.

## Rule for code in posts

**Every code block must compile against the released module before it is
published.** Verify locally, then paste:

```sh
mkdir -p /tmp/snip && cd /tmp/snip
cat > go.mod <<'EOM'
module snip
go 1.25
require github.com/kslamph/tronlib/v2 v2.0.0
replace github.com/kslamph/tronlib/v2 => /path/to/tronlib
EOM
# ... write main.go ...
go build ./... && go vet ./...
```

The first draft of the TRC-20 post shipped a `tron.Error{}` value that does not
implement `error` (pointer receiver). The compiler caught it; readers would not
have.
