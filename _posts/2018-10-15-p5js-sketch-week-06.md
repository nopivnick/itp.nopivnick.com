---
layout:       single
title:        "ICM Week 6: My Sixth P5.js Sketch"
date:         2018-10-15 21:09:31 -04:00
categories:   ICM
---

For [this week's assignment](https://editor.p5js.org/nopivnick/sketches/ryPBA2fiQ) I attempted to deface the [About](http://catskillfoodcompany.com/about.php) page of my dear friend Jonah's business. Jonah makes sausage and I just think that's hysterical.

This is what the hosted page looks like rendered in the browser:

<iframe src="https://drive.google.com/file/d/18r3bJGFn41T4nCPMz7j9za6Bh-ezZ9Xt/preview" width="640" height="480"></iframe>

However downloading the content from the server and uploading it to the P5 sketch appears to have completely broken the css:

<iframe src="https://drive.google.com/file/d/18l64kqictPSxyNulErZVPy3995oX7Ncd/preview" width="640" height="480"></iframe>

During upload, the P5.js site refused to accept certain files claiming: "You can't upload files of this type."

<iframe src="https://drive.google.com/file/d/18kLIsx0vn2iYgZNZ7971mGx7TMdIC1c5/preview" width="640" height="480"></iframe>

The offending files were all plain text with filenames 'css, css(1), css(2), and css(3)' but lacking any file suffix (e.g., .css). their content certainly looks like CSS:

```css
/* latin-ext */
@font-face {
  font-family: 'Bitter';
  font-style: italic;
  font-weight: 400;
  src: local('Bitter Italic'), local('Bitter-Italic'), url(https://fonts.gstatic.com/s/bitter/v13/rax-HiqOu8IVPmn7erxlJD1wmULYyT8.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: 'Bitter';
  font-style: italic;
  font-weight: 400;
  src: local('Bitter Italic'), local('Bitter-Italic'), url(https://fonts.gstatic.com/s/bitter/v13/rax-HiqOu8IVPmn7erxrJD1wmULY.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```
It seems to be specific to font styling so maybe it's to do with localization.

I know this site uses WordPress to manage content and generate pages using php and a database, likely MySQL. I might also mention the markup in the html page I downloaded was horribly formatted and I was pleasantly surprised to see P5.js editor's Tidy Code functionality works on `index.html` as well as `sketch.js`.

Another observation: when uploading the style.css file that was downloaded as part of the 'Save website as ...' to the P5.js editor, all the file's content disappeared. Regardless, I manually pasted the content of the original site's `style.css` but no change.

I also tried moving `style.css` between the `catskill-about` directory and the root of the project folder then editing the relative path in `index.html`:

```html
<link rel="stylesheet" href="./about-catskill_files/styles.css">
```

to:

```html
<link rel="stylesheet" href="styles.css">
```

Still no effect.

So the site looks like garbage without the css working and 'index.html' is so convoluted (likely due at least in part to WordPress) it makes it difficult to explore this week's assignment. I've put so much time into this so far, I figured the best course of action was to start stripping out as much of the original html content as possible just to get to a point where the content is more easily manipulated.

After wasting a considerable amount of time wrestling with the `html`, I haven't got very much to show for myself. I regret not having found a simpler site to play with.

- I couldn't figure out how to have the P5 canvas lay beneath the rest of the content in index.html

- short of the above, I couldn't figure out how to get the p5 canvas to appear as the first element (i.e., at the top of the page) despite positioning `<script src="sketch.js"></script>` as the first element under `<body>`.

- the image I'm calling in `sketch.js` appears to fill the page when the sketch is first run but disappears as soon as the cursor passes into the frame. I suspect that has something to do with mouseX and mouseY defaulting to (0, 0) before the cursor even crosses into the frame but I'm not sure.
