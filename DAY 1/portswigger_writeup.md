# PortSwigger Lab — [Lab Title]
**Date:** 2025-10-05  
**Difficulty:** Easy  
**Time spent:** 1h 

---

## Objective
Exploit the DOM XSS vulnerability in the search section of the website.

---

## Tools
- Browser (incognito)
- Burp Suite community edition

---

## Recon
- Source and Data Flow Observation:

1)The user-controllable data was sourced from the location.search property (the URL query string).

2)This data was then passed to the document.write function, which acted as the primary sink.

HTML Context Identification:

1)The document.write call placed the raw user input directly into an HTML context, specifically within the img src attribute.

2)This lack of proper output encoding meant the input was treated as live HTML/JavaScript code rather than mere data for the attribute.

3)By using svg onload paylod starting with "> it terminated the input and added a new html in client side server so that when the browser rendered in onload request it processed the alert request.

- HTML Context Breakout:

1)The payload initiated with the characters "> to terminate the existing HTML structure.

2)The double quote (") successfully closed the vulnerable img src attribute.

3)The angle bracket (>) then closed the entire img tag, freeing the rest of the input to be parsed as new HTML.

Payload Injection and Trigger:

1)The new, malicious HTML element <svg onload=alert(1)> was successfully injected into the client-side server (the rendered DOM).

2)The exploit was triggered by the onload event handler, which automatically processed the alert(1) request as soon as the <svg> element was rendered by the browser.

---

## Exploit (step-by-step)
1. Step one: I changed the search request.
2. Step two: payload used is "><syg onload = alert(1)>
3. Step three: Port swigger displayed solved message.

---

## Proof
- Screenshot: `screenshots/ps_DOM XSS in document.write sink using source location.search_proof.png`  
- Short explanation : The sceenshot shows that my payload was taken in the input of img src and then alert had been generated which then solved the lab.

---

## Remediation
- [ use element.textContent or element.innerText instead of .innerHtml ] Fix idea 1  
- [ use XSS sanitizer which are librarries dedicated to sanitize inputs like svg etc.] Fix idea 2  

---

## Notes / Takeaways
- Commands, Burp features used
- What you learned or struggled with
