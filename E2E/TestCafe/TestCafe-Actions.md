# Click
```
t.click( selector [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being clicked. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Click Action Options.

The following example shows how to use the t.click action to check a checkbox element.
```
import { Selector } from 'testcafe';

const checkbox = Selector('#testing-on-remote-devices');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Click a check box and check its state', async t => {
    await t
        .click(checkbox)
        .expect(checkbox.checked).ok();
});
```

The next example uses the options parameter to set the caret position in an input box.
```
import { Selector } from 'testcafe';

const nameInput = Selector('#developer-name');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Click Input', async t => {
    await t
        .typeText(nameInput, 'Peter Parker')
        .click(nameInput, { caretPos: 5 })
        .pressKey('backspace')
        .expect(nameInput.value).eql('Pete Parker');
});
```

# Double Click
```
t.doubleClick( selector [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being double-clicked. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Click Action Options.

The following example shows how to use the t.doubleClick action to invoke a dialog.
```
import { Selector } from 'testcafe';

const dialog = Selector('#dialog');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Invoke Image Options Dialog', async t => {
    await t
        .doubleClick('#thumbnail')
        .expect(dialog.visible).ok();
});
```
The `t.doubleClick` action will not invoke integrated browser actions such as text selection. Use it to perform double clicks that are processed by webpage elements, not the browser. To select text, use the `t.selectText` action or emulate a key shortcut with `t.pressKey('ctrl+a')`.

# Right Click
```
t.rightClick( selector [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being right-clicked. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Click Action Options.

The following example shows how to use the t.rightClick action to invoke a grid's popup menu.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://www.example.com/`;

test('Popup Menu', async t => {
    await t
        .rightClick('#cell-1-1')
        .expect(Selector('#cell-popup-menu').exists).notOk();
});
```
The `t.rightClick` action will not invoke integrated browser context menus, native editor menus, etc. Use it to perform right clicks that are processed by webpage elements, not the browser.

# Drag Element
The `t.drag` or t.dragToElement actions will not invoke integrated browser actions such as selecting text. Use them to perform drag-and-drop actions that are processed by webpage elements, not the browser. To select text, use `t.selectText`.

## Drag an Element by an Offset
```
t.drag( selector, dragOffsetX, dragOffsetY [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being dragged. See Selecting Target Elements.
- dragOffsetX	Number	An X-offset of the drop coordinates from the mouse pointer's initial position.
- dragOffsetY	Number	An Y-offset of the drop coordinates from the mouse pointer's initial position.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Mouse Action Options.

The following example demonstrates how to use the t.drag action with a slider.
```
import { Selector } from 'testcafe';

const slider = Selector('#developer-rating');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Drag slider', async t => {
    await t
        .click('#i-tried-testcafe');
        .expect(slider.value).eql(1)
        .drag('.ui-slider-handle', 360, 0, { offsetX: 10, offsetY: 10 })
        .expect(slider.value).eql(7);
});
```

## Drag an Element onto Another One
```
t.dragToElement( selector, destinationSelector [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being dragged. See Selecting Target Elements.
- destinationSelector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element that serves as the drop location. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See DragToElement Action Options.

This sample shows how to drop an element into a specific area using the `t.dragToElement` action.
```
import { ClientFunction } from 'testcafe';

fixture `My fixture`
    .page `http://www.example.com/`;

const designSurfaceItems = Selector('.design-surface').find('.items');

test('Drag an item from the toolbox', async t => {
    await t
        .dragToElement('.toolbox-item.text-input', '.design-surface')
        .expect(designSurfaceItems.count).gt(0);
});
```

# Hover
```
t.hover( selector [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element being hovered over. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Mouse Action Options.

The following example shows how to move the mouse pointer over a combo box to display the dropdown list, then select an item and check that the combo box value has changed.
```
import { Selector } from 'testcafe';

const comboBox = Selector('.combo-box');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Select combo box value', async t => {
    await t
        .hover(comboBox)
        .click('#i-prefer-both')
        .expect(comboBox.value).eql('Both');
});
```

# Take Screenshot
## Take a Screenshot of the Entire Page
```
t.takeScreenshot( [options] )
```
- path (optional)	String	The screenshot file's relative path and name. The path is relative to the root directory specified in the runner.screenshots API method or the -s (--screenshots) command line option. This property overrides the relative path specified with the default or custom path patterns.
- fullPage (optional)	Boolean	Specifies that the full page should be captured, including content that is not visible due to overflow.	false

```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

test('Take a screenshot of a fieldset', async t => {
    await t
        .typeText('#developer-name', 'Peter Parker')
        .click('#submit-button')
        .takeScreenshot({
            path:     'my-fixture/thank-you-page.png',
            fullPage: true
        });
});
```

## Take a Screenshot of a Page Element
```
t.takeElementScreenshot(selector[, path][, options])
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element whose screenshot should be taken. See Selecting Target Elements.
- path (optional)	String	The screenshot file's relative path and name. The path is relative to the root directory specified in the runner.screenshots API method or the -s (--screenshots) command line option. This path overrides the relative path the default or custom path patterns specify.
- options (optional)	Object	Options that define how the screenshot is taken. See details below.

```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

test('Take a screenshot of a fieldset', async t => {
    await t
        .click('#reusing-js-code')
        .click('#continuous-integration-embedding')
        .takeElementScreenshot(Selector('fieldset').nth(1), 'my-fixture/important-features.png');
});
```

# Navigate
```
t.navigateTo( url )
```
The URL to navigate to. Absolute or relative to the current page.

```
fixture `My fixture`
    .page `http://www.example.com/`;

test('Navigate to the main page', async t => {
    await t
        .click('#submit-button')
        .navigateTo('http://devexpress.github.io/testcafe');
});
```

You can use the file:// scheme or relative paths to navigate to a webpage in a local directory.
```
fixture `My fixture`
    .page `http://www.example.com/`;

test('Navigate to local pages', async t => {
    await t
        .navigateTo('file:///user/my-website/index.html')
        .navigateTo('../my-project/index.html');
});
```

# Press Key
```
t.pressKey( keys [, options] )
```
- keys	String	The sequence of keys and key combinations to be pressed.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Basic Action Options.

Key Type	Example
- Alphanumeric keys	'a', 'A', '1'
- Modifier keys	'shift', 'alt' (⌥ key on macOS), 'ctrl', 'meta' (meta key on Linux and ⌘ key on macOS)
- Navigation and action keys	'backspace', 'tab', 'enter'
- Key combinations	'shift+a', 'ctrl+d'
- Sequential key presses	Any of the above in a string separated by spaces, for example, 'a ctrl+b'

The following navigation and action keys are supported:
- 'backspace'
- 'tab'
- 'enter'
- 'capslock'
- 'esc'
- 'space'
- 'pageup'
- 'pagedown'
- 'end'
- 'home'
- 'left'
- 'right'
- 'up'
- 'down'
- 'ins'
- 'delete'

## Browser Processing Emulation
The t.pressKey action triggers only page handlers for most keystrokes.

The 'backspace', 'delete', 'left' and 'right' key presses in contentEditable elements are processed only when text is selected.

Text Field-Based Inputs. TestCafe supports selection and navigation with keystrokes in the following input types:
- email
- number
- password
- search
- tel
- text
- url

The following example shows how to use the t.pressKey action:
```
import { Selector } from 'testcafe';

const nameInput = Selector('#developer-name');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Key Presses', async t => {
    await t
        .typeText(nameInput, 'Peter Parker')
        .pressKey('home right . delete delete delete delete')
        .expect(nameInput.value).eql('P. Parker');
});
```

# Select Text
## Select Text in Input Elements
```
t.selectText( selector [, startPos] [, endPos] [, options] )

```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element whose text will be selected. See Selecting Target Elements.
- startPos (optional)	Number	The start position of the selection. A zero-based integer.	0
- endPos (optional)	Number	The end position of the selection. A zero-based integer.	Length of the visible text content.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Basic Action Options.

The following example demonstrates text selection in an input element.
```
import { ClientFunction, Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

const developerNameInput = Selector('#developer-name');

const getElementSelectionStart = ClientFunction(selector => selector().selectionStart);
const getElementSelectionEnd   = ClientFunction(selector => selector().selectionEnd);

test('Select text within input', async t => {
    await t
        .typeText(developerNameInput, 'Test Cafe', { caretPos: 0 })
        .selectText(developerNameInput, 7, 1);

    await t
        .expect(await getElementSelectionStart(developerNameInput)).eql(1)
        .expect(await getElementSelectionEnd(developerNameInput)).eql(7);
});
```
If the startPos value is greater than the endPos value, the action will perform a backward selection.

## Select <textarea> Content
```
t.selectTextAreaContent( selector [, startLine] [, startPos] [, endLine] [, endPos] [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the <textarea> whose text will be selected. See Selecting Target Elements.
- startLine (optional)	Number	The line number at which selection starts. A zero-based integer.	0
- startPos (optional)	Number	The start position of selection within the line defined by the startLine. A zero-based integer.	0
- endLine (optional)	Number	The line number at which selection ends. A zero-based integer.	The index of the last line.
- endPos (optional)	Number	The end position of selection within the line defined by endLine. A zero-based integer.	The last position in endLine.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Basic Action Options.

The following example shows how to select text within a <textarea> element.
```
import { ClientFunction, Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

const commentTextArea = Selector('#comments');

const getElementSelectionStart = ClientFunction(selector => selector().selectionStart);
const getElementSelectionEnd   = ClientFunction(selector => selector().selectionEnd);

test('Select text within textarea', async t => {
    await t
        .click('#tried-test-cafe')
        .typeText(commentTextArea, [
            'Lorem ipsum dolor sit amet',
            'consectetur adipiscing elit',
            'sed do eiusmod tempor'
        ].join(',\n'))
        .selectTextAreaContent(commentTextArea, 0, 5, 2, 10);

    await t
        .expect(await getElementSelectionStart(commentTextArea)).eql(5)
        .expect(await getElementSelectionEnd(commentTextArea)).eql(67);
});
```
If the position defined by startLine and startPos is greater than the one defined by endLine and endPos, the action will perform a backward selection.

## Select within Editable Content
```
t.selectEditableContent( startSelector, endSelector [, options] )
```
- startSelector	Function | String | Selector | Snapshot | Promise	Identifies a webpage element from which selection starts. The start position of selection is the first character of the element's text. See Selecting Target Elements.
- endSelector	Function | String | Selector | Snapshot | Promise	Identifies a webpage element at which selection ends. The end position of selection is the last character of the element's text. See Selecting Target Elements.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Basic Action Options.

According to Web standards, start and end elements must have a common ancestor with the contentEditable attribute set to true - the range root.

The example below shows how to select several sections within a contentEditable area.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://www.example.com/`;

test('Delete text within a contentEditable element', async t => {
    await t
        .selectEditableContent('#foreword', '#chapter-3')
        .pressKey('delete')
        .expect(Selector('#chapter-2').exists).notOk()
        .expect(Selector('#chapter-4').exists).ok();
});
```

# Type Text
```
t.typeText( selector, text [, options] )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the webpage element that will receive input focus. See Selecting Target Elements.
- text	String	The text to be typed into the specified webpage element.
- options (optional)	Object	A set of options that provide additional parameters for the action. See Typing Action Options. If this parameter is omitted, TestCafe sets the cursor to the end of the text before typing, thus preserving the text that is already in the input box.

The t.typeText action clicks the specified element before text is typed if this element is not focused. If the target element is not focused after the click, t.typeText does not type text.

Use the t.selectText and t.pressKey actions to implement operations such as selecting or deleting text.

The following example shows how to use t.typeText with and without options.
```
import { Selector } from 'testcafe';

const nameInput = Selector('#developer-name');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Type and Replace', async t => {
    await t
        .typeText(nameInput, 'Peter')
        .typeText(nameInput, 'Paker', { replace: true })
        .typeText(nameInput, 'r', { caretPos: 2 })
        .expect(nameInput.value).eql('Parker');
});
```

## Typing Into DateTime, Color and Range Inputs
- Input type	Pattern	Example
- Date	yyyy-mm-dd	'2017-12-23'
- Week	yyyy-Www	'2017-W03'
- Month	yyyy-mm	'2017-08'
- DateTime	yyyy-mm-ddThh:mm	'2017-11-03T05:00'
- Time	hh:mm	'15:30'
- Color	#rrggbb	'#003000'
- Range	n	'45'

# Upload
The t.setFilesToUpload and t.clearUpload actions only allow you to manage the list of files for upload. These files are uploaded to the server after you initiate upload, for example, when you click the Upload or Submit button on a webpage.

## Populate File Upload Input
```
t.setFilesToUpload( selector, filePath )
```
- selector	Function | String | Selector | Snapshot | Promise	Identifies the input field to which file paths are written. See Selecting Target Elements.
- filePath	String | Array	The path to the uploaded file, or several such paths. Relative paths are resolved against the folder with the test file.

The following example illustrates how to use the t.setFilesToUpload action.
```
fixture `My fixture`
    .page `http://www.example.com/`;

test('Uploading', async t => {
    await t
        .setFilesToUpload('#upload-input', [
            './uploads/1.jpg',
            './uploads/2.jpg',
            './uploads/3.jpg'
        ])
        .click('#upload-button');
});
```
The t.setFilesToUpload action removes all file paths from the input before populating it with new ones.

## Clear File Upload Input
```
t.clearUpload( selector )
```

```
import { Selector } from 'testcafe';

const uploadBtn = Selector('#upload-button');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Trying to upload with no files specified', async t => {
    await t
        .setFilesToUpload('#upload-input', [
            './uploads/1.jpg',
            './uploads/2.jpg',
            './uploads/3.jpg'
        ])
        .clearUpload('#upload-input')
        .expect(uploadBtn.visible).notOk();
});
```

# Resize Window
## Setting the Window Size in pixels
```
t.resizeWindow( width, height )
```

```
import { Selector } from 'testcafe';

const menu = Selector('#side-menu');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Side menu disappears on small screens', async t => {
    await t
        .resizeWindow(200, 100)
        .expect(menu.getStyleProperty('display')).eql('none');
});
```
## Fitting the Window into a Particular Device
```
t.resizeWindowToFitDevice( deviceName [, options] )
```
- deviceName	String	The name of the device. See the list of supported devices in this repository.
- options (optional)	Object	Provide additional information about the device.
    - portraitOrientation	Boolean	true for portrait screen orientation; false for landscape.

```
import { Selector } from 'testcafe';

const header = Selector('#header');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Header is displayed on Xperia Z in portrait', async t => {
    await t
        .resizeWindowToFitDevice('Sony Xperia Z', {
            portraitOrientation: true
        })
        .expect(header.getStyleProperty('display')).notEql('none');
});
```

## Maximizing the Window
```
t.maximizeWindow( )
```

```
import { Selector } from 'testcafe';

const menu = Selector('#side-menu');

fixture `My fixture`
    .page `http://www.example.com/`;

test('Side menu is displayed in full screen', async t => {
    await t
        .maximizeWindow()
        .expect(menu.visible).ok();
});
```

# Action Options
## Basic Action Options
```
{
    speed: Number
}
```
- Parameter	Type	Description	Default
- speed	Number	The speed of action emulation. Defines how fast TestCafe performs the action when running tests. A number between 1 (the maximum speed) and 0.01 (the minimum speed). If test speed is also specified in the CLI, API or in test code, the action speed setting overrides test speed.	1
```
    .typeText(nameInput, ' Parker', { speed: 0.1 });
```

## Mouse Action Options
```
{
    modifiers: {
        ctrl: Boolean,
        alt: Boolean,
        shift: Boolean,
        meta: Boolean
    },

    offsetX: Number,
    offsetY: Number,
    speed: Number
}
```
- ctrl, alt, shift, meta	Boolean	Indicate which modifier keys are to be pressed during the mouse action.	false
- offsetX, offsetY	Number	Mouse pointer coordinates that define a point where the action is performed or started. If an offset is a positive integer, coordinates are calculated relative to the top-left corner of the target element. If an offset is a negative integer, they are calculated relative to the bottom-right corner.	The center of the target element.
- speed	Number	The speed of action emulation. Defines how fast TestCafe performs the action when running tests. A value between 1 (the maximum speed) and 0.01 (the minimum speed). If test speed is also specified in the CLI or programmatically, the action speed setting overrides test speed.	1

Mouse action options are used in the t.drag and t.hover actions.
```
import { Selector } from 'testcafe';

const sliderHandle = Selector('.ui-slider-handle');

fixture `My Fixture`
    .page `http://devexpress.github.io/testcafe/example/`

test('My Test', async t => {
    await t
        .drag(sliderHandle, 360, 0, {
            offsetX: 10,
            offsetY: 10,
            modifiers: {
                shift: true
            }
        });
});
```

## DragToElement Action Options
```
{
    modifiers: {
        ctrl: Boolean,
        alt: Boolean,
        shift: Boolean,
        meta: Boolean
    },

    offsetX: Number,
    offsetY: Number,
    destinationOffsetX: Number,
    destinationOffsetY: Number,
    speed: Number
}
```

## Click Action Options
```
{
    modifiers: {
        ctrl: Boolean,
        alt: Boolean,
        shift: Boolean,
        meta: Boolean
    },

    offsetX: Number,
    offsetY: Number,
    caretPos: Number,
    speed: Number
}
```

## Typing Action Options
```
{
    modifiers: {
        ctrl: Boolean,
        alt: Boolean,
        shift: Boolean,
        meta: Boolean
    },

    offsetX: Number,
    offsetY: Number,
    caretPos: Number,
    replace: Boolean,
    paste: Boolean,
    speed: Number
}
```

```
import { Selector } from 'testcafe';

const nameInput = Selector('#developer-name');

fixture `My Fixture`
    .page `http://devexpress.github.io/testcafe/example/`

test('My Test', async t => {
    await t
        .typeText(nameInput, 'Peter')
        .typeText(nameInput, 'Parker', { replace: true });
});
```

