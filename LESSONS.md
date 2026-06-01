# Lessons Learned: Doing iOS 9.3.5 Web Development in 2026

_Thoughts and lessons learned by me, Donna J. Harris._

Note: No AI was involved in generating this documentation. Some AI was used to troubleshoot.

## Overview

The following lessons have been discovered through hand debugging and testing, commenting out code, viewing in a modern browser and comparing with the iOS 9.3.5 Safari browser on an iPad 3.

## Testing Locally

<sup>Monday, June 1, 2026</sup>

I did my first iOS 9.3.5 web page loading test after posting the MonoVerse POC website live at **[bible.foundationalthings.com](https://bible.foundationalthings.com)** (hosted by GitHub Pages)

But, once I realized there would be lots of back and forth necessary to debug why the site wasn't loading properly, it finally occured to me that I didn't need to commit test files to my GitHub repository and have GitHub Pages run an action to publish the site for every single change. Nope.... not needed at all.

Instead, because all my devices are on my local Wi-Fi network, I spun up a local HTTP server on my development laptop (a relatively modern environment) using Mac's Terminal:

```
python3 -m http.server 8080
```

Then I had to figure out what the IP address was:

```
ipconfig getifaddr en0
```

And finally, I put it all together and loaded the local page in my iPad 3's browser, something like this: `192.168.100.83:8080` which loads the main app page, or go directly to whatever test page I'm working on, like:

```
192.168.100.83:8080/tests/test5.html
```

Not only is this quicker, because I don't have to upload and fiddle with commits, it is actually INSTANT because I am not waiting for GitHub Pages to do its thing. (It may also save me money in the long run, if I ever go over the free limits of using actions.)

## `XMLHttpRequest` responses seem to have these problems...

When the "**NO**" examples are included in my "simple" HTML/CSS/JS code, the area of the page relying on the `XMLHttpRequest` will not render.

### LESSON 1: Trailing commas are bad

<sup>Monday, June 1, 2026</sup>

iOS 9.3.5 does not like trailing commas. Apparently this little thing (which is added by extensions like Prettier) totally makes iOS 9.3.5 Safari choke silently.

Red herrings were ternary operators and inline concatentation used within function calls, etc., but these "fixes" were removing the trailing comma issues. (Where, Prettier is adding them back, even if I manually removed them.)


**YES:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail'
        );
```

**NO:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail',
        );
```

#### LESSON 1a: Don't use modern code formatters for legacy code

Using Prettier in VS Code was actually introducing/re-introducing problems because it was looking to achieve a standard too modern for iOS 9.3.5

Don't use these tools for this kind of project work. Beware of anything that would alter your code to the wrong standard.


