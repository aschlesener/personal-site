---
title: "Writing a Chrome Extension"
date: 2018-11-11
categories:
- Coding
- Educational
tags:
- chrome
- extension
- security
- scripting
url: /posts/2018/11/writing-a-chrome-extension/
---

# Writing a Chrome Extension
### Or, How I Gamed My Favorite Subscription Box 💁

Like many millenials, I love [subscription boxes](/tags/adabox/). One particular seasonal box that I like is FabFitFun (FFF). They have special add-ons you can order with your quarterly box, but all the good ones sell out early. Their add-ons page is particularly annoying, as you have to scroll through many slow-loading pages to view all the products. Often, by the time I see a low-quantity item I want to add to my cart, it's sold out by the time I can click on it.

This most recent subscription period, I got fed up and decided to automate an easier way to get those sweet, sweet rare add-ons.

So I wrote my first Chrome extension!

## Preview

On normal sites, the extension is inactive. If you click on the extension button in the Chrome browser top bar, you get a generic list of options.
<p><img src="/img/posts/fff-expert-inactive.png"/></p>

But if you're on https://fabfitfun.com/add-ons/, the extension will activate. 

Now when you click on the extension button, you get a (very bad UI) list of options for adding all rare items to your cart, removing items from your cart, or scrolling to the bottom of the page (in case auto-scrolling stops working). 
<p><img src="/img/posts/fff-expert-buttons.png"/></p>


## Architecture
The code that actually does the thing I want (which is to automatically scroll to the bottom of the page in order to load all the items, and add all low-quantity ones to my cart, avoiding duplicates) is some pretty crappy jQuery/JavaScript. It's a lot of "find this div and do something with it". Functional, but ugly.

The interesting part (to me) is how the Chrome extension itself works.

### Manifest
The extension contains a JSON manifest file, that does some pretty important magic.

First, we declare a list of content scripts, which will load automatically whenever you visit the FFF add-ons site. Here, we're loading the minimized jQuery library, plus a custom script that scrolls to the bottom of the page, making sure all the add-on items are loaded.
```
    "content_scripts": [
        {
            "matches": [
                "https://fabfitfun.com/add-ons/*"
            ],
            "js": ["scripts/jquery-3.3.1.min.js", "scripts/scroll.js"]
        }
    ],
```

Next, we add a background script. Because content scripts are limited in scope to the page itself, we need another way to get the actual extension menu to show when we click the button, like in the screenshot above. The [background script](https://github.com/aschlesener/fff-expert/blob/master/scripts/background.js) is the first step to letting us do that - it basically says, if we're on the FFF add-on site, then activate the extension so that it shows the right menu when you click on it.

```    
    "background": {
        "scripts": ["scripts/background.js"]
    },
```

The Chrome terminology for what happens when you click on the extension button is called a "page action". So we need to declare that too. Note that this is the page action activated by the above background script.

```
    "page_action": {
        "default_popup": "popup.html"
    },
```

The [popup.html](https://github.com/aschlesener/fff-expert/blob/master/popup.html) renders the 3 buttons you saw in the screenshot above, when clicking on the active extension button.

The last important section of the manifest is declaring some permissions.
```
    "permissions": ["activeTab", "declarativeContent"],
```

`activeTab` has a good write-up on the Chrome developer site [here](https://developer.chrome.com/extensions/activeTab). Basically, it allows access to the specified site, and only when the user takes an action - i.e. clicking within the extension controls. It's a good way to help lock down to only what you need access to, and avoid the "this extension can read all your data" warning on installation.

`declarativeContent` ties in with our background.js script. It's what allows us to activate the extension and show the menu on click, when we're on the FFF add-on site.

### Scope
Permissions and scope become really important to understand when you want to run a script on the page by clicking a button in the extension popup menu.

As an example, let's take the "add low-stock items to cart" button in the popup. 

If you inspect the popup menu with Developer Tools, you'll notice that the sources just contains our popup.html, along with the popup.js script (which is included in the popup HTML). If we wanted to run our custom add-to-cart script directly, we couldn't - because that script, along with the FFF add-on page HTML which has the elements we want to target such as the add to cart button, is in a completely separate scope from our browser extension popup.

<p><img src="/img/posts/fff-popup-scope.png"/></p>

So, how do we run the add-to-cart script? We use the Chrome API to access it from within the active tab (which will be our add-on site, because we limited this extension to run only on that site). Clicking the "add to cart" button in the popup will execute the add-to-cart script, which gets loaded in with the FFF add-on site in a separate scope. Kinda weird to wrap your head around at first, but once you do it makes a lot of sense.

Here's the relevant code within popup.js:

```
let addToCart = document.getElementById('addToCart');

addToCart.onclick = function(element) {
    chrome.tabs.query({active: true, currentWindow: true}, function(tabs) {
        chrome.tabs.executeScript({
            file: 'scripts/add_cart.js'
        })
    });
};
```

## Security
We talked about ways we can limit scope when developing a Chrome extension, and how to get around existing scope limitations. But ultimately, you can run arbitrary scripts on the browser of whoever downloads your extension. 

[This Wired article](https://www.wired.com/story/chrome-extension-malware/) describes some of the problems with rampant malicious Chrome extensions. Users have had their browsing data, usernames, and passwords read. There are also extensions that have installed cryptocurrency mining scripts - cryptojacking in the browser.

Chrome does have automated security scanning when you upload an extension to the store, and they've [recently](https://thehackernews.com/2018/10/google-chrome-extensions-security.html) made security improvements to extensions. But ultimately, you should be damn careful with the arbitrary scripts you allow unknown developers to run in your browser whenever you install a Chrome extension .

## Resources
I found [this](https://robots.thoughtbot.com/how-to-make-a-chrome-extension) blog post to be helpful for getting started and understanding the concepts, like how message passing between content scripts and background scripts works. Google's [official docs](https://developer.chrome.com/extensions/getstarted) aren't bad either. 

You can find the source code and installation instructions for my fff-expert extension [here](https://github.com/aschlesener/fff-expert). 