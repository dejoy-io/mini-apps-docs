# Miniapp Subpage Sharing and Source Parameter Passing Guide

## Overview

This document describes how to implement subpage sharing functionality in Miniapp and how to track access sources. The complete process includes: setting sharing information in subpages, parameter encoding, App parameter passing, Miniapp receiving and decoding parameters, and finally implementing page navigation and source tracking.

## Core Flow

The entire process can be simply understood as:

1. **Subpage Preparation**: Set sharing information (title, description, image, etc.) and parameters to pass in the subpage's HTML
2. **Parameter Encoding**: Encode the subpage address using Base64
3. **App Passing**: App reads the encoded parameters and adds them to the Miniapp launch URL
4. **Miniapp Receiving**: Miniapp retrieves and decodes parameters when launching
5. **Page Navigation**: Navigate to the page using the decoded address

---

## Detailed Steps

### Step 1: Prepare Subpage Address

First, you need to have a subpage access address. For example:

```
/default-channel/products?product_id=UHJvZHVjdFZhcmlhbnQ6MzQ4
```

> **Note**: This is just an example of an e-commerce page. Your subpage address can be in any format, as long as it conforms to your application's routing rules.

### Step 2: Set Meta Tags in Subpage

Add the following meta tags in the HTML `<head>` section of the subpage:

```html
<!DOCTYPE html>
<html>
<head>
    <!-- 1、Title, displayed with priority in sharing card -->
    <meta property="og:title" content="Your Page Title" />
    
    <!-- 2、Description, displayed with priority in sharing card -->
    <meta property="og:description" content="A brief description of your page." />
    
    <!-- 3、URL, used with priority for WebPage sharing -->
    <meta property="og:url" content="https://www.example.com/page.html" />
    
    <!-- 4、Cover image, used with priority in sharing card -->
    <meta property="og:image" content="https://www.example.com/image.jpg" />
    
    <!-- 5、Miniapp launch parameters, passed to engine via startapp, frontend retrieves via Telegram SDK startParams -->
    <meta property="og:params" content="[Base64 encoded page address]" />
</head>
<body>
    <!-- Page content -->
</body>
</html>
```

#### Key Points

- **og:title**: Title of the sharing card
- **og:description**: Description text of the sharing card
- **og:url**: URL of the sharing link
- **og:image**: Cover image of the sharing card
- **og:params**: This is the most important parameter, which needs to contain the Base64 encoded subpage address

#### Base64 Encoding Example

Assuming your subpage address is:
```
/default-channel/products?product_id=UHJvZHVjdFZhcmlhbnQ6MzQ4
```

After Base64 encoding, you get:
```
L2RlZmF1bHQtY2hhbm5lbC9wcm9kdWN0cz9wcm9kdWN0X2lkPVVISnZaSFZqZEZaaGNtbGhiblE2TXpRNA==
```

Put this encoded value into `og:params`:

```html
<meta property="og:params" content="L2RlZmF1bHQtY2hhbm5lbC9wcm9kdWN0cz9wcm9kdWN0X2lkPVVISnZaSFZqZEZaaGNtbGhiblE2TXpRNA==" />
```

> **Tip**: The value in `og:params` can be a relative path or absolute path, defined by the developer. Since parameter encoding and decoding are both done on the Miniapp side, the App will not parse the content of this value.

### Step 3: App Reads and Passes Parameters (Miniapp developers don't need to focus on this)

When users share a subpage, the App will:

1. Read the `og:params` value from the page meta tags
2. Add this value as the `start_param` parameter to the Miniapp launch URL


### Step 4: Miniapp Launch and Parameter Decoding

When users open the Miniapp through a sharing link, you need to:

1. **Get Launch Parameters**: Extract the `start_param` parameter from the URL (recommended to get via Telegram SDK)
2. **Base64 Decode**: Decode the `start_param` using Base64
3. **Page Navigation**: Navigate to the page using the decoded address

#### Decoding Example

Encoded value:
```
JTJGZGVmYXVsdC1jaGFubmVsJTJGcHJvZHVjdHMlMkZhc2NpaS10ZWUlM0Z2YXJpYW50JTNEVUhKdlpIVmpkRlpoY21saGJuUTZNelE0
```

After decoding:
```
/default-channel/products?product_id=UHJvZHVjdFZhcmlhbnQ6MzQ4
```

#### Implementation Code

**Get via Telegram SDK (Recommended)**:

```javascript
// Ensure Telegram WebApp is initialized
if (window.Telegram && window.Telegram.WebApp) {
    const webApp = window.Telegram.WebApp;
    
    // Get launch parameters
    const startParam = webApp.initDataUnsafe.start_param;
    
    if (startParam) {
        // Base64 decode
        const decodedParam = atob(startParam);
        
        // Navigate to page
        window.location.href = decodedParam;
    }
    
    // Prepare WebApp
    webApp.ready();
} else {
    console.error('Telegram WebApp SDK not loaded');
}
```

---

## Access Source Tracking

### Parameter Description

The following key parameters can be parsed from the launch link for access source tracking:

- **spaceId**: Space identifier
- **appId**: Application identifier

### Complete Launch Link Example with Source Parameters

