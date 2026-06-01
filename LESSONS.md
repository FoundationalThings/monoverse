# Lessons Learned: Doing iOS 9.3.5 Web Development in 2026

_Thoughts and lessons learned by me, Donna J. Harris._

## Overview

The following lessons have been discovered through hand debugging and testing, commenting out code, viewing in a modern browser and comparing with the iOS 9.3.5 Safari browser on an iPad 3.

These are my own notes about developing for legacy environments while developing MonoVerse. No AI was involved in generating this documentation, although AI has generated the initial POC code and has assisted with some troubleshooting and pattern checking.

### LESSON 0: Test locally
<sup>Monday, June 1, 2026</sup>

Most important for sanity! Test locally first.

iOS 9.3.5 development needs a lot of manually debugging and troubleshooting. Save yourself the hassle and run a small HTTP Python server locally on your network and connect your legacy (and other!) test devices to it while working through these things.

How-To Link: **[TEST_LOCALLY.md](TEST_LOCALLY.md)**

### LESSON 1: Trailing commas are bad
<sup>Monday, June 1, 2026</sup>

iOS 9.3.5 does not like trailing commas very much. Apparently this little thing (which is added by extensions like Prettier -- see Lesson 1a) totally makes iOS 9.3.5 Safari die.

According to Claude AI, the JavaScript engine doesn't support trailing commas in function calls. That's an ES2017 feature. So, the old WebKit simply chokes on it silently.

**DO THIS:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail'
        );
```

**NOT THIS:**
```
        addResult(
          'rawBooks.length: ' + rawBooks.length,
          rawBooks.length > 0 ? 'pass' : 'fail',
        );
```

_Note: the **NOT THIS** example results in the entire object not being rendered on the page, even if some code is good._

I experienced several red herrings, such as using ternary operators and inline concatentation used within function calls, etc., but all of the "fixes" I experienced also included removing the trailing commas. But simply removing the trailing commas from all function calls allowed the initial POC index page to load.


### LESSON 1a: Don't use modern code formatters for legacy code
<sup>Monday, June 1, 2026</sup>

Using Prettier in VS Code was actually introducing/re-introducing problems because it was looking to achieve a standard too modern for iOS 9.3.5

Don't use these tools for this kind of project work.

Beware of anything that could alter your code to the wrong standard.
