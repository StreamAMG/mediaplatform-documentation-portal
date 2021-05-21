# Generating a Ksession (KS)

When not using the complimentary StreamAMG Cloudpay solution (See Cloudpay) for Ksession token generation, this service should be ran by the client application to generate the secure and time based playback tokens


## Generate a ksession (KS) access token
To generate a ksession token for playing back a video the below helper can be ran server side to return a ksession token response for a specific video. 

A token should be generated every 10 minutes and that same token stored and used to authenticate all playback reuqests for a specific video to reduce the number of calls a client would need to make.

#### Variables needed

1. **Partner ID**, which can be found in the KMC under Integration Settings
2. **Admin Secret key** This can be found in the integration settings area within Media Platform [HERE](https://kmc.mp.streamamg.com/settings/integrationSettings)
3. **Entry ID** for the entry you want to embed, which is found in the Content tab, This should be used as **sview** peramater
4. **userid** is not required for a valid ksession
5. **Expiry Time** expiry time of the token suggested as 10 minutes.

```php
<?php class KalturaHelper {        public static function generateCachedEntryViewingSession($adminSecret, $userId, $entryId, $partnerId, $expiry = 86400, $cache = 60, $custom = '')        {                return KalturaHelper::generateCachedSession($adminSecret, $userId, '0', $partnerId, $expiry, 'sview:' . $entryId . ',setrole:PLAYBACK_BASE_ROLE,widget:1', $cache, $custom);        } 
        public static function generateCachedSession($adminSecret, $userId, $type, $partnerId, $expiry = 86400, $privileges = '', $cache = 60, $custom = '')        {                $roundedTime = time() + ($cache - (time() % $cache));                $expiry = $roundedTime+$expiry;                $info = $partnerId.';'.$partnerId.';'.$expiry.';'.$type.';'.$roundedTime.';'.$us erId.';'.$privileges.';;' . $custom;                $signature = KalturaHelper::hash ( $adminSecret , $info );                $encoded_str = base64_encode( $signature . "|" . $info ); 
                return $encoded_str;        } 
        private static function hash ( $salt , $str )        {                $tosha = $salt . $str;                return sha1($tosha);        } 
} 
$secret = new KalturaHelper(); $myValue = $secret::generateCachedEntryViewingSession("<ADMIN_SECRET>", "", "<ENTRY_ID>", "<PARTNER_ID>"); echo $myValue; 
?>
```
<!-- theme: warning -->
>### Ksession token
>If  valid ksession token is not valid  the video 
>will not play.
## KSession response

A kession is returned in a base64 encoded format an example of which is below

KSession":"N2NkZjY1NTc2OTZhNmRiODY2ODM5YzFjZWNjZGQzMTU0MzRhOGM5Y3wzMDAwNTgyOzMwMDA1ODI7MTYyMTYxMjIwMDswOzE2MjE1OTQyMDA7Vmlld2VyO3N2aWV3OjBfZmxkMXJvM3Msc2V0cm9sZTpQTEFZQkFDS19CQVNFX1JPTEUsd2lkZ2V0OjE7Ow=="

#### Decoded response

```json
Kaltura_Client_KalturaInternalTools_Type_InternalToolsSession Object
(
    [partner_id] => 3000582 ///partnerId
    [valid_until] => 1621612200///epoch valid until time
    [partner_pattern] => 3000582///partnerid
    [type] => 0 /ksession type 
    [error] => ///valid errors
    [rand] => 1621594200//epoch time now
    [user] => Viewer //ksession type
    [privileges] => sview:0_fld1ro3s,setrole:PLAYBACK_BASE_ROLE,widget:1  //privilages to access content only sview.<entryid>
)
<br>valid until: 21/05 16:50:00<br>now: 1621593922 (21/05 11:45:22)
````