# Selectors
If a selector initializer has several matching DOM nodes on the page, the selector returns the first node from the matched set.

Import the Selector constructor from the `testcafe` module. Call this constructor and pass a CSS selector string as an argument.
```
import { Selector } from 'testcafe';
const article = Selector('.article-content');
```

You can write a selector that matches several page elements and then filter them by text, attribute, etc.
```
const windowsRadioButton  = Selector('.radio-button').withText('Windows');
const selectedRadioButton = Selector('.radio-button').withAttribute('selected');
```

If you need to find a specific element in the DOM tree, you can search for it using the selector API.
```
const buttonWrapper = Selector('.article-content').find('#share-button').parent();
```

## Creating Selectors
Use the Selector constructor to create a selector.
```
Selector( init [, options] )
```
init can be Function | String | Selector | Snapshot | Promise

A CSS selector string that matches one or several nodes.
```
// A selector is created from a CSS selector string.
const submitButton = Selector('#submit-button');
```

A regular function executed on the client side. This function must return a DOM node, an array of DOM nodes, NodeList, HTMLCollection, null or undefined.
```
import { Selector } from 'testcafe';

// A selector is created from a regular function.
// This selector will take an element by id that is saved in the localStorage.
const element = Selector(() => {
    const storedElementId = window.localStorage.storedElementId;

    return document.getElementById(storedElementId);
});
```

You can provide a function that takes arguments, and then pass serializable objects to the selector when you call it.
```
import { Selector } from 'testcafe';

const elementWithId = Selector(id => {
    return document.getElementById(id);
});

fixture `My fixture`
    .page `http://www.example.com/`;

test('My Test', async t => {
    await t.click(elementWithId('buy'));
});
```

Selector can created based on the previous selector and inherits
```
// This selector is created from a CSS selector
// that matches all elements of a specified class.
const ctaButton = Selector('.cta-button');

// This selector is created based on the previous selector and inherits
// its initializer, but overwrites the `visibilityCheck` parameter.
Selector(ctaButton, { visibilityCheck: true });
```

A DOM Node Snapshot returned by selector execution.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://www.example.com/`;

test('My Test', async t => {
    const topMenuSnapshot = await Selector('#top-menu')();

    // This selector is created from a DOM Node state object returned
    // by a different selector. The new selector will use the same initializer
    // as 'elementWithId' and will always be executed with the same parameter
    // values that were used to obtain 'topMenuSnapshot'. You can still
    // overwrite the selector options.
    const visibleTopMenu = Selector(topMenuSnapshot, {
        visibilityCheck: true
    });
});
```

A promise returned by a selector.
```
import { Selector } from 'testcafe';

const elementWithIdOrClassName = Selector(value => {
    return document.getElementById(value) || document.getElementsByClassName(value);
});

fixture `My fixture`
    .page `http://www.example.com/`;

test('My Test', async t => {
    // This selector is created from a promise returned by a call to a
    // different selector. The new selector will be initialized with the
    // same function as the old one and with hard-coded parameter values
    // as in the previous example.
    const submitButton = Selector(elementWithIdOrClassName('main-element'));
});
```

## Using Selectors
### Check if an Element Exists.
Selector has properties:
- exists	Boolean	true if at least one matching element exists.
- count	    Number	The number of matching elements.
```
import { Selector } from 'testcafe';

fixture `Example page`
    .page `http://devexpress.github.io/testcafe/example/`;

test('My test', async t => {
    const osCount            = Selector('.column.col-2 label').count;
    const submitButtonExists = Selector('#submit-button').exists;

    await t
        .expect(osCount).eql(3)
        .expect(submitButtonExists).ok();
});
```

### Obtain Element State
Selectors and promises returned by selectors expose API to get the state (size, position, classes, etc.) of the matching element.
```
const headerText = await Selector('#header').textContent;
```

```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page('http://devexpress.github.io/testcafe/example/');

const windowsInput = Selector('#windows');

