# API
https://devexpress.github.io/testcafe/documentation/test-api/test-code-structure.html


# Test Code Structure
## Fixtures
TestCafe tests must be organized into categories called fixtures. A JavaScript, TypeScript or CoffeeScript file with TestCafe tests can contain one or more fixtures.
```
fixture( fixtureName )
fixture `fixtureName`
```
This function returns the fixture object that allows you to configure the fixture - specify the start webpage, metadata and initialization and clean-up code for tests included in the fixture.

## Tests
```
test( testName, fn(t) )

fixture `MyFixture`;

test('Test1', async t => {
    /* Test 1 Code */
});

test('Test2', async t => {
    /* Test 2 Code */
});
```

### Test Controller
A test controller object t exposes the test API's methods. That is why it is passed to each function that is expected to contain server-side test code (like test, beforeEach or afterEach).

Using Test Controller Outside of Test Code 
```
import { Selector, t } from 'testcafe';

class Page {
    constructor () {
        this.loginInput    = Selector('#login');
        this.passwordInput = Selector('#password');
        this.signInButton  = Selector('#sign-in-button');
    }
    async login () {
        await t
            .typeText(this.loginInput, 'MyLogin')
            .typeText(this.passwordInput, 'Pa$$word')
            .click(this.signInButton);
    }
}

export default new Page();
```
TestCafe implicitly resolves test context and provides the right test controller.

### Setting Test Speed
Factor specifies the test speed. Must be a number between 1 (the fastest) and 0.01 (the slowest).
```
t.setTestSpeed( factor )
```

### Setting Page Load Timeout
The page load timeout defines the time passed after the `DOMContentLoaded` event within which the `window.load` event should be raised.
```
t.setPageLoadTimeout( duration )
```
Page load timeout (in milliseconds). 0 to skip waiting for the window.load event.

## Specifying the Start Webpage
You can specify the web page where all tests in a fixture start using the fixture.page function.
```
fixture.page( url )
fixture.page `url`
```
Similarly, you can specify a start page for individual tests using the test.page function that overrides the fixture.page.
```
test.page( url )
test.page `url`
```

Test page override fixture page.
```
fixture `MyFixture`
    .page `http://devexpress.github.io/testcafe/example`;

test('Test1', async t => {
    // Starts at http://devexpress.github.io/testcafe/example
});

test
    .page `http://devexpress.github.io/testcafe/blog/`
    ('Test2', async t => {
        // Starts at http://devexpress.github.io/testcafe/blog/
    });
```
If the start page is not specified, it defaults to about:blank.

## Specifying Testing Metadata
To define metadata, use the meta method. You can call this method for a fixture and a test.
```
fixture.meta('key1', 'value1')
test.meta('key2', 'value2')
fixture.meta({ key1: 'value1', key2: 'value2', key3: 'value3' })
test.meta({ key4: 'value1', key5: 'value2', key6: 'value3' })
```
- Run Tests by Metadata
- Using Metadata in Reports

## Initialization and Clean-Up
### Test Hooks
You can specify a hook for each test in a fixture using the beforeEach and afterEach methods in the fixture declaration.
```
fixture.beforeEach( fn(t) )
fixture.afterEach( fn(t) )
test.before( fn(t) )
test.after( fn(t) )
```

If `test.before` or `test.after` is specified, it overrides the corresponding `fixture.beforeEach` and `fixture.afterEach` hook, so that the latter are not executed.
```
fixture `My fixture`
    .page `http://example.com`
    .beforeEach( async t => {
        /* test initialization code */
    })
    .afterEach( async t => {
        /* test finalization code */
    });

test
    .before( async t => {
        /* test initialization code */
    })
    ('MyTest', async t => { /* ... */ })
    .after( async t => {
        /* test finalization code */
    });
```

### Sharing Variables Between Test Hooks and Test Code
You can share variables between test hook functions and test code by using the test context object. Test context is available through the `t.ctx` property.
```
fixture `Fixture1`
    .beforeEach(async t  => {
        t.ctx.someProp = 123;
    });

test
    ('Test1', async t => {
        console.log(t.ctx.someProp); // > 123
    })
    .after(async t => {
         console.log(t.ctx.someProp); // > 123
    });
