---
version: v1.4.6
category: API
redirect_from:
    - /docs/v0.24.0/api/dialog/
    - /docs/v0.25.0/api/dialog/
    - /docs/v0.26.0/api/dialog/
    - /docs/v0.27.0/api/dialog/
    - /docs/v0.28.0/api/dialog/
    - /docs/v0.29.0/api/dialog/
    - /docs/v0.30.0/api/dialog/
    - /docs/v0.31.0/api/dialog/
    - /docs/v0.32.0/api/dialog/
    - /docs/v0.33.0/api/dialog/
    - /docs/v0.34.0/api/dialog/
    - /docs/v0.35.0/api/dialog/
    - /docs/v0.36.0/api/dialog/
    - /docs/v0.36.3/api/dialog/
    - /docs/v0.36.4/api/dialog/
    - /docs/v0.36.5/api/dialog/
    - /docs/v0.36.6/api/dialog/
    - /docs/v0.36.7/api/dialog/
    - /docs/v0.36.8/api/dialog/
    - /docs/v0.36.9/api/dialog/
    - /docs/v0.36.10/api/dialog/
    - /docs/v0.36.11/api/dialog/
    - /docs/v0.37.0/api/dialog/
    - /docs/v0.37.1/api/dialog/
    - /docs/v0.37.2/api/dialog/
    - /docs/v0.37.3/api/dialog/
    - /docs/v0.37.4/api/dialog/
    - /docs/v0.37.5/api/dialog/
    - /docs/v0.37.6/api/dialog/
    - /docs/v0.37.7/api/dialog/
    - /docs/v0.37.8/api/dialog/
    - /docs/latest/api/dialog/
source_url: 'https://github.com/electron/electron/blob/master/docs/api/dialog.md'
excerpt: "Display native system dialogs for opening and saving files, alerting, etc."
title: "dialog"
sort_title: "dialog"
---

# dialog

> Display native system dialogs for opening and saving files, alerting, etc.

Process: [Main](http://electron.atom.io/docs/tutorial/quick-start#main-process)

An example of showing a dialog to select multiple files and directories:

```javascript
const {dialog} = require('electron')
console.log(dialog.showOpenDialog({properties: ['openFile', 'openDirectory', 'multiSelections']}))
```

The Dialog is opened from Electron's main thread. If you want to use the dialog
object from a renderer process, remember to access it using the remote:

```javascript
const {dialog} = require('electron').remote
console.log(dialog)
```

## Methods

The `dialog` module has the following methods:

### `dialog.showOpenDialog([browserWindow, ]options[, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object
  * `title` String
  * `defaultPath` String
  * `buttonLabel` String - Custom label for the confirmation button, when
    left empty the default label will be used.
  * `filters` String[]
  * `properties` String[] - Contains which features the dialog should use, can
    contain `openFile`, `openDirectory`, `multiSelections`, `createDirectory`
    and `showHiddenFiles`.
* `callback` Function (optional)
  * `filePaths` String[] - An array of file paths chosen by the user

On success this method returns an array of file paths chosen by the user,
otherwise it returns `undefined`.

The `filters` specifies an array of file types that can be displayed or
selected when you want to limit the user to a specific type. For example:

```javascript
{
  filters: [
    {name: 'Images', extensions: ['jpg', 'png', 'gif']},
    {name: 'Movies', extensions: ['mkv', 'avi', 'mp4']},
    {name: 'Custom File Type', extensions: ['as']},
    {name: 'All Files', extensions: ['*']}
  ]
}
```

The `extensions` array should contain extensions without wildcards or dots (e.g.
`'png'` is good but `'.png'` and `'*.png'` are bad). To show all files, use the
`'*'` wildcard (no other wildcard is supported).

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(filenames)`

**Note:** On Windows and Linux an open dialog can not be both a file selector
and a directory selector, so if you set `properties` to
`['openFile', 'openDirectory']` on these platforms, a directory selector will be
shown.

### `dialog.showSaveDialog([browserWindow, ]options[, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object
  * `title` String
  * `defaultPath` String
  * `buttonLabel` String - Custom label for the confirmation button, when
    left empty the default label will be used.
  * `filters` String[]
* `callback` Function (optional)
  * `filename` String

On success this method returns the path of the file chosen by the user,
otherwise it returns `undefined`.

The `filters` specifies an array of file types that can be displayed, see
`dialog.showOpenDialog` for an example.

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(filename)`

### `dialog.showMessageBox([browserWindow, ]options[, callback])`

* `browserWindow` BrowserWindow (optional)
* `options` Object
  * `type` String - Can be `"none"`, `"info"`, `"error"`, `"question"` or
  `"warning"`. On Windows, "question" displays the same icon as "info", unless
  you set an icon using the "icon" option.
  * `buttons` String[] - Array of texts for buttons. On Windows, an empty array
    will result in one button labeled "OK".
  * `defaultId` Integer - Index of the button in the buttons array which will
    be selected by default when the message box opens.
  * `title` String - Title of the message box, some platforms will not show it.
  * `message` String - Content of the message box.
  * `detail` String - Extra information of the message.
  * `icon` [NativeImage](http://electron.atom.io/docs/api/native-image)
  * `cancelId` Integer - The value will be returned when user cancels the dialog
    instead of clicking the buttons of the dialog. By default it is the index
    of the buttons that have "cancel" or "no" as label, or 0 if there is no such
    buttons. On macOS and Windows the index of "Cancel" button will always be
    used as `cancelId`, not matter whether it is already specified.
  * `noLink` Boolean - On Windows Electron will try to figure out which one of
    the `buttons` are common buttons (like "Cancel" or "Yes"), and show the
    others as command links in the dialog. This can make the dialog appear in
    the style of modern Windows apps. If you don't like this behavior, you can
    set `noLink` to `true`.
* `callback` Function
  * `response` Number - The index of the button that was clicked

Shows a message box, it will block the process until the message box is closed.
It returns the index of the clicked button.

If a `callback` is passed, the API call will be asynchronous and the result
will be passed via `callback(response)`.

### `dialog.showErrorBox(title, content)`

* `title` String - The title to display in the error box
* `content` String - The text content to display in the error box

Displays a modal dialog that shows an error message.

This API can be called safely before the `ready` event the `app` module emits,
it is usually used to report errors in early stage of startup.  If called
before the app `ready`event on Linux, the message will be emitted to stderr,
and no GUI dialog will appear.

## Sheets

On macOS, dialogs are presented as sheets attached to a window if you provide
a `BrowserWindow` reference in the `browserWindow` parameter, or modals if no
window is provided.

You can call `BrowserWindow.getCurrentWindow().setSheetOffset(offset)` to change
the offset from the window frame where sheets are attached.
