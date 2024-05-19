# endlets
Bookmarklet to find endpoints easily with one click

![image](https://github.com/0dayCTF/endlets/assets/44453666/5429cb2a-804b-416f-bf4a-5db0aef11086)
This is a powerful JavaScript bookmarklet designed to find and display all endpoint URLs within a webpage by recursively fetching and processing scripts. This tool is useful for developers, security researchers, and bug hunters.

## Installation
Open your browser's bookmarks manager.
Create a new bookmark.
Set the bookmark's name to "Show Endpoints".
## Copy the following JavaScript code and set it as the URL for the bookmark:
```javascript
javascript:(function(){var regex=/(?<=(\"|\%27|\`))\/[a-zA-Z0-9_?&=\/\-\#\.]*(?=(\"|\'|\%60))/g;const results=new Set();function fetchAndProcess(url,depth){if(depth>3)return;fetch(url).then(response=>response.text()).then(scriptContent=>{var matches=scriptContent.matchAll(regex);for(let match of matches){let scriptUrl=match[0];results.add(scriptUrl);if(scriptUrl.startsWith("/")){scriptUrl=window.location.origin+scriptUrl;}fetchAndProcess(scriptUrl,depth+1);}}).catch(error=>{console.log("An error occurred while fetching script:",error);});}function processScripts(){var scripts=document.getElementsByTagName("script");for(var i=0;i<scripts.length;i++){var src=scripts[i].src;if(src){fetchAndProcess(src,0);}}}function processPageContent(){var pageContent=document.documentElement.outerHTML;var matches=pageContent.matchAll(regex);for(const match of matches){let scriptUrl=match[0];results.add(scriptUrl);if(scriptUrl.startsWith("/")){scriptUrl=window.location.origin+scriptUrl;}fetchAndProcess(scriptUrl,0);}}function writeResults(){results.forEach(result=>{document.write(result+"<br>");});}processScripts();processPageContent();setTimeout(writeResults,5000);})();
```
## Usage
Navigate to the webpage you want to analyze.
Click on the "Show Endpoints" bookmark.
Wait a few seconds for the bookmarklet to fetch and process the scripts.
The extracted endpoints will be displayed on the page.
This will also work on mobile browsers if you sync your bookmarks.
## How It Works
Regular Expression: Uses a regular expression to match endpoint URLs within scripts.
Recursive Fetching: Fetches script content recursively up to a specified depth (default is 3) to ensure deeper analysis.  
Result Display: Extracted URLs are collected in a Set to ensure uniqueness and then written to the document for easy viewing.
## Customization
Recursion Depth: Adjust the recursion depth by modifying the if(depth>3)return; line in the bookmarklet code.
Timeout Duration: Change the timeout duration by modifying the setTimeout(writeResults, 5000); line to allow more or less time for script fetching.
Limitations
CORS Restrictions: Some scripts may not be fetchable due to cross-origin resource sharing (CORS) policies.
Depth Limit: The recursion depth is limited to prevent infinite loops, but this can be adjusted if needed.
Browser Compatibility: The bookmarklet is designed to work in modern browsers with JavaScript enabled.
## Contributing
I welcome contributions! If you have suggestions for improvements or new features, please open an issue or submit a pull request :)
