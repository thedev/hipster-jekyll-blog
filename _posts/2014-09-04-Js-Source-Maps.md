---
layout: post
title: Javascript Source Mapping
comments: true
tags:
- Javascript, Developer Tools
---

	GET http://localhost:51794/Scripts/file.js.map 404 (Not Found)

Source mapping enables debugging of minified JS files using the browser developer tools.

The .map file is specified in the coresponding minified file.
	//# sourceMappingURL=file.map

If the map file does not exist developer tools will show an error but users will not see this.

To prevent errors simply remove sourceMappingURL line or create add the .map file. 

Details: [http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