test('Obtain Element State', async t => {
    await t.click(windowsInput);

    const windowsInputChecked = await windowsInput.checked; // returns true
});
```

### DOM Node Snapshot
If you need to get the entire DOM element state, call the selector with the `await` keyword like you would do with regular async functions. It returns a DOM Node Snapshot that contains all property values exposed by the selector in a single object.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

test('DOM Node Snapshot', async t => {
    const sliderHandle         = Selector('#slider').child('span');
    const sliderHandleSnapshot = await sliderHandle();

    console.log(sliderHandleSnapshot.hasClass('ui-slider-handle'));    // => true
    console.log(sliderHandleSnapshot.childElementCount);               // => 0
});
```

### Define Action Targets
You can pass selectors to test actions to use the returned element as the action target.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

const label = Selector('#tried-section').child('label');

test('My Test', async t => {
    await t.click(label);
});
```

DOM element snapshots can also be passed to test actions.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

const label = Selector('#tried-section').child('label');

test('My Test', async t => {
    const labelSnapshot = await label();

    await t.click(labelSnapshot);
});
```
Note that if a selector initializer has multiple matching DOM nodes on the page, the action will be executed only for the first node from the matched set.

### Define Assertion Actual Value
You can check whether a particular DOM node has the expected state by passing a selector property directly to assertions.
```
import { Selector } from 'testcafe';

fixture `My fixture`
    .page `http://devexpress.github.io/testcafe/example/`;

test('Assertion with Selector', async t => {
    const developerNameInput = Selector('#developer-name');

    await t
        .expect(developerNameInput.value).eql('')
        .typeText(developerNameInput, 'Peter')
        .expect(developerNameInput.value).eql('Peter');
});
```

### Selector Timeout
When a selector is called in test code, TestCafe waits for the target node to appear in the DOM within the selector timeout. You can specify the selector timeout in test code by using the timeout option. To set this timeout when launching tests, pass it to the runner.run method if you use API or specify the selector-timeout option if you run TestCafe from the command line.

### Debug Selectors
TestCafe outputs information about failed selectors to test run reports.

When you try to use a selector that does not match any DOM element, the test fails and an error is thrown. The error message indicates which selector has failed.

An error can also occur when you call selector's methods in a chain. These methods are applied to the selector one by one. TestCafe detects a method after which the selector no longer matches any DOM element. This method is highlighted in the error message.

## Functional-Style Selectors
### Filter DOM Nodes
If a selector returns multiple DOM nodes, you can filter them to select a single node that will eventually be returned by the selector.

- nth(index)
    - Finds an element by its index in the matched set. The index parameter is zero-based. If index is negative, the index is counted from the end of the matched set.
```
// Selects the third ul element.
Selector('ul').nth(2);

// Selects the last div element.
Selector('div').nth(-1);
```

- withText
    - withText(text)	Creates a selector that filters a matched set by the specified text. Selects elements that **contain** this text. To filter elements by strict match, use the withExactText method. The text parameter is case-sensitive.
    - withText(re)	Creates a selector that filters a matched set using the specified regular expression.
```
// Selects label elements that contain 'foo'.
// Matches 'foo', 'foobar'. Does not match 'bar', 'Foo'.
Selector('label').withText('foo');

// Selects div elements whose text matches
// the /a[b-e]/ regular expression.
// Matches 'ab', 'ac'. Does not match 'bb', 'aa'.
Selector('div').withText(/a[b-e]/);
```
Note that when withText filters the matched set, it leaves not only the element that immediately contains the specified text but also its ancestors. Assume the following markup.
```
<div class="container">
    <div class="child">some text</div>
