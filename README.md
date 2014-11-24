# JavaScript SDK for Beachfront.iO #

## Overview  ##
This document details the process of integrating the Beachfront.iO Javascript AD SDK into an HTML5 Web Page.

## Requirements  ##
The javascript SDK can be used on mobile browsers with HTML5 video element support.

## Getting Started  ##
1.  Include the JavaScript SDK library on your html page.
    It can be included inside `<head>` tag
    ``` html
    <!DOCTYPE html>
    <html>
        <head>
            <title>Title of the document</title>
            <script type="text/javascript" src="http://platform.beachfront.io/bfio_ad.min.js"></script>
        </head>
        <body>
            The content of the document......
        </body>
    </html>
    ```
    or directly inside `<body>` tag
    ``` html
    <html>
        <head>
            <title>Title of the document</title>
        </head>
        <body>
            The content of the document......
            <script type="text/javascript">
                document.write('<script language="JavaScript" src="http://platform.beachfront.io/bfio_ad.min.js" type="text/javascript"><\/scr' + 'ipt>');
            </script>
        </body>
    </html>
    ```

2. Put a `<div>` container on your page. Video Ad will be played inside this container.

## Operations  ##
In order to call the SDK methods, you must first get the reference to the bfiomobileweb object you wish to control.
Be sure that you are using a separate bfiomobileweb instance for each Ad container.

```var bfio_ad = new bfiomobileweb();```

## Functions ##
The following subsections lists the functions that the JavaScript SDK supports.

### Ad ###
This method can be used for common cases. After execution, div container will be filled with an Ad.
After the Ad video completes, the div container content will be removed.
```html
var div_container = document.getElementById('div_container);
var ad_params = {
    app_id:'6332602c-2423-477a-cff7-2022e89426aa',
    ad_init_id:'2ca35c9c-0152-425e-cae0-6f399f96a032',
    autoplay: true,
    overlay_class: 'ad_overlay',
    overlay_width: '320px',
    overlay_height: '200px',
    poster: 'http://images.mefeedia.com/entries/66256474/video_250.png'
}
bfio_ad.Ad(div_container, ad_params);
```
####div_container
reference to an Element object or Element ID.

####ad_params
* `app_id` - (mandatory) beachfront.io app id.
* `ad_init_id` - (mandatory) beachfront.io ad unit id.
* `autoplay` - (optional) ad unit auto play (true/false), by default is true .
* `overlay_class` - (optional) CSS class name for overlay div, by default {position:absolute;background:black;display:block;top:0px;left:0px}.
* `overlay_width` - (optional) by default will be auto detected
* `overlay_height` - (optional) by default will be auto detected.
* `poster` - (optional) video thumb url.


### AdStart ###
In case the video can't be auto played or auto play is false, it's possible to start playing the ad video with this method
```html
bfio_ad.AdStart();
```
### AdRemove ###
Cleanup div container content. Usually div container content is removed automatically.

```html
bfio_ad.AdRemove();
```
### addListener ###
add event listeners
```html
bfio_ad.addListener(event_name, event_handler);
```
####Options
* `event_name` - name of the event. As of now, we support the following events:
         AdInitialized, AdVideoReadyToPlay, AdVideoStart, AdPlaying, AdRemainingTimeChange, AdVideoFirstQuartile, AdVideoMidpoint, AdVideoThirdQuartile, AdVideoComplete, AdClickThru, AdError
* `event_handler` - function that will be called when te event is fired

### removeListener ###
remove event listeners
```html
bfio_ad.removeListener(event_name, event_handler);
```
####Options
* `event_name` - name of the event.
* `event_handler` - function to remove.

## Events ##
###AdlInitialized
Ad is available and all container div elements and events were initialized

###AdClickThru
The 'AdClickThru' event will be fired when the click-through html is clicked. (Ad Video was clicked)

###AdVideoStart
The 'AdVideoStart' event will be fired when the video starts playing.
This is fired after receiving the callback for  the 'timeupdate' event of the HTML5 video element for the very first time.

###AdPlaying
The 'AdPlaying' event will be fired after receiving the callback for the 'play' event of the HTML5 video element.

###AdVideoFirstQuartile
The 'AdVideoFirstQuartile' event will be fired when 25% of the video is completed.

###AdVideoMidpoint
The 'AdVideoFirstQuartile' event will be fired when 50% of the video is completed.

###AdVideoThirdQuartile
The 'AdVideoFirstQuartile' event will be fired when 75% of the video is completed.

###AdVideoComplete
The 'AdVideoComplete' event will be fired when 100% of the video is completed.

###AdError
The 'AdError' event will be fired when ad fails to load or encounters an error.

###AdRemainingTimeChange
The 'AdRemainingTimeChange' event will be fired when the 'timeupdate' event of the HTML5 video element is triggered.

###AdVideoReadyToPlay
The 'AdVideoReadyToPlay' event will be fired after 'canplay' or 'canplaythrough' event of the HTML5 video element.

##Ad Examples##
### Basic Ad example ###
```html
<div style="position:relative;display:block;width:320px; height: 200px; top: 0px; left:0px;" id="video_ad"></div>
<input type="button" value="Start Ad" id="start_ad">
<script type="text/javascript">
    document.write('<script language="JavaScript" src="http://platform.beachfront.io/bfio_ad.min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
<script type="text/javascript">
    var video_ad = new bfiomobileweb();
    if (video_ad) {
        var ad_params = {
            app_id:'6332602c-2423-477a-cff7-2022e89426aa',
            ad_init_id:'2ca35c9c-0152-425e-cae0-6f399f96a032',
            autoplay: true,
            poster: 'http://images.mefeedia.com/entries/66256474/video_250.png',
            overlay_width: '320px',
            overlay_height: '200px'
        }
        video_ad.addListener('AdInitialized', function(arg){
            // event fires when Ad is available and all container div elements and events were initialized
            // arg = {type: 'event name',
            //        overlay: 'reference to an overlay div object',
            //        video_ad: 'reference to video object'};
        });
        video_ad.addListener('AdVideoReadyToPlay', function(){
            // event fires when Ad Video is ready to play
        });
        video_ad.addListener('AdClickThru', function(){
            // event fires when the click-through html is clicked
        });
        video_ad.addListener('AdRemainingTimeChange', function(arg){
            // arg = {type: 'event name',
            //        currentTime: 'currentTime value from video tag timeupdate event'};
        });
        video_ad.addListener('AdVideoComplete', function(){
            // event fires when the Ad video is completed, in some cases this event will fired when video can't play
        });
        video_ad.addListener('AdError', function(errorInfo){
            // arg = {errorCode: 'error code',
            //        errorMsg: 'error description'};
        });


        video_ad.Ad('video_ad', ad_params);
        document.getElementById('start_ad').addEventListener('click', video_ad.AdStart);
    }
</script>
```
