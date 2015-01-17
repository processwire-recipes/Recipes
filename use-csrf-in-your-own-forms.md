title: Use CSRF in your own forms

----

version: 1.0.2

----

authors: Harmster

----

tags: forms, csrf, security

----

problem: 
If you do not wish to create forms via the ProcessWire API, but still aiming to use PW's form features, you can use its CRSF (Cross-Site Request Forgery) preventing features like so:

----

solution: 
First you need to create a token and a token name, you do that as following:

```PHP
$tokenName = $this->session->CSRF->getTokenName();
$tokenValue = $this->session->CSRF->getTokenValue();
```

Now what you want to do is create a hidden input field like this:

```PHP
$html .= '<input type="hidden" id="_post_token" name="' . $tokenName . '" value="' . $tokenValue . '"/>';
```

Now this will generate something that will look like this:

```PHP
<input type="hidden" id="_post_token" name="TOKEN1470842875" value="fe8ce9c1b9e6b9e361830df3525c49317a35332fbf626aa8793777a3b705824a">
```

You are done on the form side.
 
You can now go to the part where you are receiving the post. Then use:

```PHP
$session->CSRF->validate();
```

This will return true (1) on a valid request and an exception on a bad request. You can test this out to open up your Firebug/Chrome debug console and change the value of the textbox to something else.

Basicly what this does is set a session variable with a name (getTokenName) and gives it a hashed value. If a request has a token in it it has to have the same value or it is not send from the correct form.

----

resources:
* [Harmster's forum post](https://processwire.com/talk/topic/3779-use-csrf-in-your-own-forms/)
* [Wikipedia on CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery)
* Alternative: [Forum post: Create simple forms using api](https://processwire.com/talk/topic/2089-create-simple-forms-using-api/)