```
https://x.fashion/#tgWebAppData=auth_date%3D1753809100%26hash%3D2411aec4904805112706a5ee4b6dbb986cd98534b2f079d3c1a172706d2d351c%26signature%3D%26start_param%3DJTJGZGVmYXVsdC1jaGFubmVsJTJGcHJvZHVjdHMlMkZhc2NpaS10ZWUlM0Z2YXJpYW50JTNEVUhKdlpIVmpkRlpoY21saGJuUTZNelE0%26user%3D%7B%22added_to_attachment_menu%22%3Afalse%2C%22allows_write_to_pm%22%3Afalse%2C%22first_name%22%3A%222917069982%22%2C%22id%22%3A2917069982%2C%22is_bot%22%3Afalse%2C%22is_premium%22%3Afalse%2C%22last_name%22%3A%22%22%2C%22username%22%3A%22%40i8w39fnf%3Amt.social%22%2C%22language_code%22%3A%22zh-CN%22%2C%22photo_url%22%3A%22https%3A%2F%2Fmt.social%2F_matrix%2Fmedia%2Fv3%2Fthumbnail%2Fmt.social%2FnaNAHDnDBDqEaDvgoeAfbibm%3Fwidth%3D384%26height%3D384%26method%3Dcrop%26allow_redirect%3Dtrue%22%2C%22provider%22%3A%22matrix%22%2C%22extra%22%3A%7B%22hs%22%3A%22mt.social%22%7D%7D&tgWebAppPlatform=android&tgWebAppStartParam=JTJGZGVmYXVsdC1jaGFubmVsJTJGcHJvZHVjdHMlMkZhc2NpaS10ZWUlM0Z2YXJpYW50JTNEVUhKdlpIVmpkRlpoY21saGJuUTZNelE0&tgWebAppThemeParams={"accent_text_color"%3A"%23FFFFFF"%2C"bg_color"%3A"%23111111"%2C"button_color"%3A"%23A8A8A8"%2C"button_text_color"%3A"%23A8A8A8"%2C"destructive_text_color"%3A"%23FFFFFF"%2C"header_bg_color"%3A"%23527DA3"%2C"hint_color"%3A"%23A8A8A8"%2C"link_color"%3A"%23A8A8A8"%2C"progress_circle_color"%3A"%231C93E3"%2C"secondary_bg_color"%3A"%23222222"%2C"section_header_text_color"%3A"%23FFFFFF"%2C"sheet_scroll_up"%3A"%23E1E4E8"%2C"subtitle_text_color"%3A"%23FFFFFF"%2C"text_color"%3A"%23FFFFFF"%2C"text_err_color"%3A"%23CC4747"}&tgWebAppVersion=8.0&spaceId=!cclujis0:mt.social&appId=2z8fva1nizblqU5B6ysZwzsvqxN
```

### Code for Parsing Access Source Parameters

```javascript
// Parse access source parameters from launch link
function parseSourceParams() {
    const url = window.location.href;
    const hashIndex = url.indexOf('#');
    
    if (hashIndex !== -1) {
        const hashParams = url.substring(hashIndex + 1);
        const params = new URLSearchParams(hashParams);
        
        // Get key parameters
        const spaceId = params.get('spaceId');
        const appId = params.get('appId');
        
        console.log('Space ID:', spaceId);
        console.log('App ID:', appId);
        
        // Permission control based on source parameters
        if (spaceId && appId) {
            // Check if user belongs to specified space
            checkUserPermission(spaceId, appId);
        }
        
        return { spaceId, appId };
    }
}
```

---

## Complete Implementation Example

### Subpage HTML Template

```html
<!DOCTYPE html>
<html>
<head>
    <!-- 1、Title, displayed with priority in sharing card -->
    <meta property="og:title" content="Your Page Title" />
    
    <!-- 2、Description, displayed with priority in sharing card -->
    <meta property="og:description" content="A brief description of your page." />
    
    <!-- 3、URL, used with priority for WebPage sharing -->
    <meta property="og:url" content="https://www.example.com/page.html" />
    
    <!-- 4、Cover image, used with priority in sharing card -->
    <meta property="og:image" content="https://www.example.com/image.jpg" />
    
    <!-- 5、Miniapp launch parameters, passed to engine via startapp, frontend retrieves via Telegram SDK startParams -->
    <meta property="og:params" content="[Base64 encoded page address]" />
    
    <!-- Other meta tags -->
</head>
<body>
    <!-- Page content -->
</body>
</html>
```

### Handle Parameters When Miniapp Launches

```javascript
// Ensure Telegram WebApp is initialized
if (window.Telegram && window.Telegram.WebApp) {
    const webApp = window.Telegram.WebApp;
    
    // Get launch parameters
    const startParam = webApp.initDataUnsafe.start_param;
    
    if (startParam) {
        // Base64 decode
        const decodedParam = atob(startParam);
        
        // Navigate to page
        window.location.href = decodedParam;
    }
    
    // Prepare WebApp
    webApp.ready();
} else {
    console.error('Telegram WebApp SDK not loaded');
}
```

---

## Important Notes

1. **Encoding Consistency**: Ensure the web page and Miniapp use the same Base64 encoding method
2. **URL Encoding**: Note that special characters in URLs may require additional encoding
3. **Parameter Length**: Base64 encoding increases data length, pay attention to parameter length limits
4. **Error Handling**: Add appropriate error handling mechanisms in the Miniapp, for example:
   - Check if Telegram SDK is loaded
   - Check if start_param exists
   - Handle decoding failure cases

---

## Summary

Complete flow for implementing subpage sharing and access source tracking:

1. ✅ Set `og:params` meta tag in subpage HTML (containing Base64 encoded page address)
2. ✅ App automatically reads and passes parameters to launch link
3. ✅ Miniapp retrieves `start_param` via Telegram SDK when launching
4. ✅ Decode parameters using Base64
5. ✅ Navigate to page using decoded address
6. ✅ Optional: Parse `spaceId` and `appId` for access source tracking

By following these steps, you can implement complete subpage sharing functionality!
