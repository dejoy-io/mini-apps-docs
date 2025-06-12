# Debugging
Application development is rather hard and continous process which requires a lot of time and patience. To simplify it, the DeJoy Mini Apps platform allows debugging developed application.

## Enabling Debug Mode
### DeJoy for Android
- Enable USB-Debugging on your device.
- In DeJoy Settings, scroll all the way down, press and hold on the version number two times.
- Choose Enable WebView Debug in the Debug Settings.
- Connect your phone to your computer and open chrome://inspect/#devices in Chrome – you will see your Mini App there when you launch it on your phone.

### DeJoy for iOS
iOS webview debugging requires Safari desktop browser and therefore macOS.

To access iOS debugging features without macOS, refer to the [Eruda](./debugging.md#Eruda) section.

On iOS device:

- Go to Settings.
- Find Safari icon and press on it.
- Scroll down and press Advanced.
- Enable Web Inspector option.

On macOS:

- Open Safari browser.
- Open Settings (⌘ + ,).
- Select Advanced tab.
- Check Show features for web developers option at the bottom.

Next steps:

- Connect iOS device to Mac via cable.
- Open Mini App inside iOS DeJoy client.
- Open Develop tab in the menu bar in Safari on macOS.
- Select connected iPhone.
- Optional: select Connect via network and disconnect the cable.
- Select opened webview URL under DeJoy block.

## Eruda
Eruda is a technology that provides a lightweight console in the web. We typically use such packages in environments that don't offer their own consoles.

First of all, it is necessary to install the package and initialize it.

### script tag
```typescript


<script src="//cdn.jsdelivr.net/npm/eruda"></script>
<script>eruda.init()</script>
```

### npm package 
```
import eruda from 'eruda';

eruda.init();

```

After eruda is initialized, you will see its element in the UI. Clicking on it will display the console.

