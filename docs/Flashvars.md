# Player Flashvars

The Media Platform player embed contains a powerful solution allowing many perameters to be passed into the player embed client side to control elements of playback, style and functionality within the application itself.

Helpful articles for further reading

1. [Kaltura playerAPI](http://player.kaltura.com/docs/api)
2. [Flashvar List](http://player.kaltura.com/docs/api#uiVars)

<!-- theme: warning -->

> ### Flashvar risks
>
> Incorrect use of flashvars can affect playback and cause issues for end users if not correctly verified.

Flashvar format

Flashvars form part of the player embed code and always confrom to the below format see [Video Player Overview](./Video-Player-Overview)

```html
<html>
	<head>
		<script src="https://open.http.mp.streamamg.com/p/<PARTNER_ID>/sp/<PARTNER_ID>00/embedIframeJs/uiconf_id/<UICONF_ID>/partner_id/<PARTNER_ID>"></script> 
	</head>
	<body>
		<div id="<VIDEO_DIV_ID>" style="width: 560px; height: 351px;"></div> 
		<script> 
			kWidget.embed({ 
				"targetId": "<VIDEO_DIV_ID>", 
				"wid": "<_PARTNER_ID>", 
				"uiconf_id": <UICONF_ID>, 
					// Plugin configuration / flashvars go here 
		'flashvars':{
			'myPlugin':{
				'fooAttribute': 'bar',
				'barAttribute': 'foo'
			},
			'autoPlay' : true
		},
				"entry_id": "<ENTRY_ID>" 
			}); 
		</script>
	</body>
</html>

```

# Useful Flashvars

There are many flashvar availiable for use but the most common/useful can be found below

## Ksession (KS)
The ksession is the secure playback token that should be created and passed into the embed code to authnetticate playback see [Generating-a-Ksession](./Generating-a-Ksession.md)

#### sample

```json

	"flashvars": { 
										"ks" : "MjdhNDJmZjI4MDk1OTM3NjJkMTExYTUyODk4ZmMzYTllOTlkNWIwOTMwMDEzNDM7MzAwMTM0MzsxNTY5MzI2ODgwOzA7MTU2OTI0MDQ4MDtzdmlldzowX256NHllNjN4LHNldHJvbGU6UExBWUJBQ0tfQkFTRV9ST0xFLHdpZGdldDoxOzt3bGM="
				}, 
```

## Player Watermarking
Layered watermarking can be applied to player both in the studio [Here]((https://kmc.mp.streamamg.com/studio/v2) or via the embed code itself using the below flashvar permaters

1. **plugin** active or not active for this video
2. **img** image location
3. **href** image click through link
4. **CssClass** Css class for location, padding, etc

#### sample

```json
flashvars: {
		
			"watermark": {
				"plugin" : true,
				"img" : "https://www.worcester.gov.uk/images/easyblog_shared/2019/b2ap3_large_Football---carousel.jpg",
				"href" : "http://www.football.com/",
				"cssClass" : "topRight"
			},
```
## Google DFP Advertising
Advertising via VAST, VPAID or VMAP urls can be applied to player both in the studio [Here]((https://kmc.mp.streamamg.com/studio/v2) or via the embed code itself using the below flashvar permaters for when using dynamic and custom fields for a DFP tag.

1. **plugin** active or not active for this video
2. **adTagUrl** Specific Ad tag, variable permaters such as refferer, Age, video type, size that are requested should be dynamically inserted.

```json
	flashvars: {
			"doubleClick": {
				"plugin" : true,
				"adTagUrl" : "http://pubads.g.doubleclick.net/gampad/ads?sz=640x480&iu=%2F3510761%2FadRulesSampleTags&ciu_szs=160x600%2C300x250%2C728x90&cust_params=adrule%3Dpremidpostwithpod&impl=s&gdfp_req=1&env=vp&ad_rule=1&vid=12345&cmsid=3601&output=xml_vast2&unviewed_position_start=1&url=[referrer_url]&videotype=[tag]&age={over18]&loginstatus={true]correlator=[timestamp]",

	
			},

		}
 ```
## Autoplay
Does the video want to autoplay or not

<!-- theme: info-->
>When starting a video with autoplay the video must start muted due to browser restrictions on user initated playback and ausio.

1. **autoPlay** true or false

```json
flashvars: {
			"autoPlay" : true,
					},
 ```
## Custom Player Styling
Player styling can be mananged in the player both in the studio [Here]((https://kmc.mp.streamamg.com/studio/v2) or by applying a custom CSS file to player within the embed code

Further reading on the styling components can be found [HERE](https://knowledge.kaltura.com/help/kaltura-player-toolkit-theme-skin-and-plugins-guide)

```json
	"flashvars": { 
			"IframeCustomPluginCss1" = "Cssfileurl.css"
				}, 

```

