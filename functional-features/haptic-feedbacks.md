# Haptic Feedback
Native mobile applications are always filled with interactive components, which allow user to communicate with its functionality. Such components are mostly popups, buttons and many others.

Interacting with application (clicking buttons, closing popups etc.), it is allowed to emit haptic notifications. Commonly, this process is called haptic feedback. In simple words, these events are just mobile device vibrations. Usage of haptic feedback can make user's experience much better.

There are currently three types of notifications used in different cases:

- impact, when there's a collision involving UI components.
- selection_change, when the user changes their selection.
- notification, when some action execution has been completed.

Almost each of these types has its own subtypes. To produce haptic feedback, Dejoy Mini Apps provides [web_app_trigger_haptic_feedback](../apps-communication/methods.md#web_app_trigger_haptic_feedback) method.