</div>
```
In this instance, a selector that targets div elements with the 'some text' text will match both elements (first, the parent and then, the child).

- withExactText
 - Creates a selector that filters a matched set by the specified text. Selects elements whose text content strictly matches this text. To search for elements that contain a specific text, use the withText method. The text parameter is case-sensitive.
```
// Selects elements of the 'container' class
// whose text exactly matches 'foo'.
// Does not match 'bar', 'foobar', 'Foo'.
Selector('.container').withExactText('foo');
```
Note that when `withExactText` filters the matched set, it leaves not only the element that immediately contains the specified text but also its ancestors (if they do not contain any other text). See an example for withText.

- withAttribute`(attrName [, attrValue])`
    - Finds elements that contain the specified attribute and, optionally, attribute value.
    - If attrName or attrValue is a String, withAttribute selects an element by strict match.
    - The attribute name. This parameter is case-sensitive.
    - The attribute value. This parameter is case-sensitive. You can omit it to select elements that have the attrName attribute regardless of the value.
```
// Selects div elements that have the 'myAttr' attribute.
// This attribute can have any value.
Selector('div').withAttribute('myAttr');

// Selects div elements whose 'attrName' attribute
// is set to 'foo'. Does not match
// the 'otherAttr' attribute, or the 'attrName' attribute
// with the 'foobar' value.
Selector('div').withAttribute('attrName', 'foo');

// Selects ul elements that have an attribute whose
// name matches the /[123]z/ regular expression.
// This attribute must have a value that matches
// the /a[0-9]/ regular expression.
// Matches the '1z' and '3z' attributes with the
// 'a0' and 'a7' values.
// Does not match the '4z' or '1b' attribute,
// as well as any attribute with the 'b0' or 'ab' value.
Selector('ul').withAttribute(/[123]z/, /a[0-9]/);
```

- filterVisible
    - Creates a selector that filters a matched set leaving only visible elements. These are elements that do not have display: none or visibility: hidden CSS properties and have non-zero width and height.
```
// Selects all visible div elements.
Selector('div').filterVisible();
```

- filterHidden
    - Creates a selector that filters a matched set leaving only hidden elements. These are elements that have a display: none or visibility: hidden CSS property or zero width or height.
```
// Selects all hidden label elements.
Selector('label').filterHidden();
```

- filter
    - filter(cssSelector)	Finds elements that match the cssSelector.
    - filter(filterFn, dependencies)	Finds elements that satisfy the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function.
    - filter((node, idx)
        - node	The current DOM node.
        - idx	Index of the current node among other nodes in the matched set.

```
// Selects li elements that
// have the someClass class.
Selector('li').filter('.someClass')
```

````
Selector('ul').filter((node, idx) => {
    // node === a <ul> node
    // idx === index of the current <ul> node
});
````

The dependencies parameter allows you to pass objects to the filterFn client-side scope where they appear as variables.
```
import { Selector } from 'testcafe';

fixture `Example page`
    .page `http://devexpress.github.io/testcafe/example/`;

test('My test', async t => {
    const secondCheckBox = Selector('input')
        .withAttribute('type', 'checkbox')
        .nth(1);

    const checkedInputs = Selector('input')
        .withAttribute('type', 'checkbox')
        .filter(node => node.checked);

    const windowsLabel = Selector('label')
        .withText('Windows');

    await t
        .click(secondCheckBox)
        .expect(checkedInputs.count).eql(1)
        .click(windowsLabel);
});
```

### Search for Elements in the DOM Hierarchy
The selector API provides methods to find elements in a DOM hierarchy in jQuery style.

- find
    - find(cssSelector)	Finds all descendants of all nodes in the matched set and filters them by cssSelector.
    - find(filterFn, dependencies)	Finds all descendants of all nodes in the matched set and filters them using filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects input elements that are descendants
// of div elements with the someClass class.
Selector('div.someClass').find('input');
```

- parent
    - parent()	Finds all parents of all nodes in the matched set (first element in the set will be the closest parent).
    - parent(index)	Finds all parents of all nodes in the matched set and filters them by index (0 is the closest). If index is negative, the index is counted from the end of the matched set.
    - parent(cssSelector)	Finds all parents of all nodes in the matched set and filters them by cssSelector.
    - parent(filterFn, dependencies)	Finds all parents of all nodes in the matched set and filters them by the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects all ancestors of all ul elements.
