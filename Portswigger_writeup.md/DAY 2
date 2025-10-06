Date: 6 OCT 2025
Lab : DOM XSS in jQuery anchor href attribute sink using location.search source

Summary: reflected returnPath injected into <a href> → javascript: payload executes on click.

Steps (short):
	1.	Open ...?returnPath=/abc123 → Inspect → find <a href="...">Back</a>.
	2.	Change to ?returnPath=javascript:alert(document.cookie) → reload → click Back → alert shows cookies.

Payload: javascript:alert(document.cookie)

Root cause: unsanitized user input placed inside href (DOM XSS).

Fixes: block javascript: scheme, validate/whitelist return paths, encode output.
