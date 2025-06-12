# Methods
  DeJoy Mini Apps methods are events, which execute some predefined action. They are always called by a Mini App.

## Web
Since the web version of DeJoy displays the front-end application in an ```<iframe/>``` tag, it uses the default way of communication between 2 iframes - sending messages through ```window.parent.postMessage``` function.

As the first parameter, you should pass a JSON object converted to a string. The object should have this interface:

```typescript
interface MessageJSON {
  eventType: string;
  eventData?: any;
}
```

The second parameter is ```targetOrigin``` - allowed parent iframe origin. We recommend avoiding the usage of wildcard ```*``` as long as it is not secure - your application could be inserted not by DeJoy, but by another iframe that will still be able to communicate with your app and receive some data.

As a default value, you could use https://dejoy.io.

So, as you see, each method has its own name expressed by eventType and parameters stored in eventData property. Here is the usage example:

```typescript
const data = JSON.stringify({
  eventType: 'web_app_setup_back_button',
  eventData: {
    is_visible: true,
  },
});

window.parent.postMessage(data, 'https://dejoy.io');
```
This code will make the  [BackButton](../ui/back-button.md) appear.

## Desktop and Mobile
Unlike the web, desktop and mobile applications use a bit more unusual way of calling methods. Both of these platforms will create a global function ```window.WebviewProxy.postEvent```.

As the first argument, this function accepts the event name. The second one - the parameters object, converted to a string. Here is how it works:

```typescript
const data = JSON.stringify({ is_visible: true });

window
  .WebviewProxy
  .postEvent('web_app_setup_back_button', data);
```
## Windows Phone
 Windows Phone app provides such function as ```window.external.notify```. It accepts the same parameter as the web version does:

```typescript
const data = JSON.stringify({
  eventType: 'web_app_setup_back_button',
  eventData: { is_visible: true },
});

window.external.notify(data);
```

## Calling Methods
Handling all possible environments for a developer's application can be challenging. To simplify this process, the community developed the [@telegram-apps/sdk](https://docs.telegram-mini-apps.com/packages/telegram-apps-sdk/2-x) package, which greatly eases integration.

Here's how to use it:

```typescript
import { postEvent } from '@telegram-apps/sdk';

postEvent('web_app_set_header_color', { color_key: 'bg_color' });
```

You can learn more about calling methods in the package's [documentation](https://docs.telegram-mini-apps.com/packages/telegram-apps-bridge/events#calling-methods).

## Available Methods
This section contains a list of available methods to call with their names, description, and parameters. In case, Mini App does not satisfy the minimal method version requirement, nothing will happen. The native app just doesn't know which method should be called as long as it is not defined internally.

### iframe_ready
Notifies parent iframe about the current frame is ready. This method is only used in the Web version of DeJoy. As a result, Mini App will receive ```set_custom_style``` event.

|Field|	Type	|Description
|-|-|-|
|reload_supported |	boolean|	Optional. True, if current Mini App supports native reloading.


### iframe_will_reload
Notifies parent iframe about the current iframe is going to reload.

### web_app_add_to_home_screen
Available since: v8.0

Prompts the user to add the Mini App to the home screen. Note that if the device cannot determine the installation status, the event may not be received even if the icon has been added.

### web_app_biometry_get_info
Available since: v7.2

Requests the current biometry settings.

### web_app_biometry_open_settings
Available since: v7.2

Opens the biometric access settings for bots. Useful when you need to request biometrics access to users who haven't granted it yet.

```
INFO

This method can be called only in response to user interaction with the Mini App interface (e.g. a click inside the Mini App or on the main button)
```

### web_app_biometry_request_access
Available since: v7.2

Requests permission to use biometrics.

|Field|	Type|	Description|
|-|-|-|
|reason|	string|	Optional. The text to be displayed to a user in the popup describing why the bot needs access to biometrics, 0-128 characters.

### web_app_biometry_request_auth
Available since: v7.2

Authenticates the user using biometrics.

|Field|	Type|	Description|
|-|-|-|
|reason	|string	|Optional. The text to be displayed to a user in the popup describing why you are asking them to authenticate and what action you will be taking based on that authentication, 0-128 characters.

### web_app_biometry_update_token
Available since: v7.2

Updates the biometric token in secure storage on the device. To remove the token, pass an empty string.

|Field|	Type|	Description|
|-|-|-|
|token |	string|	Token to store. Has max length of 1024 symbols.

