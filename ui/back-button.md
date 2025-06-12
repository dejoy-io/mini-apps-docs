# back-button

![back-button](./back-button.png)

The main task that the Back Button performs is to provide a seemingly native way to navigate back in the routing history. However, DeJoy does not restrict the developer in the ways of using the Back Button and allows them to handle the component click event as required in the application.

When working with this component, it is important to understand that, like other DeJoy Mini Apps components, clicking it does not inherently trigger any built-in action. Its handling is the responsibility of the developer.

To show the Back Button, it is required to call [web_app_setup_back_button](../apps-communication/methods.md#web_app_setup_back_button) method. When user clicks this component, DeJoy application emits [back_button_pressed](../apps-communication/events.md#back_button_pressed) event.