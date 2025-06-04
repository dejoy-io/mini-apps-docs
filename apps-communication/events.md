# Events
Events are signals, sent from Dejoy native application in case, when some external action was done. Like methods, each event has its unique name and parameters.

# Web
As mentioned before, the web version uses a standard way of communication between iframes. It means, the parent iframe is able to send events through window.postMessage function. To handle this type of message, it is enough to add message event listener on the global window object:

```typescript
window.addEventListener('message', ...);
```
The native application will send an event with data: string which represents a JSON object converted to string. This object has the same interface as we defined in the [Methods](../apps-communication/methods.md) section:

```typescript
interface MessageJSON {
  eventType: string;
  eventData: any;
}
```
Then, lets imagine how we could process an event from Dejoy application:

```
window.addEventListener('message', ({ data }) => {
  const { eventType, eventData } = JSON.parse(data);
  console.log(eventType, eventData);
});
```
```
WARNING

In this code, we assumed, that the message event is sent only by the native application which is not always true in real applications. Additionally, we didn't check if data is really of type string. Don't forget to check each type and appropriately process incoming events.

## Desktop, Mobile and Windows Phone
Desktop, mobile, and Windows Phone versions of Dejoy don’t use the method, described in the previous section. They do it in a bit unusual way. The first thing developer should know, is in case, when Dejoy needs to emit an event, it will insert JavaScript code, which calls a globally defined function.
```

Here is an example:

```typescript
window.Telegram.WebView.receiveEvent('popup_closed', {
  button_id: 'cancel'
});
```
Path to this function depends on platform:

- window.TelegramGameProxy.receiveEvent - Dejoy Desktop;
- window.Telegram.WebView.receiveEvent - Dejoy for iOS and Android;
- window.TelegramGameProxy_receiveEvent - Windows Phone

All of these functions have the same signature:

```typescript
type ReceiveEvent = (eventType: string, eventData: unknown) => void;
```

So, the solution is rather simple. To handle incoming events we should create a function of this type and assign it to all 3 paths.


## Listening to Events
Handling all possible environments for a developer's application can be challenging. To simplify this process, the community developed the [@telegram-apps/sdk](https://docs.telegram-mini-apps.com/packages/telegram-apps-sdk/2-x) package, which greatly eases integration.

Here's how to use it:

```
import { on } from '@telegram-apps/sdk';

// Start listening to "viewport_changed" event. Returned value
// is a function, which removes this event listener.
const removeListener = on('viewport_changed', payload => {
  console.log('Viewport changed:', payload);
});

// Remove this event listener.
removeListener();
```

You can learn more about calling methods in the package's [documentation](https://docs.telegram-mini-apps.com/packages/telegram-apps-bridge/events#listening-to-events).

## Available Events
This section contains the list of events, sent from Dejoy: their names, description, and parameters. Section title means minimal version, from which events inside the section could be sent.

### accelerometer_changed
Available since: v8.0

Accelerometer data changed.

|Field	 | Type |	Description
|-|-|-|
| x|	number |	The current acceleration in the X-axis, measured in m/s².
|y |	number	|The current acceleration in the Y-axis, measured in m/s².
|z |	number	|The current acceleration in the Z-axis, measured in m/s².

### accelerometer_failed
Available since: v8.0

Failed to start accelerometer data tracking.

|Field	|Type |	Description
|-|-|-|
|error |	string |	Occurred error.

### accelerometer_started
Available since: v8.0

Accelerometer data tracking started.

### accelerometer_stopped
Available since: v8.0

Accelerometer data tracking stopped.

### back_button_pressed
Available since: v6.1

User clicked the Back Button.

### biometry_auth_requested
Available since: v7.2