```

### Fixture Hooks
Fixture hooks are executed before the first test in a fixture is started and after the last test is finished. Unlike test hooks, fixture hooks are executed between test runs and do not have access to the tested page. Use them to perform server-side operations like preparing the server that hosts the tested app.
```
fixture.before( fn(ctx) )
fixture.after( fn(ctx) )
```
fn: An asynchronous hook function that contains initialization or clean-up code.

```
fixture `My fixture`
    .page `http://example.com`
    .before( async ctx => {
        /* fixture initialization code */
    })
    .after( async ctx => {
        /* fixture finalization code */
    });
```

### Sharing Variables Between Fixture Hooks and Test Code
Hook functions passed to fixture.before and fixture.after methods take a ctx parameter that contains fixture context. You can add properties to this parameter to share the value or object with test code.
```
fixture `Fixture1`
    .before(async ctx  => {
        ctx.someProp = 123;
    })
    .after(async ctx  => {
        console.log(ctx.someProp); // > 123
    });
```
To access fixture context from tests, use the `t.fixtureCtx` property.
```
t.fixtureCtx
```

Test code can read from t.fixtureCtx, assign to its properties or add new ones, but it cannot overwrite the entire t.fixtureCtx object.
```
fixture `Fixture1`
    .before(async ctx  => {
        ctx.someProp = 123;
    })
    .after(async ctx  => {
        console.log(ctx.newProp); // > abc
    });

test('Test1', async t => {
    console.log(t.fixtureCtx.someProp); // > 123
});

test('Test2', async t => {
    t.fixtureCtx.newProp = 'abc';
});
```

## Skipping Tests
TestCafe allows you to specify that a particular test or fixture should be skipped when running tests. Use the fixture.skip and test.skip methods for this.
```
fixture.skip
test.skip
```

You can also use the only method to specify that only a particular test or fixture should run while all others should be skipped.
```
fixture.only
test.only
```
If several tests or fixtures are marked with only, all the marked tests and fixtures are run.

```
fixture.skip `Fixture1`; // All tests in this fixture are skipped

test('Fixture1Test1', () => {});
test('Fixture1Test2', () => {});

fixture `Fixture2`;

test('Fixture2Test1', () => {});
test.skip('Fixture2Test2', () => {}); // This test is skipped
test('Fixture2Test3', () => {});
```

```
fixture.only `Fixture1`;
test('Fixture1Test1', () => {});
test('Fixture1Test2', () => {});

fixture `Fixture2`;

test('Fixture2Test1', () => {});
test.only('Fixture2Test2', () => {});
test('Fixture2Test3', () => {});

// Only tests in Fixture1 and the Fixture2Test2 test are run
```

## Inject Scripts into Tested Pages
TestCafe allows you to inject custom scripts into pages visited during the tests. You can add scripts that mock browser API or provide helper functions.
```
fixture.clientScripts( script[, script2[, ...[, scriptN]]] )
test.clientScripts( script[, script2[, ...[, scriptN]]] )
```

You can use the page option to specify pages into which scripts should be injected. Otherwise, TestCafe injects scripts into all pages visited during the test or fixture.
```
fixture `My fixture`
    .page `http://example.com`
    .clientScripts('assets/jquery.js');
test
    ('My test', async t => { /* ... */ })
    .clientScripts({ module: 'async' });
test
    ('My test', async t => { /* ... */ })
    .clientScripts({
        page: /\/user\/profile\//,
        content: 'Geolocation.prototype.getCurrentPosition = () => new Positon(0, 0);'
    });
```

## Disable Page Caching
You can disable page caching to keep items in these storages after navigation. Use the fixture.disablePageCaching and test.disablePageCaching methods to disable caching during a particular fixture or test.
```
fixture.disablePageCaching
test.disablePageCaching
```

```
fixture
    .disablePageCaching `My fixture`
    .page `https://example.com`;
test
    .disablePageCaching
    ('My test', async t => { /* ... */ });
