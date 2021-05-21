# Video Player Overview

The Media Platform player is based upon the Kaltura CE player With multiple improvement and adaptions primarily for the sports and betting industries. The primary base is HLSJS with all streams served using the HLS format to end users [HLS](https://developer.apple.com/streaming/)

This article documents the basics of embedding a video player in a web or html5 application

## Create a video Player Embed Code
To manually write an embed code, you’ll need a few things:

1. Your **Partner ID**, which can be found in the KMC under Integration Settings
2. The **Player ID** of the player you wish to use, which can be found in the Studio tab in the ID column. It is also known as the **UI Conf ID**.
3. The **Entry ID** for the entry you want to embed, which is found in the Content tab
4. An **HTML div** in the page you want to embed your player, the player will render responsivley based on the div size
5. A **Ksession token** (also reffered to as KS) to authenticate playback, Read more about it [HERE](./Flashvars.md)


<!-- theme: info -->

> ### My Creds
>
> Visit Media Platform[**Here**](https://kmc.mp.streamamg.com/studio/v2)to retireve these Variables, or speak to your account manager/Integration team.

### Example Embed code
<!--
title: "Standard Html embed code"
lineNumbers: true
-->
```html

<html>
	<head>
			<script src="https://open.http.mp.streamamg.com/p/<PARTNERID>/sp/<PARTNERID>00/embedIframeJs/uiconf_id/<GLOBALUICONF>/partner_id/<PARTNERID>"></script> 
	</head>
	<body>
		<div id="mp_player_1520883895" style="width: 100%; height: auto;"></div> 
		<script> 
			kWidget.embed({ 
				"targetId": "mp_player_1520883895", 
				"wid": "_<PARTNERID>", 
				"uiconf_id": <UICONF>
				"flashvars": { 
					"streamerType": "auto" ,
					"ks": "<KSESSIONTOKEN>
							}, 
				"entry_id": "<ENTRY>" 
			}); 
		</script>
	</body>
</html>
```
<!-- theme: warning -->

>### Ksession token
>If  valid ksession token is not passed into the embed code the video 
>will not play, This token is either retrieved by the [cloudpay ksession service](./CloudPay-API-Specification.yaml/paths/~1api~1v1~1session~1ksession/get) or via the  helper service [Found here](./Generating-a-Ksession.md)

## Other Embed types

For other non standard use cases. You can retrieve an embed code for any peice of content within the KMC directly for a more simplified but less flexible embedding solution.

### Auto Embed
Auto embed is the most basic embed code for the Media Platform player. It uses precise code and is good for getting a player or widget onto the page quickly and easily without any runtime customizations.

Auto embed is optimized for packing a lots of resources into the initial request, allowing the player to be rendered quickly.

Here’s how to use the auto embed code:

```html
<script src="https://open.http.mp.streamamg.com/p/<PARTNERID>/sp/<PARTNERID>00/embedIframeJs/uiconf_id/<UICONF>/partner_id/<PARTNERID>?autoembed=true&entry_id=<ENTRYID>&playerId=<HTMLDIV>&width=100%&height=100%&flashvars[ks]=<KSESSIONTOKEN>"></script>
```

### Dynamic Embed
Dynamic embed is recommended for cases where you want to control runtime configuration dynamically and/or have more control over the embed call.

Basic dynamic embed code is in the below format:
```html
<script src="https://open.http.mp.streamamg.com/p/<PARTNERID>/sp/<PARTNERID>00/embedIframeJs/uiconf_id/<UICONF>/partner_id/<PARTNERID>"></script>
<div id="mp_player_1621589911" style="width: 1280px; height: 720px;"></div>
<script>
kWidget.embed({
  "targetId": "mp_player_1621589911",
  "wid": "_<PARTNERID>,
  "uiconf_id": <UICONF>,
  "flashvars": { 'ks' = '<KSESSIONTOKEN>' },
  "entry_id": "<ENTRYID>"
});
</script>
```
### IFrame Embed
The iframe embed is good for sites that don’t allow third-party JavaScript to be embedded in their pages. This makes the iFrame embed a better fit for sites that have stringent page security requirements.

Note that if you use the iFrame-only embed mode, the page won’t be able to access the player API:

```html
<iframe id="mp_player_1621592293" src="https://open.http.mp.streamamg.com/p/<PARTNERID>/sp/<PARTNERID>00/embedIframeJs/uiconf_id/<UICONF>/partner_id/<PARTNERID>?iframeembed=true&playerId=mp_player_1621592293&entry_id=<ENTRYID>&flashvars[ks]=<KSESSIONTOKEN>" width="100%" height="100%" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay; fullscreen; encrypted-media" frameborder="0"></iframe>
```

## Destroy Player embed
kWidget.destroy enables you to cleanly remove a kWidget.embed:


```html
<div id="myEmbedTarget" style="width:400px;height:330px;"></div>
<script src="https://open.http.mp.streamamg.com/p/<PARTNERID>/sp/<PARTNERID>00/embedIframeJs/uiconf_id/<UICONF>/partner_id/<PARTNERID>"></script>
<script>
	kWidget.destroy( 'myEmbedTarget' );
</script>
```

## Adding SEO components

You can add supportive SEO metadta to the relevent div container for any embed code type as follows.
```html
<div id="myVideoContainer" itemprop="video" itemscope itemtype="http://schema.org/VideoObject" >
	<div id="myVideoTarget" style="width:400px;height:330px;">
		<!--  SEO and video metadata go here -->
		<span itemprop="description" content="test folgers coffe"></span>
		<span itemprop="name" content="FolgersCoffe.mpeg"></span>
		<span itemprop="width" content="400"></span>
		<span itemprop="height" content="300"></span>
	</div>
</div>
```

