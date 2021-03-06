  <a href="https://travis-ci.org/Sightengine/client-php">
   <img src="https://travis-ci.org/Sightengine/client-php.svg?branch=master">
  </a>

# About

Use the Sightengine Moderation API to instantly moderate images and videos. See http://sightengine.com for more information.

Before starting, please make sure you have created an account on https://sightengine.com

# Install

```php
composer require sightengine/client-php
```

# Initialize the client

You will need your API USER and API SECRET to initialize the client. You can find both of them on your Sightengine account.
```php
use Sightengine\SightenginClient;

$client = new SightengineClient('{api_user}', '{api_secret}');
```

# Moderate an image

The API accepts both standard still images: JPEG, PNG, WEBP etc. and multi-frame GIF images.

Several moderation engines are available for you to choose from (nudity detection, inappropriate content detection etc...). Please refer to the documentation for more.

## Moderate an image through a public URL:

```php
# Detect nudity in an image

$output = $client->check(['nudity'])->image('http://img09.deviantart.net/2bd0/i/2009/276/c/9/magic_forrest_wallpaper_by_goergen.jpg')

# Detect nudity, weapons, alcohol, drugs, likely fruadulant users, celebrities and faces in an image, along with image properties and type
$output = $client->check(['nudity', 'type', 'properties', 'wad', 'face', 'scam', 'celebrity'])->image('http://img09.deviantart.net/2bd0/i/2009/276/c/9/magic_forrest_wallpaper_by_goergen.jpg')
```

## Moderate a local image:
```php
# Detect nudity in an image
$output = $client->check(['nudity'])->image('/full/path/to/image.jpg')

# Detect nudity, weapons, alcohol, drugs and faces in an image, along with image properties and type
$output = $client->check(['nudity', 'type', 'properties', 'wad', 'face'])->image('/full/path/to/image.jpg')
```

# Video and Stream Moderation
The first step to detect nudity in a video stream is to submit the video stream to the API.

```php
$client->check(['nudity'])->video('http://www.quirksmode.org/html5/videos/big_buck_bunny.webm', 'https://example.com/yourcallback')
```

# Feedback
In order to report a misclassification, you need to report the image that was misclassified, the model that was run on this image (models are nudity, face, type, wad), and the correct class of the image.

For each model, there are different classes that you may report. Here are the details:

The nudity model has 3 classes:
 * raw: corresponding to raw nudity
 * partial: corresponding to partial nudity
 * safe: corresponding to no nudity

The face model has 3 classes:
 * none
 * single
 * multiple
 
The type model has 2 classes:
* photo
* illustration

The wad model has 3 classes:
* no-weapons
* weapons
* no-alcohol
* alcohol
* no-drugs
* drugs
 
```php
$client->feedback(model, class,image)
```
Example of feedback on a local image:
```php
$client->feedback("nudity","safe", "/full/path/to/image.jpg")
$client->feedback("type","illustration", "/full/path/to/image.jpg")
$client->feedback("nudity","raw", "/full/path/to/image.jpg")
```
Example of feedback through a public URL::
```php
$client->feedback("nudity","safe", "http://img09.deviantart.net/2bd0/i/2009/276/c/9/magic_forrest_wallpaper_by_goergen.jpg")
$client->feedback("type","illustration", "http://img09.deviantart.net/2bd0/i/2009/276/c/9/magic_forrest_wallpaper_by_goergen.jpg")
$client->feedback("nudity","raw", "http://img09.deviantart.net/2bd0/i/2009/276/c/9/magic_forrest_wallpaper_by_goergen.jpg")
```
