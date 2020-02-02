# Summary

Demonstration of userScripts.register. Runs at document-start, correct error messages, GM_getValue/GM_setValue/exportFunction/cloneInto support, limited for one userscript currently that has to be re-registered on Firefox start

# Description

Workaround until the userscript API is widely spread and usable with unsafeWindow & common, proper user script managern. This should be an extended proof of concept. Don't use this productively or for mission critical purpose. Install the addon & navigate to it's settings.

The default code included with the example allows you to load a userScript which will "eat" the content of pages matching the Hosts entry. Make any changes you want to make before clicking the Register script button at the bottom of the panel.

In the attached screenshot, the extension will "eat" the content of pages whose domain name ends in .org. This is the default behavior for this extension.

Nothing will happen until you click the Register script button. The button implements the user script according to the settings on this dialog. That means that you can experiment with the behavior of the script without having to implement an extensions yourself.

# Additional info

General API & this example is described on https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/userScripts/Working_with_userScripts (unfortunately the described version & screenshots there belong to this version here & are not part of the official webextension-examples from MDN).

Based on https://bugzilla.mozilla.org/attachment.cgi?id=9033244 within https://bugzilla.mozilla.org/show_bug.cgi?id=1516356#c1 & https://github.com/mdn/webextensions-examples/tree/master/user-script.

Maintained on Github:
https://github.com/kekkc/webextensions-examples/blob/master/user-script/Release/user-script.zip
--> Use FF Developer > set xpinstall.signatures.required to false in about:config > load ZIP. Userscript can be reigstered from the extension options page.
-->Alternatively use this AMO version here.

Limitation:
Only a copied proof of concept. Handles one script, doesn't support local "@require scripts" & has to be re-registered on FF restart.

Additional links:
https://github.com/greasemonkey/greasemonkey/issues/2663
https://github.com/erosman/support/issues/103#issuecomment-565829719

# User script

https://addons.mozilla.org/de/firefox/addon/userscripts-poc/

This extension demonstrates the `browser.userScripts.register()` API, which enables an extension to register URL-matching scripts at runtime which runs in isolated sandboxes.

This extension adds a browser action that shows a popup. The popup lets you specify:

* some code that comprises your content script
* one or more [match patterns](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Match_patterns), comma-separated. The content script will only be loaded into pages whose URLs match these patterns.

Once these are set up you can register the user script by clicking "Register script". The extension will then register a "User Script" with the given properties by calling `browser.userScripts.register()`.

To keep things simple, you can only have one script registered at any time: if you click "Register script" again, the old script is unregistered. To do this, the extension keeps a reference to the `RegisteredUserScript` object returned from `browser.userScripts.register()`: this object provides the `unregister()` method.

Note that the extension uses a background script to register the user scripts and to keep a reference to the `RegisteredUserScript` object. If it did not do this, then as soon as the popup window closed, the `RegisteredUserScript` would go out of scope and be destroyed, and the browser would then unregister the content script as part of cleanup.

## Default settings

The popup is initialized with some default values for the pattern and the code:

* the pattern `*://*.org/*`, which will load the script into any HTTP or HTTPS pages on a `.org` domain.
* the code `document.body.innerHTML = '<h1>This page has been eaten<h1>'`

To try the extension out quickly, just click "Register script" with these defaults, and load http://example.org/ or 
https://www.mozilla.org/. Then try changing the pattern or the code, and reloading these or related pages.