Selector('ul').parent();

// Selects all closest parents of all input elements.
Selector('input').parent(0);

// Selects all furthest ancestors of all labels.
Selector('label').parent(-1);

// Selects all divs that are ancestors of an 'a' element.
Selector('a').parent('div');
```

- child
    - child()	Finds all child elements (not nodes) of all nodes in the matched set.
    - child(index)	Finds all child elements (not nodes) of all nodes in the matched set and filters them by index. The index parameter is zero-based. If index is negative, the index is counted from the end of the matched set.
    - child(cssSelector)	Finds all child elements (not nodes) of all nodes in the matched set and filters them by cssSelector.
    - child(filterFn, dependencies)	Finds all child elements (not nodes) of all nodes in the matched set and filters them by the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects all children of all ul elements.
Selector('ul').child();

// Selects all closest children of all div elements.
Selector('div').child(0);

// Selects all furthest children of all table elements.
Selector('table').child(-1);

// Selects all ul elements that are children of a nav element.
Selector('nav').child('ul');
```

- sibling
    - sibling()	Finds all sibling elements (not nodes) of all nodes in the matched set.
    - sibling(index)	Finds all sibling elements (not nodes) of all nodes in the matched set and filters them by index. Elements are indexed as they appear in their parents' childNodes collections. The index parameter is zero-based. If index is negative, the index is counted from the end of the matched set.
    - sibling(cssSelector)	Finds all sibling elements (not nodes) of all nodes in the matched set and filters them by cssSelector.
    - sibling(filterFn, dependencies)	Finds all sibling elements (not nodes) of all nodes in the matched set and filters them by the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects all siblings of all td elements.
Selector('td').sibling();

// Selects all li elements' siblings
// that go first in their parent's child lists.
Selector('li').sibling(0);

// Selects all ul elements' siblings
// that go last in their parent's child lists.
Selector('ul').sibling(-1);

// Selects all p elements that are siblings of an img element.
Selector('img').sibling('p');
```

- nextSibling
    - nextSibling()	Finds all succeeding sibling elements (not nodes) of all nodes in the matched set.
    - nextSibling(index)	Finds all succeeding sibling elements (not nodes) of all nodes in the matched set and filters them by index. Elements are indexed beginning from the closest sibling. The index parameter is zero-based. If index is negative, the index is counted from the end of the matched set.
    - nextSibling(cssSelector)	Finds all succeeding sibling elements (not nodes) of all nodes in the matched set and filters them by cssSelector.
    - nextSibling(filterFn, dependencies)	Finds all succeeding sibling elements (not nodes) of all nodes in the matched set and filters them by the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects all succeeding siblings of all 'a' elements.
Selector('a').nextSibling();

// Selects all closest succeeding siblings of all div elements.
Selector('div').nextSibling(0);

// Selects all furthest succeeding siblings of all pre elements.
Selector('pre').nextSibling(-1);

// Selects all p elements that are succeeding siblings of an hr element.
Selector('hr').nextSibling('p');
```

- prevSibling
    - prevSibling()	Finds all preceding sibling elements (not nodes) of all nodes in the matched set.
    - prevSibling(index)	Finds all preceding sibling elements (not nodes) of all nodes in the matched set and filters them by index. Elements are indexed beginning from the closest sibling. The index parameter is zero-based. If index is negative, the index is counted from the end of the matched set.
    - prevSibling(cssSelector)	Finds all preceding sibling elements (not nodes) of all nodes in the matched set and filters them by cssSelector.
    - prevSibling(filterFn, dependencies)	Finds all preceding sibling elements (not nodes) of all nodes in the matched set and filters them by the filterFn predicate. Use an optional dependencies parameter to pass functions, variables or objects to the filterFn function. See Filtering DOM Elements by Predicates.
