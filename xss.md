# XSS

## XSS Testing Payloads

- If after typing this into an input field, you receive the popup, the site is vulnerable to XSS
  - This can be confirmed by looking at the source code (CTRL+U/I)
  - `<script>alert(window.origin)</script>`

- The `alter()` function may be blocked. Some alternatives include:
  - `<plaintext>`: This will stop rendering the HTML after it is inputted
  - `<script>print()</script>`: Thi will pop up a browser print dialog

 **To determine if the XSS vulnerability is persistent, we can refresh the page.**`