```


# Selecting Page Elements
## Selectors
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

### Creating Selectors
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

### Using Selectors
#### Check if an Element Exists. 
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

#### Obtain Element State 
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

#### DOM Node Snapshot
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

#### Define Action Targets
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

#### Define Assertion Actual Value
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

#### Selector Timeout
When a selector is called in test code, TestCafe waits for the target node to appear in the DOM within the selector timeout. You can specify the selector timeout in test code by using the timeout option. To set this timeout when launching tests, pass it to the runner.run method if you use API or specify the selector-timeout option if you run TestCafe from the command line.

#### Debug Selectors 
TestCafe outputs information about failed selectors to test run reports.

When you try to use a selector that does not match any DOM element, the test fails and an error is thrown. The error message indicates which selector has failed.

An error can also occur when you call selector's methods in a chain. These methods are applied to the selector one by one. TestCafe detects a method after which the selector no longer matches any DOM element. This method is highlighted in the error message.

### Functional-Style Selectors
#### Filter DOM Nodes
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

#### Search for Elements in the DOM Hierarchy
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

#### Filtering DOM Elements by Predicates
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

#### Examples 
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

### Selector Options
#### options.boundTestRun
If you need to call a selector from a Node.js callback, assign the current test controller to the boundTestRun option.

#### options.dependencies
Use this option to pass functions, variables or objects to selectors initialized with a function. The dependencies object's properties are added to the function's scope as variables.

#### options.timeout
The amount of time, in milliseconds, allowed for an element returned by the selector to appear in the DOM before the test fails.

#### options.visibilityCheck
true to additionally require the returned element to become visible within options.timeout.

#### Overwrite Options
You can use the selector's `with` function to overwrite its options.

### Extending Selectors
TestCafe allows you to extend element state with custom properties and methods executed on the client side.

### Edge Cases and Limitations

## DOM Node State
### Members Common Across All Nodes
- childElementCount	Number	The number of child HTML elements.
- childNodeCount	Number	The number of child nodes.
- hasChildElements	Boolean	true if this node has child HTML elements.
- hasChildNodes	Boolean	true if this node has child nodes.
- nodeType	Number	The type of the node. See Node.nodeType.
- textContent	String	The text content of the node and its descendants. See Node.textContent.
- hasClass(className)	Boolean	true if the element has the specified class name.

### Members Specific to Element Nodes
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

## Framework-Specific Selectors
### React 
https://github.com/DevExpress/testcafe-react-selectors/blob/master/README.md

### Angular
https://github.com/DevExpress/testcafe-angular-selectors/blob/master/angular-selector.md

## Examples of Working with DOM Elements
### Access Page Element Properties
### Get a Page Element Using Custom Logic
### Access Child Nodes in the DOM Hierarchy
### Access Shadow DOM
### Check if an Element is Available
### Enumerate Elements Identified by a Selector
### Select Elements With Dynamic IDs


# Actions
## Click
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

## Double Click
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

## Right Click
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

## Drag Element
The `t.drag` or t.dragToElement actions will not invoke integrated browser actions such as selecting text. Use them to perform drag-and-drop actions that are processed by webpage elements, not the browser. To select text, use `t.selectText`.

### Drag an Element by an Offset 
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

### Drag an Element onto Another One 
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

## Hover
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

## Take Screenshot
### Take a Screenshot of the Entire Page 
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

### Take a Screenshot of a Page Element
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

## Navigate
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

## Press Key
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

### Browser Processing Emulation 
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

## Select Text
### Select Text in Input Elements
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

### Select <textarea> Content
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

### Select within Editable Content
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

## Type Text
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

### Typing Into DateTime, Color and Range Inputs 
- Input type	Pattern	Example
- Date	yyyy-mm-dd	'2017-12-23'
- Week	yyyy-Www	'2017-W03'
- Month	yyyy-mm	'2017-08'
- DateTime	yyyy-mm-ddThh:mm	'2017-11-03T05:00'
- Time	hh:mm	'15:30'
- Color	#rrggbb	'#003000'
- Range	n	'45'

## Upload
The t.setFilesToUpload and t.clearUpload actions only allow you to manage the list of files for upload. These files are uploaded to the server after you initiate upload, for example, when you click the Upload or Submit button on a webpage.

### Populate File Upload Input
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

### Clear File Upload Input
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

## Resize Window
### Setting the Window Size in pixels
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
### Fitting the Window into a Particular Device
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

### Maximizing the Window
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

## Action Options
### Basic Action Options
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

### Mouse Action Options
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

### DragToElement Action Options
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

### Click Action Options
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

### Typing Action Options
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


# Assertions


# Obtaining Data From the Client


# Intercepting HTTP Requests


# Built-In Waiting Mechanisms


# Authentication


# Pausing the Test


# Handling Native Dialogs


# Working with <iframes>


# Debugging


# Identify the Browser and Platform


# Accessing Console Messages