```
// Selects all preceding siblings of all p elements.
Selector('p').prevSibling();

// Selects all closest preceding siblings of all figure elements.
Selector('figure').prevSibling(0);

// Selects all furthest preceding siblings of all option elements.
Selector('option').prevSibling(-1);

// Selects all p elements that are preceding siblings of a blockquote element.
Selector('blockquote').prevSibling('p');
```

### Filtering DOM Elements by Predicates
Functions that search for elements through the DOM tree allow you to filter the matched set by a filterFn predicate.
- node	The current matching node.
- idx	A zero-based index of node among other matching nodes.
- originNode	A node from the left-hand selector's matched set whose parents/siblings/children are being iterated.
```
Selector('section').prevSibling((node, idx, originNode) => {
    // node === the <section>'s preceding sibling node
    // idx === index of the current <section>'s preceding sibling node
    // originNode === the <section> element
});


const isNodeOk = (node, idx, originNode) => { /*...*/ };

Selector('ul').prevSibling((node, idx, originNode) => {
    return isNodeOk(node, idx, originNode);
}, { isNodeOk });
```

### Examples
Finds all ul elements on page. Then, in each found ul element finds label elements. Then, for each label element finds a parent that matches the div.someClass selector.
```
Selector('ul').find('label').parent('div.someClass')
```

Like in jQuery, if you request a property of the matched set or try to evaluate a snapshot, the selector returns values for the first element in the set.
```
// Returns id of the first element in the set
const id = await Selector('ul').find('label').parent('div.someClass').id;

// Returns snapshot for the first element in the set
const snapshot = await Selector('ul').find('label').parent('div.someClass')();
```

However, you can obtain data for any element in the set by using nth filter.
```
// Returns id of the third element in the set
const id = await Selector('ul').find('label').parent('div.someClass').nth(2).id;

// Returns snapshot for the fourth element in the set
const snapshot = await Selector('ul').find('label').parent('div.someClass').nth(4)();
```

Note that you can add text and index filters in the selector chain.
```
Selector('.container').parent(1).nth(0).find('.content').withText('yo!').child('span');
```
In this example the selector:
1. finds the second parent (parent of parent) of .container elements;
2. picks the first element in the matched set;
3. in that element, finds elements that match the .content selector;
4. filters them by text yo!;
5. in each filtered element, searches for a child with tag name span.

The following example shows how to use functional-style selection in test code.
```
import { Selector } from 'testcafe';

fixture `Example page`
    .page `http://devexpress.github.io/testcafe/example/`;

