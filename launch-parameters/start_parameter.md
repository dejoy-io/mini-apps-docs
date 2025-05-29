# Start Parameter
The start parameter is a custom string parameter passed to a Mini App from an external environment. Its value is stored in the ```tgWebAppStartParam``` launch parameter.

This parameter is included in one of the following cases:

- The bot link contains the startattach query parameter.
```
https://t.me/botusername?startattach=ABC
```
- The Direct Link contains the startapp query parameter.
```
https://t.me/botusername/appname?startapp=ABC
```
In both cases mentioned above, the start parameter will be set to ```ABC```.
```
INFO

This launch parameter is not included in the location hash. Instead, it can be found in the URL query parameters.
```
```
TIP

The value of this parameter is duplicated in the init data's start_param property.
```
## Restrictions
- Only the following characters are allowed: ```A-Z```, ```a-z```, ```0-9```, ```_ ```(underscore) and the ```-``` (minus). We recommend using [base64url](https://base64.guru/standards/base64url) to encode parameters with binary and other types of content. The parameter can be up to ```512``` characters long.
- You can validate start parameter with the following RegExp: ```/^[\w-]{0,512}$/````