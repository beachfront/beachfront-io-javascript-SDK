# JavaScript SDK for Beachfront.iO #

## Overview  ##
This document details the process of integrating the Beachfront.iO Javascript AD SDK into an HTML5 Web Page.

## Requirements  ##
The javascript SDK can be used on mobile browsers with HTML5 video element support.

## Getting Started  ##
1.  Include the JavaScript SDK library in your html page.
    It can be included in page `<head>` tag
    ``` html
    <!DOCTYPE html>
    <html>
        <head>
            <title>Title of the document</title>
            <script type="text/javascript" src="http://beachfront.io/preroll/bfio_ad_min.js"></script>
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
                document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_ad_min.js" type="text/javascript"><\/scr' + 'ipt>');
            </script>
        </body>
    </html>
    ```

2. Put a `<div>` container inside your page. Video Ad will be played inside this container.

## Operations  ##
In order to call the SDK methods, you must first get a reference to the bfiomobileweb object you wish to control.
Be sure that you are using a separate bfiomobileweb instance for each Ad container.

```var bfio_ad = new bfiomobileweb();```

## Functions ##
The following subsections list the functions that the JavaScript SDK supports.

### Ad ###
This method can be used for common cases. After execution, div container will be filled with an Ad.
After the Ad video completes, the div container content will be removed.
```html
var div_container = document.getElementById('div_container);
var ad_params = {
    app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',
    ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba',
    autoplay: true,
    overlay_class: 'ad_overlay',
    overlay_width: '320px',
    overlay_height: '200px',
    poster: 'http://beachfront.io/templates/images/pics/ad_Progressive.jpg'
}
bfio_ad.Ad(div_container, ad_params);
```
####div_container
reference to an Element object or Element ID.

####ad_params
* `app_id` - (mandatory) beachfront.io app id. 
* `ad_init_id` - (mandatory) beachfront.io ad unit id.
* `autoplay` - (optional) ad unit autoplay (true/false), by default is true .
* `overlay_class` - (optional) CSS class name for overlay div, by default {position:absolute;background:black;display:block;top:0px;left:0px}.
* `overlay_width` - (optional) by default will be autodetected
* `overlay_height` - (optional) by default will be autodetected.
* `poster` - (optional) video thumb url.
* `zipcode` - (optional) users zipcode.

### AdStart ###
In case the video can't be autoplayed or autoplay is false, it's possible to start playing the ad video with this method
```html
bfio_ad.AdStart();
```
### AdRemove ###
Cleanup div container content. Usually div container content is removed automatically.

```html
bfio_ad.AdRemove();
```
### Preroll ###
```html
bfio_ad.Preroll(div_container, ad_params);
```
####div_container
reference to an Element object or Element ID.

####ad_params
* `app_id` - (mandatory) beachfront.io app id. 
* `ad_init_id` - (mandatory) beachfront.io ad unit id.
* `preroll_mode` - (mandatory) depends on container div content ('video', 'link', 'action').
* `autoplay` - (optional) ad unit autoplay (true/false), by default is true .
* `overlay_class` - (optional) CSS class name for overlay div, by default {position:absolute;background:black;display:block;top:0px;left:0px}.
* `overlay_width` - (optional) by default will be autodetected
* `overlay_height` - (optional) by default will be autodetected.
* `callback` - (optional) callback function for preroll_mode = 'action'. Fires when ad complete. For the rest preroll_mode is not required.
* `poster` - (optional) video thumb url.
* `zipcode` - (optional) users zipcode.

###PrerollStart ###
In case video can't be autoplayed or autoplay is false, it's possible to start playing ad video with this method
```html
bfio_ad.PrerollStart();
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
    document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_ad_min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
<script type="text/javascript">
    var video_ad = new bfiomobileweb();
    if (video_ad) {
        var ad_params = {
            app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',
            ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba',
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
            // event fires when Ad Video  is ready to play
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

### `<VIDEO>` tag Preroll Ad example ###
```html
<div style="position:relative;display:block;width:320px; height: 200px; top: 0px; left:0px;" id="video_preroll">
    <video style="width:320px; height: 200px;"
           src="http://videos.mefeedia.com/bb5f5e08e8f74a121100de36b0c5d55b.mp4"
           poster="http://images.mefeedia.com/entries/66256474/video_250.png" controls></video>
</div>
<input type="button" value="Play Video" id="video_play">
<script type="text/javascript">
    document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_ad_min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
<script type="text/javascript">
    var video_preroll = new bfiomobileweb();
    if ( video_preroll) {
        var preroll_params = {
            app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',
            ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba',
            preroll_mode:'video',
            autoplay: true
        }
        video_preroll.Preroll('video_preroll', preroll_params);
        document.getElementById('video_play').addEventListener('click', video_preroll.PrerollStart);
    }
</script>
```

### `<A>` link to video file Preroll Ad example ###
```html
<div id="link_preroll" style="position:relative; display:block;width:250px; height: 188px;">
    <a href="http://videos.mefeedia.com/bb5f5e08e8f74a121100de36b0c5d55b.mp4">
        <img src="http://images.mefeedia.com/entries/66256474/video_250.png">
    </a>
</div>
<input type="button" value="Click To Play" id="video_play">
<script type="text/javascript">
    document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_ad_min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
<script type="text/javascript">
    var link_preroll = new bfiomobileweb();
    if ( link_preroll) {
        var preroll_params = {
            app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',
            ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba',
            preroll_mode:'link',
            autoplay: false
        }
        link_preroll.Preroll('link_preroll', preroll_params);
        document.getElementById('video_play').addEventListener('click', link_preroll.PrerollStart);
    }
</script>
```

### On demand preroll Ad example ###
```html
<div id="action_preroll" style="position:relative; display:block; width:320px; height: 200px;"></div>
<input type="button" value="Click To Play" id="video_play">
<script type="text/javascript">
    document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_ad_min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
<script type="text/javascript">
    var action_preroll = new bfiomobileweb();
    if ( action_preroll) {
        var preroll_params = {
            app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',
            ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba',
            preroll_mode:'action',
            callback: function(){alert('callback action');}
        }
        action_preroll.Preroll('action_preroll', preroll_params);
        document.getElementById('video_play').addEventListener('click', action_preroll.PrerollStart);
    }
</script>
```
