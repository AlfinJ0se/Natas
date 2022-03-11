# Natas-21

There are 2 domains in this level [**http://natas21-experimenter.natas.labs.overthewire.org](http://natas21-experimenter.natas.labs.overthewire.org/) and [http://natas21.natas.labs.overthewire.org](http://natas21.natas.labs.overthewire.org/) .**

## **Source : [http://natas21.natas.labs.overthewire.org](http://natas21.natas.labs.overthewire.org/)**

```php
function print_credentials() { /* {{{ */
    if($_SESSION and array_key_exists("admin", $_SESSION) and $_SESSION["admin"] == 1) {
    print "You are an admin. The credentials for the next level are:<br>";
    print "<pre>Username: natas22\n";
    print "Password: <censored></pre>";
    } else {
    print "You are logged in as a regular user. Login as an admin to retrieve credentials for natas22.";
    }
}
session_start();
print_credentials();
```

It checks if we are a admin or not basically depending on our $_SESSION super global . If we had a $_SESSION[”admin”] and if it was equal to 1 the condition would be satisfied and we would be an admin .

## Source : [**http://natas21-experimenter.natas.labs.overthewire.org**](http://natas21-experimenter.natas.labs.overthewire.org/)

![Untitled](Natas-21%207df97/Untitled.png)

```php
if(array_key_exists("submit", $_REQUEST)) {
    foreach($_REQUEST as $key => $val) {
    $_SESSION[$key] = $val;
    }
}
```

It is taking every key and value in $_REQUEST superglobal and assigning it into $_SESSION superglobal without checking if the key is align,fontsize or bgcolor . Then it styles the web page according to this .

```php

$style = "background-color: ".$_SESSION["bgcolor"]."; text-align: ".$_SESSION["align"]."; font-size: ".$_SESSION["fontsize"].";";
$example = "<div style='$style'>Hello world!</div>";
```
We can basically send a post request with align=center&fontsize=100%&bgcolor=blue&**admin=1** and it will be assigned to $_SESSION[”admin”]=1 . So we will have a key : admin and its value : 1 .So the codintion on print_credentials will be satisfied and we can get the password for the next Level .

You are an admin. The credentials for the next level are:
```
Username: natas22
Password: chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ
```
