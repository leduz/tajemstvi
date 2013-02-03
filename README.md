tajemstvi
=========
<?php
02.
// Údaje z https://www.facebook.com/developers/
03.
define('APP_ID', 'xxx');
04.
define('API_KEY', 'xxx');
05.
define('APP_SECRET', 'xxx');
06.
define('CANVAS_PAGE', 'http://apps.facebook.com/app-namespace/');
07.
define('CANVAS_URL', 'http://localhost/zdrojak-hello-world/');
08.
 
09.
// Facebook knihovna z Github.com
10.
require_once 'libs/facebook.php';
11.
 
12.
// Vytvoříme instanci Facebook knihovny
13.
$facebook = new Facebook( array('appId' => APP_ID, 'secret' => APP_SECRET, ));
14.
 
15.
// Získáme ID přihlášeného uživatele
16.
$user = $facebook->getUser();
17.
 
18.
// Je uživatel přihlášený na Facebooku? resp. máme session?
19.
if(isset($user)) {
20.
try {
21.
// Zkusíme získat jeho profilová data (na uživatelova data nepotřebujeme extended_permission)
22.
$user_profile = $facebook->api('/me');
23.
} catch (FacebookApiException $e) {
24.
// Vypíšeme text Exception
25.
echo "<strong>" . $e->getMessage() . "</strong>";
26.
$user = NULL;
27.
}
28.
}
29.
 
30.
// Uživatel se odhlásil, odstranil aplikaci...
31.
if(!is_null($user)) {
32.
// Získáme logout url
33.
$logoutUrl = $facebook->getLogoutUrl();
34.
} else {
35.
// Získáme přihlašovací url
36.
$loginUrl = $facebook->getLoginUrl();
37.
}
38.
?>
39.
 
40.
<!doctype html>
41.
<html>
42.
 
43.
<head>
44.
<meta charset="utf-8">
45.
<title>Zdroják.cz - Hello world aplikace</title>
46.
</head>
47.
 
48.
<body>
49.
<p>
50.
Ahoj
51.
<strong><?php if(!is_null($user)) echo $user_profile["name"]; ?></strong>, jak je? :-)
52.
</p>
53.
 
54.
<h2>Přihlásit / odhlásit?</h2>
55.
<p>
56.
<?php if ($user): ?>
57.
<a href="<?php echo $logoutUrl;?>">Odhlásit se z Facebooku!</a>
58.
<?php else:?>
59.
<a href="<?php echo $loginUrl;?>">Přihlásit se na Facebook!</a>
60.
<?php endif?>
61.
</p>
62.
</body>
63.
</html>
