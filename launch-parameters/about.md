# Launch Parameters
The launch parameters are the list of parameters that is passed by the native Dejoy application to the Mini App. It helps the developer to find out the characteristics of the Dejoy application, the current device, get basic information about the user and much more.

## Transmission Method
In a web environment, one of the simplest and fastest ways to transfer data between locally running applications is by specifying it in the address bar. A server can include data in the URL's hash, making it accessible to the client application. Similarly, Dejoy Mini Apps use this approach, with the Dejoy client environment sending data to the Mini App through the URL's hash. This enables the Mini App to access essential information upon launch.

The native Dejoy application transmits a list of these parameters in the dynamic part of the URL (in the hash #). To access these parameters, you need to use the [window.location.hash]() property in your JavaScript code.

## Extraction
It is important to remember that [hash]() is a string property, while Dejoy transmits a whole list of properties, after which the question arises about formatting and processing this list. In fact, everything is quite simple.

The native Dejoy application passes the list of launch parameters as query-parameters and saves the resulting string in [window.location.hash](). For this reason, in order to extract the launch parameters, it is enough to perform the following operation:

```typescript
const hash = window.location.hash.slice(1);
console.log(hash); // tgWebAppData=...&tgWebAppVersion=6.2&...

const params = new URLSearchParams(hash);
console.log(params.get('tgWebAppVersion')); // "6.2"
```

```
TIP

However, users have the capability to refresh the current application without exiting it. If the application uses hash routing, it may lose the initial hash after some time. Therefore, it's advisable to save this data during the initial launch of the application.
```

## Parameters List
### tgWebAppVersion
The current Dejoy Mini Apps version used by the native application. This parameter is important to use, for example, before calling the Dejoy Mini Apps [methods](../apps-communication/methods.md) to make sure, they are supported.

### tgWebAppData
Contains data describing the current user, data sign, and also some useful values. To learn more, visit the [Init Data](../launch-parameters/init-data.md) page.

### tgWebAppPlatform
[Dejoy application identifier](../README.md#supported-applications). It can be used as a factor determining the visual style of the application, for example, when, depending on the device, the developer needs to display components that are different visually.

### tgWebAppThemeParams
Parameters of the native Dejoy application [theme](../ui/theming.md). This parameter can be used to style the application even at the moment of rendering the loader.

The value of this parameter is a JSON object converted to the string. To get a more user-friendly value, it is enough to use the ```JSON.parse``` method.

```typescript

const theme = {
  bg_color: '#212121',
  text_color: '#ffffff',
  hint_color: '#aaaaaa',
  link_color: '#8774e1',
  button_color: '#8774e1',
  button_text_color: '#ffffff',
  secondary_bg_color: '#0f0f0f',
};
```

### tgWebAppShowSettings
Parameter used only by Dejoy SDK to show the Settings Button on startup. It has no other meaning to external developers.

### tgWebAppBotInline
This parameter is being added in case the current application is launched in inline mode. This allows calling such Dejoy Mini Apps method as [web_app_switch_inline_query](../apps-communication/methods.md#web_app_switch_inline_query).

### tgWebAppStartParam
Parameter that contains a custom string value passed in the bot or application link. [Learn more](../launch-parameters/start-parameter.md).

### tgWebAppFullscreen
Parameter indicating if the application is currently launched in the fullscreen mode.