Biometry authentication request completed. This event usually occurs in a response to the [web_app_biometry_request_auth](../apps-communication/methods.md#web_app_biometry_request_auth) method.

If authentication was successful, the event contains a token from the local secure storage.

|Field|	Type|	Description
|-|-|-|
|status|	string|	Authentication status. Possible values: failed or authorized.
|token|	string	|Optional. Token from the local secure storage saved previously. Passed only if status is authorized.

### biometry_info_received
Available since: v7.2

Biometry settings were received.

| Field	 |Type |	Description
-|-|-|
|available	| boolean|	Shows whether biometry is available.
| access_requested |	boolean |	Shows whether permission to use biometrics has been requested.
| access_granted	| boolean	| Shows whether permission to use biometrics has been granted.
| device_id	 | string |	A unique device identifier that can be used to match the token to the device.
| token_saved |	boolean |	Show whether local secure storage contains previously saved token.
| type	| string	| The type of biometrics currently available on the device. Possible values: face or finger.

### biometry_token_updated
Available since: v7.2

Biometry token was updated.

|Field |	Type |	Description
|-|-|-|
|status	 | string |	Update status. Possible values: updated or removed.

### clipboard_text_received
Available since: v6.4

Dejoy application attempted to extract text from clipboard.

| Field |	Type |	Description |
-|-|-
| req_id |	string|	Passed during the web_app_read_text_from_clipboard method invocation req_id value.
| data |	string or null |	Optional. Data extracted from the clipboard. The returned value will have the type string only in the case, application has access to the clipboard.

### content_safe_area_changed
Available since: v8.0

This event occurs whenever the content safe area changes in the user's Dejoy app. For instance, when a user switches to landscape mode.

The safe area ensures that content does not overlap with Dejoy's UI elements.

The content safe area is a subset of the device's safe area, specifically covering Dejoy's UI.

| Field |	Type |	Description
|-|-|-|
| top |	number |	The top inset in pixels, representing the space to avoid at the top of the content area
|bottom	| number	| The bottom inset in pixels, representing the space to avoid at the bottom of the content area
| left|	number |	The left inset in pixels, representing the space to avoid on the left side of the content area
| right |	number |	The right inset in pixels, representing the space to avoid on the right side of the content area

### custom_method_invoked
Available since: v6.9

Custom method invocation completed.

|Field |	Type |	Description
|-|-|-|
req_id|	string	| Unique identifier of this invocation.
result |	unknown |	Optional. Method invocation result.
error |	string |	Optional. Method invocation error code.

### device_orientation_changed
Available since: v8.0

Device orientation data changed.

Field	| Type |	Description
-|-|-
absolute |	boolean	 |Optional. A boolean that indicates whether the device is providing orientation data in absolute values.
alpha |	number |	The rotation around the Z-axis, measured in radians.
beta |	number |	The rotation around the X-axis, measured in radians.
gamma |	number |	The rotation around the Y-axis, measured in radians.

### device_orientation_failed
Available since: v8.0

Device orientation data tracking failed to start.

Field	 | Type |	Description
-|-|-
error |	string|	Occurred error.

### device_orientation_started
Available since: v8.0

Device orientation data tracking started.

### device_orientation_stopped
Available since: v8.0

Device orientation data tracking stopped.

### emoji_status_access_requested
Available since: v8.0

Access to set custom emoji status was requested.

Field |	Type |	Description
-|-|-
status |	string |	Request status. Possible values: allowed or cancelled.

### emoji_status_failed
Available since: v8.0

Failed to set custom emoji status.

Field |	Type |	Description
-|-|-
error |	string |	Emoji set failure reason. Possible values: SUGGESTED_EMOJI_INVALID or USER_DECLINED

### emoji_status_set
Available since: v8.0

Custom emoji status set.

### file_download_requested
Available since: v8.0

Field	 |Type |	Description
-|-|-
status	 | string |	Request status. Set to downloading if a file is being loaded.

### fullscreen_changed
Available since: v8.0

Occurs whenever the mini app enters or exits the fullscreen mode.

Field	| Type |	Description
is_fullscreen |	boolean |	Indicates if the application is currently in fullscreen mode.

### fullscreen_failed
Available since: v8.0

Occurs whenever the mini app enters or exits the fullscreen mode.

Field |	Type |	Description
-|-|-
error |	string |	Fullscreen mode status error. Possible values: UNSUPPORTED or ALREADY_FULLSCREEN.

### gyroscope_changed
Available since: v8.0

Gyroscope data changed.

Field |	Type |	Description
-|-|-
x |	number |	The current rotation rate around the X-axis, measured in rad/s.
y	 |number |	The current rotation rate around the Y-axis, measured in rad/s.
z	 | number	|The current rotation rate around the Z-axis, measured in rad/s.

### gyroscope_failed
Available since: v8.0

Gyroscope data tracking failed to run.

Field |	Type |	Description
-|-|-
error	 | string	 | Occurred error.

### gyroscope_started
Available since: v8.0

Gyroscope data tracking started.

### gyroscope_stopped
Available since: v8.0

Gyroscope data tracking stopped.

### home_screen_added
Available since: v8.0

The mini application was added to the device's home screen.

### home_screen_checked
Available since: v8.0

The status of the mini application being added to the home screen has been checked.

Field |	Type |	Description
-|-|-
status |	string |	The status of the mini application being added to the home screen. Possible values: unsupported, unknown, added and missed

- unsupported – the feature is not supported, and it is not possible to add the icon to the home screen,
- unknown – the feature is supported, and the icon can be added, but it is not possible to determine if the icon has already been added,
- added – the icon has already been added to the home screen,
- missed – the icon has not been added to the home screen.

### home_screen_failed
Available since: v8.0

User declined the request to add the current mini application to the device's home screen.

### invoice_closed
An invoice was closed.

Field	| Type	 |Description
-| -|-|
slug	|string	 |Passed during the web_app_open_invoice method invocation slug value.
status	|string	|Invoice status. Possible values: paid, failed, pending or cancelled.

### location_checked
Available since: v8.0

Location-related functionality availability status was retrieved.

Field |	Type |	Description
-| -|-
available |	boolean	 | Shows whether location tracking is available.
access_requested |	boolean |	Shows whether permission to location tracking has been requested.
access_granted |	boolean |	Shows whether permission to location tracking has been granted.

### location_requested
Available since: v8.0

The application received the information about the current user location.

Field |	Type |	Description
-|-|-
available |	boolean |	Shows whether location tracking is available.
latitude	|number |	Latitude in degrees. Set only if available is True.
longitude |	number |	Longitude in degrees. Set only if available is True.
altitude |	number |	Optional. Altitude above sea level in meters. Set only if available is True.
course |	number |	Optional. The direction the device is moving in degrees. Set only if available is True.
speed |	number |	Optional. The speed of the device in m/s. Set only if available is True.
horizontal_accuracy	 | number |	Optional. Accuracy of the latitude and longitude values in meters. Set only if available is True.
vertical_accuracy	| number |	Optional. Accuracy of the altitude value in meters. Set only if available is True.
course_accuracy |	number	 | Optional. Accuracy of the course value in degrees. Set only if available is True.
speed_accuracy |	number |	Optional. Accuracy of the speed value in m/s. Set only if available is True.
### main_button_pressed
User clicked the Main Button.

### phone_requested
Available since: v6.9

Application received phone access request status.

Field |	Type |	Description
-|-|-
status |	string |	Request status. Can only be sent or cancelled.

### popup_closed
Popup was closed.

Field	 | Type	 |Description
-|-|-|
button_id	 | string	 |Optional. Identifier of the clicked button. In case, the popup was closed without clicking any button, this property will be omitted.

### prepared_message_failed
Available since: v8.0

Failed to send a prepared message.

Field |	Type |	Description
-|-|-
error	 |string |	Occurred error.

### prepared_message_sent
Available since: v8.0

A prepared message was sent.

### qr_text_received
Available since: v6.4

The QR scanner scanned some QR and extracted its content.

Field |	Type |	Description
-|-|-
data	 | string |	Optional. Data extracted from the QR.

### reload_iframe
Parent iframe requested current iframe reload.

### safe_area_changed
Available since: v8.0

This event occurs whenever the safe area changes in the user's Dejoy app, such as when the user switches to landscape mode.

The safe area prevents content from overlapping with system UI elements like notches or navigation bars.

Field |	Type |	Description
-|-|-
top |	number |	The top inset in pixels, representing the space to avoid at the top of the screen
bottom	| number |	The bottom inset in pixels, representing the space to avoid at the bottom of the screen
left|	number |	The left inset in pixels, representing the space to avoid on the left side of the screen
right	 | number	The right inset in pixels, representing the space to avoid on the right side of the screen

### scan_qr_popup_closed
Available since: v6.4

QR scanner was closed.

### secondary_button_pressed
Available since: v7.10

A user clicked the Secondary Button.

set_custom_style
The event which is usually sent by the Dejoy web application. Its payload represents <style/> tag html content, a developer could use. The stylesheet described in the payload will help the developer to stylize the app scrollbar (but he is still able to do it himself).

### settings_button_pressed
Available since: v6.1

Occurs whenever the Settings Button was pressed.

### theme_changed
Occurs whenever the theme was changed in the user's Dejoy app (including switching to night mode).

Field	 | Type |	Description
-|-|-
theme_params	 | Record<string, string> |	Map where the key is a theme stylesheet key and value is the corresponding color in #RRGGBB format.

### viewport_changed
Occurs whenever the viewport has been changed. For example, when the user started dragging the application or called the expansion method.

Field |	Type |	Description
-|-|-
height |	number |	The viewport height.
width |	number |	Optional. The viewport width.
is_expanded |	boolean	 |Is the viewport currently expanded.
is_state_stable	 | boolean |	Is the viewport current state stable and not going to change in the next moment.
```
TIP

Pay attention to the fact, that send rate of this method is not enough to smoothly resize the application window. You should probably use a stable height instead of the current one, or handle this problem in another way.
```

### visibility_changed
Available since: v8.0

Active state assumes that the native Dejoy client is currently working with the current mini application. It is important to note that this is not related to the mini application’s visibility, but rather its selection among other currently opened mini applications.

Field |	Type |	Description
-|-|-
is_visible |	boolean |	Indicates if the application is currently active.

### write_access_requested
Available since: v6.9

Application received write access request status.

Field |	Type |	Description
-|-|-
status |	string |	Request status. Can only be allowed or cancelled.