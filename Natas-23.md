# Natas-23

```php
<?php
if(array_key_exists("passwd",$_REQUEST)){
if(strstr($_REQUEST["passwd"],"iloveyou") && ($_REQUEST["passwd"] > 10 )){
echo "<br>The credentials for the next level are:<br>";
echo "<pre>Username: natas24 Password: <censored></pre>";
}
else{
echo "<br>Wrong!<br>";
}
}
// morla / 10111
?>
```

Its checking if $_REQUEST[”passwd”] has the substring “iloveyou” and it should be greater than 10 . At first it looks impossible since a string cant be > 10 . But considering type juggling in PHP we can make this condition evaluate to true . if we give 11iloveyou as the input . This string will be  converted to integer in the condition so 11 > 10 is true . So we will get the credentials .

```
The credentials for the next level are:
Username: natas24 Password: OsRmXFguozKpTZZ5X14zNO43379LZveg
```