test('My test', async t => {
    const macOSRadioButton = Selector('.column.col-2').find('label').child(el => el.value === 'MacOS');

    await t
        .click(macOSRadioButton.parent())
        .expect(macOSRadioButton.checked).ok();
});
```

## Selector Options
### options.boundTestRun
If you need to call a selector from a Node.js callback, assign the current test controller to the boundTestRun option.

### options.dependencies
Use this option to pass functions, variables or objects to selectors initialized with a function. The dependencies object's properties are added to the function's scope as variables.

### options.timeout
The amount of time, in milliseconds, allowed for an element returned by the selector to appear in the DOM before the test fails.

### options.visibilityCheck
true to additionally require the returned element to become visible within options.timeout.

### Overwrite Options
You can use the selector's `with` function to overwrite its options.

## Extending Selectors
TestCafe allows you to extend element state with custom properties and methods executed on the client side.

## Edge Cases and Limitations

# DOM Node State
## Members Common Across All Nodes
- childElementCount	Number	The number of child HTML elements.
- childNodeCount	Number	The number of child nodes.
- hasChildElements	Boolean	true if this node has child HTML elements.
- hasChildNodes	Boolean	true if this node has child nodes.
- nodeType	Number	The type of the node. See Node.nodeType.
- textContent	String	The text content of the node and its descendants. See Node.textContent.
- hasClass(className)	Boolean	true if the element has the specified class name.

## Members Specific to Element Nodes
Property
- attributes	Object	Attributes of the element as { name: value, ... }. You can also use the getAttribute method to access attribute values.
- boundingClientRect	Object	The size of the element and its position relative to the viewport. Contains the left, right, bottom, top, width and height properties. You can also use the getBoundingClientRectProperty method to access these properties.
- checked	Boolean	For checkbox and radio input elements, their current state. For other elements, undefined.
- classNames	Array of String	The list of element's classes.
- clientHeight	Number	The inner height of the element, including padding but not the horizontal scrollbar height, border, or margin. See Element.clientHeight.
- clientLeft	Number	The width of the left border of the element. See Element.clientLeft.
- clientTop	Number	The width of the top border of the element. See Element.clientTop.
- clientWidth	Number	The inner width of the element, including padding but not the vertical scrollbar width, border, or margin. See Element.clientWidth.
- focused	Boolean	true if the element is focused.
- id	String	The element's identifier. See Element.id.
- innerText	String	The element's text content "as rendered". See The innerText IDL attribute.
- namespaceURI	String	The namespace URI of the element. If the element does not have a namespace, this property is set to null. See Element.namespaceURI.
- offsetHeight	Number	The height of the element including vertical padding and borders. See HTMLElement.offsetHeight.
- offsetLeft	Number	The number of pixels that the upper left corner of the element is offset by to the left within the offsetParent node. See HTMLElement.offsetLeft.
- offsetTop	Number	The number of pixels that the upper left corner of the element is offset by to the top within the offsetParent node. See HTMLElement.offsetTop.
- offsetWidth	Number	The width of the element including vertical padding and borders. See HTMLElement.offsetWidth.
- selected	Boolean	Indicates that <option> element is currently selected. For other elements, undefined. See HTMLOptionElement.
- selectedIndex	Number	For <select> element, the index of the first selected <option> element. For other elements, undefined. See HTMLSelectElement.selectedIndex.
- scrollHeight	Number	The height of the element's content, including content not visible on the screen due to overflow. See Element.scrollHeight.
- scrollLeft	Number	The number of pixels that the element's content is scrolled to the left. See Element.scrollLeft.
- scrollTop	Number	The number of pixels that the element's content is scrolled upward. See Element.scrollTop.
- scrollWidth	Number	Either the width in pixels of the element's content or the width of the element itself, whichever is greater. See Element.scrollWidth.
- style	Object	The computed values of element's CSS properties as { property: value, ... }. You can also use the getStyleProperty method to access CSS properties.
- tagName	String	The name of the element. See Element.tagName.
- value	String	For input elements, the current value in the control. For other elements, undefined.
- visible	Boolean	true if the element is visible, i.e. does not have display: none or visibility: hidden CSS properties and has non-zero width and height.

Method
- getStyleProperty(propertyName)	Object	Returns the computed value of the CSS propertyName property. You can also use the style property to access a hash table of CSS properties.
- getAttribute(attributeName)	String	Returns the value of the attributeName attribute. You can also use the attributes property to access a hash table of attributes.
- getBoundingClientRectProperty(propertyName)	Number	Returns the value of the propertyName property from the boundingClientRect object.
- hasAttribute(attributeName)	Boolean	true if the element has the attributeName attribute. Use the getAttribute method to obtain the attribute value.

# Framework-Specific Selectors
## React
https://github.com/DevExpress/testcafe-react-selectors/blob/master/README.md

## Angular
https://github.com/DevExpress/testcafe-angular-selectors/blob/master/angular-selector.md

# Examples of Working with DOM Elements
## Access Page Element Properties
## Get a Page Element Using Custom Logic
## Access Child Nodes in the DOM Hierarchy
## Access Shadow DOM
## Check if an Element is Available
## Enumerate Elements Identified by a Selector
## Select Elements With Dynamic IDs