### web_app_check_home_screen
Available since: v8.0

Sends a request to the native  application to check if the current mini application is added to the device's home screen.

### web_app_check_location
Available since: v8.0

Requests location-related functionality availability state.

### web_app_close
Closes Mini App.

### web_app_close_scan_qr_popup
Available since: v6.4

Closes a QR scanner. The  application creates the scan_qr_popup_closed event.

### web_app_data_send
Sends data to the bot. When this method is called, a service message is sent to the bot containing the data of the length up to 4096 bytes. Then, Mini App will be closed.

To get more information, take a look at ```web_app_data``` field in the class [Message](https://core.telegram.org/bots/api#message).

|Field|	Type|	Description|
|-|-|-|
|data |	string |	Data to send to a bot. Should not have size of more than 4096 bytes.


### web_app_exit_fullscreen
Available since: v8.0

Requests exiting the fullscreen mode for mini app.

### web_app_expand
Expands the Mini App.

### web_app_invoke_custom_method
Available since: v6.9

|Field|	Type|	Description|
|-|-|-|
|req_id|	string	|Current invocation unique identifier.
|method|	string	|Method name.
|params|	unknown	|Parameters according to method.

### web_app_open_invoice
Available since: v6.1

Opens an invoice by its specified slug. More information about invoices in this [documentation](https://core.telegram.org/bots/payments).

|Field|	Type|	Description|
|-|-|-|
|slug|	string|	Invoice unique identifier.

### web_app_open_link
Opens link in the default browser. Mini App will not be closed.

|Field|	Type|	Description|	Available since
|-|-|-|-|
|url |	string|	URL to be opened by  application. Should be a full path with https protocol.	
| try_instant_view |	boolean |	Optional. Link will be opened in Instant View mode if possible. |	v6.4

### web_app_open_location_settings
Available since: v8.0

Opens the location access settings for bots. Useful when you need to request location access from users who haven't granted it yet.

```
WARNING

Note that this method can be called only in response to user interaction with the Mini App interface (e.g., a click inside the Mini App or on the main button).
```

### web_app_open_popup
Available since: v6.2

Opens a new popup. When user closes the popup,  creates the popup_closed event.

|Field|	Type|	Description|
|-|-|-|
|title|	string|	The text to be displayed in the popup title, 0-64 characters|
|message|	string|	The message to be displayed in the body of the popup, 1-256 characters|
|buttons |	PopupButton[]|	List of buttons to be displayed in the popup, 1-3 buttons |

### PopupButton
|Field|	Type|	Description|
|-|-|-|
| id	|string	| Identifier of the button, 0-64 characters.
| type	|string	| Type of the button. Values:  - default, a button with the default style; - destructive, a button with a style that indicates a destructive action (e.| g. "Remove", "Delete", etc.); - ok, a button with the localized text "OK"; - close, a button with the localized text "Close"; - cancel, a button with the localized text "Cancel";
| text	|string |	The text to be displayed on the button, 0-64 characters. Ignored when type is ok, close or cancel. |

### web_app_open_scan_qr_popup
Available since: v6.4

Opens a QR scanner. When the scanner was closed, the  application creates the scan_qr_popup_closed event. When the scanner reads QR,  creates the qr_text_received event.

|Field|	Type|	Description|
|-|-|-|
|text |	string |	Optional. Text to be displayed in the QR scanner.

### web_app_open_tg_link
Available since: v6.1

Opens the  link by its pathname and query parameters. The link will be opened in the  app, Mini App will be closed.

|Field|	Type|	Description|
|-|-|-|
| path_full	 | string |	Should be a value taken from the link of this format: https://t.me/{path_full}. Can additionally contain query parameters.

### web_app_read_text_from_clipboard
Available since: v6.4

Reads text from the clipboard. The method accepts a request identifier which is used to appropriately retrieve the method execution result from the clipboard_text_received event.

|Field|	Type|	Description|
|-|-|-|
| req_id	| string	| Unique request identifier. Should be any unique string to handle the generated event appropriately.


### web_app_ready

Notifies  about current application is ready to be shown. This method will make  to remove application loader and display Mini App.

### web_app_request_content_safe_area
Available since: v8.0

Requests the current content safe area information from .

As a result,  triggers the content_safe_area_changed event.

### web_app_request_emoji_status_access
Available since: v8.0

Shows a native popup requesting permission for the bot to manage user's emoji status.

### web_app_request_file_download
Available since: v8.0

Displays a native popup prompting the user to download a file.

In turn, the  client emits the file_download_requested event.

|Field|	Type|	Description|
|-|-|-|
|url	|string |	The HTTPS URL of the file to be downloaded.
|file_name |	string |	The suggested name for the downloaded file.

### web_app_request_fullscreen
Available since: v8.0

Requests full screen mode for mini app.

### web_app_request_location
Available since: v8.0

Requests location data.

### web_app_request_phone
Available since: v6.9

Requests access to current user's phone.

### web_app_request_safe_area
Available since: v8.0

Requests the current safe area information from .

As a result,  triggers the safe_area_changed event.

### web_app_request_theme
Requests the current theme from . As a result,  will create theme_changed event.

### web_app_request_viewport
Requests the current viewport information from . As a result,  will create viewport_changed event.

### web_app_request_write_access
Available since: v6.9

Requests write message access to current user.

### web_app_send_prepared_message
Available since: v8.0

Opens a dialog allowing the user to share a message provided by the bot.

|Field|	Type|	Description|
|-|-|-|
|id	 |string	 | Identifier of the message (PreparedInlineMessage) previously obtained via the Bot API method savePreparedInlineMessage.


### web_app_set_background_color
Available since: v6.1

Updates the Mini App background color.

|Field|	Type|	Description|
|-|-|-|
|color	| string |	The Mini App background color in #RRGGBB format, or one of the values: bg_color or secondary_bg_color

### web_app_set_bottom_bar_color
Available since: v7.10

Updates the Mini App bottom bar background color.


|Field|	Type|	Description|
|-|-|-|
|color	 | string |	The Mini App bottom bar background color in #RRGGBB format, or one of the values: bg_color, secondary_bg_color or  bottom_bar_bg_color

### web_app_set_emoji_status
Available since: v8.0

Opens a dialog allowing the user to set the specified custom emoji as their status.

|Field|	Type|	Description|
|-|-|-|
|custom_emoji_id|	string|	Custom emoji identifier to set.
|duration|	number|	Optional. The status expiration time in seconds.


### web_app_set_header_color
Available since: v6.1

Updates the Mini App header color. This method should accept color_key or color property.

|Field|	Type|	Description|
|-|-|-|	Available since
| color_key	|string |	The Mini App header color key. Could be either bg_color or secondary_bg_color.	
| color|	string|	Color in RGB format.	v6.9


### web_app_setup_back_button
Available since: v6.1

Updates the Back Button settings.

|Field|	Type|	Description|
|-|-|-|
| is_visible|	boolean	|Should the Back Button be visible.

### web_app_setup_closing_behavior
Updates current closing behavior.

|Field|	Type|	Description|
|-|-|-|
|need_confirmation	|boolean|	Will user be prompted in case, an application is going to be closed.

### web_app_setup_main_button
Updates the Main Button settings.

|Field|	Type|	Description|	Available since
|-|-|-| -|
is_visible |	boolean |	Optional. Should the button be displayed.	
is_active	|boolean |	Optional. Should the button be enabled.	
is_progress_visible	 | boolean|	Optional. Should loader inside the button be displayed. Use this property in case, some operation takes time. This loader will make user notified about it.	
text|	string|	Optional. Text inside the button.	
color |	string |	Optional. The button background color in #RRGGBB format.	
text_color	|string |	Optional. The button text color in #RRGGBB format.	
has_shine_effect|	boolean |	Optional. Should the button have a shining effect.	v7.8

### web_app_setup_settings_button
Available since: v6.10

Updates current state of Settings Button.

|Field|	Type|	Description|
|-|-|-|
|is_visible |	boolean |	Should the Settings Button be displayed.

### web_app_setup_swipe_behavior
Available since: v7.7

Sets new swipe behavior.

|Field|	Type|	Description|
|-|-|-|
allow_vertical_swipe |	boolean |	Allows closing the application using vertical swipe.

### web_app_share_to_story
Available since: v7.8

A method that opens the native story editor.

|Field|	Type|	Description|
|-|-|-|
| media_url |	string	 |A media URL which will be used as a background for a created story.
text |	string |	Optional. The caption to be added to the media. 0-200 characters for regular users and 0-2048 characters for premium subscribers.
widget_link	 | object|	Optional. An object that describes a widget link to be included in the story. Note that only premium subscribers can post stories with links.
widget_link.url	 | string |	The URL to be included in the story.
widget_link.name |	string |	Optional. The name to be displayed for the widget link, 0-48 characters.

### web_app_start_accelerometer
Available since: v8.0

Starts tracking accelerometer data.

|Field|	Type|	Description|
|-|-|-|
| refresh_rate	| number |	The refresh rate in milliseconds, with acceptable values ranging from 20 to 1000. Note that refresh_rate may not be supported on all platforms, so the actual tracking frequency may differ from the specified value.

### web_app_start_device_orientation
Available since: v8.0

Starts tracking device orientation data.

|Field|	Type|	Description|
|-|-|-|
|refresh_rate |	number	 | The refresh rate in milliseconds, with acceptable values ranging from 20 to 1000. Note that refresh_rate may not be supported on all platforms, so the actual tracking frequency may differ from the specified value.
|need_absolute	 |boolean |	Optional. Pass true to receive absolute orientation data, allowing you to determine the device's attitude relative to magnetic north. Use this option if implementing features like a compass in your app. If relative data is sufficient, pass false.                                                  Keep in mind that some devices may not support absolute orientation data. In such cases, you will receive relative data even if need_absolute=true is passed.
### web_app_start_gyroscope
Available since: v8.0

Starts tracking gyroscope data.

|Field|	Type|	Description|
|-|-|-|
|refresh_rate |	number	| The refresh rate in milliseconds, with acceptable values ranging from 20 to 1000. Note that refresh_rate may not be supported on all platforms, so the actual tracking frequency may differ from the specified value.

### web_app_stop_accelerometer
Available since: v8.0

Stops tracking accelerometer data.

### web_app_stop_device_orientation
Available since: v8.0

Stops tracking device orientation data.

### web_app_stop_gyroscope
Available since: v8.0

Stops tracking gyroscope data.

### web_app_setup_secondary_button
Available since: v7.10

The method that updates the Secondary Button settings.

|Field|	Type|	Description|
|-|-|-|
|is_visible	| boolean |	Optional. Should the button be displayed.
| is_active |	boolean	 | Optional. Should the button be enabled.
| is_progress_visible |	boolean |	Optional. Should loader inside the button be displayed. Use this property in case, some operation takes time. This loader will make user notified about it.
| color |	string	 |Optional. The button background color in #RRGGBB format.
| text_color |	string	Optional. The button text color in #RRGGBB format.
| has_shine_effect |	boolean	 | Optional. Should the button have a shining effect.
| position	 |string |	Optional. Position of the secondary button. It applies only if both the main and secondary buttons are visible. Supported values: left, displayed to the left of the main button, right, displayed to the right of the main button, top, displayed above the main button, bottom, displayed below the main button.

### web_app_switch_inline_query
Available since: v6.7

Inserts the bot's username and the specified inline query in the current chat's input field. Query may be empty, in which case only the bot's username will be inserted. The client prompts the user to choose a specific chat, then opens that chat and inserts the bot's username and the specified inline query in the input field.

|Field|	Type|	Description|
|-|-|-|
|query |	string	|Text which should be inserted in the input after the current bot name. Max length is 256 symbols.
| chat_types	| string[] |	List of chat types which could be chosen to send the message. Could be empty list. Values: users bots groups channels

### web_app_toggle_orientation_lock
Available since: v8.0

Locks the Mini App’s orientation to its current mode (either portrait or landscape). Once locked, the orientation remains fixed, regardless of device rotation. This is useful if a stable orientation is needed during specific interactions.

|Field|	Type|	Description|
|-|-|-|
| locked |	boolean |	True if the orientation must be locked.
### web_app_trigger_haptic_feedback
Available since: v6.1

Generates the haptic feedback event.

|Field|	Type|	Description|
|-|-|-|
|type |	string |	Type of haptic event. Values: impact, when there's a collision involving UI components. notification, when some action execution has been completed. selection_change, when the user changes their selection. impact_style	string	Required when type is impact. Values: light, indicates a collision between small or lightweight UI objects medium, indicates a collision between medium-sized or medium-weight UI objects heavy, indicates a collision between large or heavyweight UI objects rigid, indicates a collision between hard or inflexible UI objects soft, indicates a collision between soft or flexible UI objects
| notification_type |	string |	Required when type is notification. Values: error,indicates that a task or action has failed success, indicates that a task or action has completed successfully warning, indicates that a task or action produced a warning