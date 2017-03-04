# Setting up Quick Tiles in Android 7.0 to Control HASS

This is a step by step guide on how to set up quick settings tiles to trigger automations, on your android device.

1. In order to do this, you'll need to download [HTTP Shortcuts](https://play.google.com/store/apps/details?id=ch.rmy.android.http_shortcuts) and [MacroDroid](https://play.google.com/store/apps/details?id=com.arlosoft.macrodroid) (I prefer this to Tasker, but Im sure you can do the same with Tasker).

2. Next, open HTTP Shortcuts and create a new shortcut with the following settings. My example turns on a scene, so you will need to customize it for your use.

    * URL (For the URL, it will depend if you have SSL set up or not.)
        - For SSL: https://NAME.duckdns.org/api/services/scene/turn_on
        - No SSL: http://IPADDRESS:8123/api/services/scene/turn_on
    * Method: Post
    * Body Content: `{"entity_id": "scene.morning"}`

3. Then scroll down to advance settings and add two headers with the following info:
    * -x-ha-access: `YOUR PASSWORD`
    * -Content-Type: `application/json`
4. Thats it for that. Now save it and open up Macrodroid.
5. Go into settings and open up Quick Tile Settings.
6. Enable a quicktile, give it a name and an icon and set it to Button.
7. Now create a new Macro with the following settings:
    * Trigger: Quick Settings Tile 1 (or whichever one you set up)
    *  Action: Locale / Tasker Plugin (You will then select HTTP Shortcuts and choose the correct shortcut)
8. Thats it. Now open up your quick settings and add the new shortcut!
9. If you have Data Saver turned on, you will want to enable unrestricted data for HTTP Shortcuts otherwise it will error out when on 3G. This took me way too long to figure out.

HTTP Shortcuts is awesome and pairing it with automations gives it limitless possibilities. I also have another macro that turns on my Good Morning scene when my alarm goes off on my phone, for example.  
