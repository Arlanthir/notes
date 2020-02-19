# PHP

## Basics

Server-side behavior when serving webpages to clients
- .php files - code

## Minimal Working Example

index.php:
```php
<?php
    echo "Hello World";
?>
```

Equivalent shortcut:
```php
<?= "Hello World" ?>
```

## Conditionals & Loops
```php
<?php
if (condition) {
} else if (condition) {
} else {
}

switch ($variable) {
    case 1:
        break;
    case 2:
    case 3:
        break;
    default:
}

while (condition) {
    // Change condition to false or break;
}

for ($i = 0; $i < 10; ++$i) {
    // Do stuff, continue; or break;
}

foreach ($items as $index => $item) {
}

do {
} while (condition);
?>
```

## Constants
```php
<?php
define("CONSTANT", "value");
echo CONSTANT;
?>
```
## Variable Allocation

### Basic Types
```php
<?php
$a = 2;
$b = "three";
$b[0] == 't';
echo strlen($b); // 5
?>
```

### Arrays
```php
<?php
$myArray = array();
$myArray[] = 0;
$myArray[] = 1;

$myArray = array(1, 8, 45);
$myArray = array('one' => 'this', 'two' => 'that');
$myArray['three'] = 'other';
foreach ($myArray as $key => $val) {
}
echo count($myArray); // 3
unset($myArray['one']);
?>
```

### Check if variable is defined and not null
```php
<?php
isset(var)
isset($_GET['prop'])
?>
```

## Classes
```php
<?php
class SimpleClass {
    public $var;

    function __construct($v) {
        $this->var = $v;
    }

    public function displayVar() {
        echo $this->var;
    }
}

$sc = new SimpleClass("A class");
?>
```

## Regex
```php
$found = preg_match("/[0-9]+/", "hello 123 world"); // Will return 1
$found = preg_match("/^[0-9]+$/", "hello 123 world"); // Will return 0
```

## Redirect to another page

Before writing to out, do:
```php
<?php
header('Location: http://www.google.com');
exit;
?>
```

## Escape string containing <, >, etc to output it as text
```php
<?php
$text = htmlentities($text);
?>
```

## Import PHP file only once
```php
<?php
require_once('config.php');
?>
```

## Sending Mails
```php
<?php
$rule = array(
    "\r" => '',
    "\n" => '',
    "\t" => '',
    '"' => '',
    ',' => '',
    '<' => '',
    '>' => ''
);

$bodyRule = array(
    "\r" => '',
    "\t" => '',
    '"' => '',
    ',' => '',
    '<' => '',
    '>' => ''
);

$name = strtr($name, $rule);
$email = strtr($email, $rule);
$content = strtr($conteudo, $bodyRule);

$subject = iconv_mime_encode('', 'This is my Subject', array("input-charset" => "UTF-8", "output-charset" => "UTF-8"));
$subject = substr($subject, 2);

$message = "New contact received: \r\n \r\nNome: $name\r\nEmail: $email\r\n" .
"Message: $content" .
"\r\n \r\nThank you.";

$from = iconv_mime_encode('', "Sender Name",
array("input-charset" => "UTF-8", "output-charset" => "UTF-8"));
$from = substr($from, 2) . " <sender@server.com>";

// OR Content-Type: text/html
$headers = "Content-Type: text/plain; charset=utf-8\r\n" . "Bcc: ". $bcc . "\r\n" . "From: $from\r\n" . "Reply-To: $from\r\n" . "\r\n";

if (!mail($to, $subject, $message, $headers)) {
    // Could not send mail
}
?>
```
**Note**: Nowadays apparently \r\n produces errors in mail clients and servers, some people advise using simply \n.

To transfer attachments, use content-type multipart/mixed and boundaries.

## Incomplete Class objects in $_SESSION
```php
<?php
Either include the class definition BEFORE session_start()
Or retrieve the object by using
unserialize(serialize($_SESSION['object']))
?>
```

## Dates
```php
<?php
$now = date("Y-m-d H:i:s");
$date = date("Y-m-d H:i:s", $someTimestamp);
$dateTime = DateTime("2010-12-08");
$date->modify('+36 months') // php 5.2
$date->add(new DateInterval('P36M')); // php 5.3
echo $dateTime->format("Y");
echo $dateTime->format("U"); // php 5.2
echo $date->getTimestamp(); // php 5.3
$diff = strtotime('2010-12-08 16:12:12')-time();
?>
```

## File Upload
```html
<form action="upload.php" method="post" enctype="multipart/form-data">
</form
```

```php
<?php
$_FILES["file"]["error"] // must be 0; 4 if no file sent
http://www.php.net/manual/en/features.file-upload.errors.php
$fileSize = $_FILES["file"]['size'];
$fileName = $_FILES["file"]['name'];
$exifType = exif_imagetype($_FILES["file"]['tmp_name']); // false if not image
?>
```
if uploading multiple files: `$_FILES["file"]["error"][$i]` etc  
(don't forget to set name="file[]" on the input if multiple)

## Take EXIF Orientation into consideration for images
```php
<?php
$image = imagecreatefromjpeg($filename);
$exif = exif_read_data($filename);
if($exif && !empty($exif['Orientation'])) {
    switch($exif['Orientation']) {
        case 8:
            $image = imagerotate($image,90,0);
            break;
        case 3:
            $image = imagerotate($image,180,0);
            break;
        case 6:
            $image = imagerotate($image,-90,0);
            break;
        default:
    }
}
?>
```

## Important .htaccess properties
```
php_value memory_limit 512M
php_value post_max_size 512M
php_value upload_max_filesize 4M
php_value max_file_uploads 100
php_value max_execution_time 3600
```

## Output buffers - Store output text in variables
```php
<?php ob_start(); ?>
<div>This will be stored in a variable</div>
<?php $v = ob_get_clean(); ?>
```

## CURL - Make HTTP Requests
```php
<?php
$ch = curl_init($url);
curl_setopt_array($ch, array(
     CURLOPT_RETURNTRANSFER => 1,
     CURLOPT_HEADER => 0,
     CURLOPT_SSL_VERIFYPEER => false,
     CURLOPT_FOLLOWLOCATION => true,
     CURLOPT_POSTREDIR => CURL_REDIR_POST_ALL
));
$result = curl_exec($ch);
$httpcode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);
?>
```

## PHP + MySQL
`mysql_set_charset ('utf8');`

## Display errors
```
error_reporting(E_ALL);
ini_set('display_errors', 1);
```
