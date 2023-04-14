# Bear 2 Thumbnailer

A simple workaround for converting images into thumbnails in [Bear 2](https://beta.bear.app/).
The thumbnailer is available at [thumbnailer.burkeware.com](https://thumbnailer.burkeware.com/).

## How to use Bear 2 Thumbnailer

1. Select any or all of a note within Bear 2 that contains images
2. Copy the selection to your clipboard using Edit > Copy As > Markdown
3. Visit [Bear 2 Thumbnailer](https://thumbnailer.burkeware.com/), adjust the 
   thumbnail size if desired, and paste (⌘V)
5. Return to Bear 2 and paste again to replace the selected portion of the note with the
   identical content with images turned into thumbnails

## How it works

Bear 2 allows image resizing and this is accomplished under the hood by adding an HTML comment
after each image in the form `<!-- {"width":255} -->`. Because of this, images can be resized
by adding or editing these comments. While they are not directly editable within the Bear 2
editor, any contents (or the entire note) can be copied as markdown (selecting from Bear 2's 
menu `Edit > Copy As > Markdown`.

This Bear 2 Thumbnailer works by transforming any text pasted onto the page by converting any 
HTML comments following an image &mdash; `![](...)<!-- {"width":n} -->` &mdash; to the same
thing but with the width set to a pre-specified value. At the heart is a simple regular 
expression function in Javascript:

```js
var re = /(!\[\]\([^\)]+\))(<!--.*?-->)*(<!-- \{"width":(\d+)} -->)?/g;
return text.replace(re, "$1<!-- {\"width\":" + thumbnailWidth + "} -->");
```

In the case that more than one HTML comment setting width directly follows an image reference,
the Bear 2 Thumbnailer ignores all by the last one (the same way as resizing behavior within
Bear 2 works).
