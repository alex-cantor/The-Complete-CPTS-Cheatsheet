# XSS

## XSS Testing Payloads

- If after typing this into an input field, you receive the popup, the site is vulnerable to XSS
  - This can be confirmed by looking at the source code (CTRL+U/I)
  - `<script>alert(window.origin)</script>`

- The `alert()` function may be blocked. Some alternatives include:
  - `<plaintext>`: This will stop rendering the HTML after it is inputted
  - `<script>print()</script>`: Thi will pop up a browser print dialog

 **To determine if the XSS vulnerability is persistent, we can refresh the page.**`

- **DOM Attack**: `<img src="" onerror=alert(window.origin)>`

## XSS Discovery

### Automated

We have many options, such as XSS Strike, Brute XSS, and XSSer

#### XSS Strike
```bash
$ git clone https://github.com/s0md3v/XSStrike.git
$ cd XSStrike
$ pip install -r requirements.txt
$ python xsstrike.py
$ python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"
```

### Manual

We have a few options, such as PayloadAllTheThings and PayloadBox
