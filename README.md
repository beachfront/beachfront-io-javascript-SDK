beachfront-io-javascript-SDK
============================

JavaScript SDK for Beachfront.iO

1. Include js library, it can be included in page header or directly inside page.

```
<script type="text/javascript" src="http://beachfront.io/preroll/bfio_preroll_3_min.js"></script>
```
or 

```
<script type="text/javascript">
    document.write('<script language="JavaScript" src="http://beachfront.io/preroll/bfio_preroll_3_min.js" type="text/javascript"><\/scr' + 'ipt>');
</script>
```

2. Put a DIV container inside page. Video Ad will be played inside this container, video ad will have the same  sizes like a DIV container

'video': require container with <video> tag. 

for example:

```
<div style="position:relative;display:block;width:320px; height: 240px; top: 0px; left:0px;" id="preroll1">
    <video style="width:320px; height: 240px;"
           src="http://videos.mefeedia.com/bb5f5e08e8f74a121100de36b0c5d55b.mp4"
           poster="http://images.mefeedia.com/entries/66256474/video_250.png" controls id="myvideo"></video>
</div>
```
'link': require container with <a> tag. 

for example:
```
<div id="preroll2" style="position:relative; display:block;width:250px; height: 188px;">
    <a href="http://videos.mefeedia.com/bb5f5e08e8f74a121100de36b0c5d55b.mp4">
        <img src="http://images.mefeedia.com/entries/66256474/video_250.png">
    </a>
</div>
```

'banner': require only container, so empty div can be used. ad video will try to autoplay
```
<div id="preroll3" style="position:relative; display:block; width:320px; height: 200px;"></div>
```

'action': require only container, so empty div can be used, but to start video play you should call PrerollStart() function. also for this mode you can apply callback function. This function will be called after ad complete, ad click or if no ad.

```
<div id="preroll3" style="position:relative; display:block; width:320px; height: 200px;"></div>
```

Step 4:  

Init ad unit.

For each ad unit container you should define an object.

```
var preroll1 = new bfiomobileweb();
```

After this cdefine params:
```
var params = {
            app_id:'e04fd6b0-4eb2-4dc8-b8d3-accfb7cf8043',  // application id
            ad_init_id:'622834c9-3b52-4114-a531-a4bf494230ba', // ad unit id
            preroll_mode:'action', // (preroll type, can be 'video', 'link', 'action', 'banner' )
            autoplay: true, // (true/false), by default true.
            volume: 0.5, // ad valume, not mandatory, by default 0.5
            callback: function(){alert('some action');} // callback function for   preroll_mode = 'action'. For other modes is not required
        }
```
Now you can init preroll for DIV container with given params. First function attr can be document elem or elem ID, the second attr is object with params.

```
preroll4.Preroll('preroll4', preroll4_params);
```

To start preroll from code you can call preroll4.PrerollStart() method.

Step 5:

Add any event handlers you require:

AdPreRollInitialized  - ad unit is ready to use (all even't applied and ready)

AdClickThru - The 'AdClickThru' event will be fired when the click-through html is clicked. (Ad Video was clicked)

AdVideoStart - The 'AdVideoStart' event will be fired when the video starts playing. 
This is fired after receiving the callback for  the 'timeupdate' event of the HTML5 video element for the very first time.

AdPlaying - The 'AdPlaying' event will be fired after receiving the callback for the 'play' event of the HTML5 video element.

AdVideoFirstQuartile - The 'AdVideoFirstQuartile' event will be fired when 25% of the video is completed.

AdVideoMidpoint - The 'AdVideoFirstQuartile' event will be fired when 50% of the video is completed.

AdVideoThirdQuartile - The 'AdVideoFirstQuartile' event will be fired when 75% of the video is completed.

AdVideoComplete - The 'AdVideoFirstQuartile' event will be fired when 100% of the video is completed.

AdError - The 'AdError' event will be fired when ad fails to load or encounters an error. 


Event handler can be applied like:  preroll4.addListener('AdPlaying', function(){});
