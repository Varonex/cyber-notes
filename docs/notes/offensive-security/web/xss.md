# Cross Site Scripting

--8<-- "components/legal-disclaimers.md:full"

## Introduction

Cross Site Scripting (XSS) is a technique that allows attackers to run arbitrary javascript on client browsers, thanks to different entryways.

```html
<input id="txt" type="text"/>
<input id="btn" type="button" value="click me :)"/>
<p id="res"></p>

<script>
	let txt = document.getElementById("txt");
	let btn = document.getElementById("btn");
	let res = document.getElementById("res");
	
	btn.addEventListener("click", () => {
		// XSS : anything will be interpreted here.
		res.innerHTML = txt.value;
	});
</script>
```

If we type `<img src=x onerror='alert()'/>`, an alert will be ran by the browser.

- **Reflected XSS** : Manipulation of entrypoints to see direct changes on the page as is (GET/POSt requests). The value is reinjected and not escaped.
- **Stored XSS** : Technique that uses reflected XSS, but the injection string is stored so that other clients can run it by visiting the infected page (forum posts, comments, usernames, ...).
- **DOM-based XSS** : Alters the page to run arbitrary javascript **directly** and **only** thanks to client legitimate code. Does not go through the webserver infrastructure, so cannot be analyzed by WAFs.

- **Source** : Entrypoint to run XSS (`location.href`, `location.search`, `document.write(c)` ...).
- **Payload** : Data to inject (`<img src=x onerror=.../>`, `<svg onload=...></>`, ...).
- **Sink** : FInal point where XSS will be ran (`eval()`, `setTimeout()`, `setInterval()`, ...).
  The two most common sinks are `alert()` and `print()`, as they can quickly demonstrate working XSS with no prior setup.

| Source                          | Parameters | Description                                       |
| ------------------------------- | ---------- | ------------------------------------------------- |
| `document.cookie`               | /          | Cookies of the current page.                      |
| `document.documentURI` : string | /          | = `location.href`.                                |
| `document.URL` : string         | /          | = `location.href`.                                |
| `document.referrer` : string    | /          | Contains the URL of the previous page.            |
| `location.hash` : string        | /          | Contains the `#` part of the url.                 |
| `location.href` : string        | /          | Contains the complete URL.                        |
| `location.pathname` : string    | /          | The path after the domain name.                   |
| `location.search` : string      | /          | Contains GET params of the query.                 |
| `window.name`                   | /          | Name of the window. Used for interdomain attacks. |

| Sink | Parameters | Description |
| ---- | ---------- | ----------- |
|      |            |             |
