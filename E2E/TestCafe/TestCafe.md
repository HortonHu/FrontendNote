# Installation
## Global Installation
```
npm install -g testcafe
```

## Local Installation
```
npm install --save-dev testcafe
```

npm scripts - add the testcafe command to the scripts section in package.json:
```
"scripts": {
    "test": "testcafe chrome tests/"
}
```
Then use npm test to run the specified TestCafe command:
```
npm test
```

## Permission
It will require screen recording permission on macOS(V10.15+)


# Command Line Interface
When you execute the testcafe command, TestCafe first reads settings from the `.testcaferc.json` configuration file if this file exists, and then applies the settings from the command line. Command line settings override values from the configuration file in case they differ. TestCafe prints information about every overridden property in the console.
## Browser List 
### Local Browsers
You can specify locally installed browsers using browser aliases or paths (with the path: prefix). 
```
testcafe chrome,path:/applications/safari.app tests/sample-fixture.js
```
Use the all alias to run tests in all the installed browsers.
```
testcafe all tests/sample-fixture.js
```

### Portable Browsers
You can specify portable browsers using paths to the browser's executable file (with the path: prefix)
```
testcafe path:d:\firefoxportable\firefoxportable.exe tests/sample-fixture.js
```

If the path contains spaces, enclose it in backticks and additionally surround the whole parameter including the keyword in quotation marks.

In Windows cmd.exe (default command prompt), use double quotation marks:
```
testcafe "path:`C:\Program Files (x86)\Firefox Portable\firefox.exe`" tests/sample-fixture.js
```

In Unix shells like bash, zsh, csh (macOS, Linux, Windows Subsystem for Linux) and Windows PowerShell, use single quotation marks:
```
testcafe 'path:`C:\Program Files (x86)\Firefox Portable\firefox.exe`' tests/sample-fixture.js
```

### Headless Mode
To run tests in the headless mode in Google Chrome or Firefox, use the `:headless`postfix:
```
testcafe "firefox:headless" tests/sample-fixture.js
```

### Chrome Device Emulation
To run tests in Chrome's device emulation mode, specify `:emulation` and device parameters.
```
testcafe "chrome:emulation:device=iphone X" tests/sample-fixture.js
```

### Remote Browsers
To run tests in a browser on a remote device, specify `remote` as a browser alias.
```
testcafe remote tests/sample-fixture.js
```
If you want to connect multiple browsers, specify `remote:` and the number of browsers. For example, if you need to use three remote browsers, specify `remote:3`.
```
testcafe remote:3 tests/sample-fixture.js
```

### Browser with arguments
If you need to pass arguments for the specified browser, write them after the browser's alias. Enclose the browser call and its arguments in quotation marks.

In Windows cmd.exe (default command prompt), use double quotation marks:
```
testcafe "chrome --start-fullscreen" tests/sample-fixture.js
```
In Unix shells like bash, zsh, csh (macOS, Linux, Windows Subsystem for Linux) and Windows PowerShell, use single quotation marks:
```
testcafe 'chrome --start-fullscreen' tests/sample-fixture.js
```
You can also specify arguments for portable browsers. If a path to a browser contains spaces, the path should be enclosed in backticks.

For Unix shells and Windows PowerShell:
```
testcafe 'path:`/Users/TestCafe/Apps/Google Chrome.app` --start-fullscreen' tests/sample-fixture.js
```
For cmd.exe:
```
testcafe "path:`C:\Program Files (x86)\Google\Chrome\Application\chrome.exe` --start-fullscreen" tests/sample-fixture.js
```
Only installed and portable browsers located on the current machine can be launched with arguments.

## File Path/Glob Pattern
The file-or-glob ... argument specifies the files or directories (separated by a space) from which to run the tests.

For example, this command runs all tests in the my-tests directory:
```
testcafe ie my-tests
```
The following command runs tests from the specified fixture files:
```
testcafe ie js-tests/fixture.js studio-tests/fixture.testcafe
```
You can also use glob patterns to specify a set of files.

The following command runs tests from files that match the `tests/*page*` pattern (for instance, `tests/example-page.js`, `tests/main-page.js`, or `tests/auth-page.testcafe`):
```
testcafe ie tests/*page*
```
If you do not specify any file or directory, TestCafe runs tests from the test or tests directories.

## Options 
### -h, --help
### -v, --version
### -b, --list-browsers
Lists the aliases of the auto-detected browsers installed on the local machine.
### -r `<name[:output],[...]>`, --reporter `<name[:output],[...]>`
Specifies the name of a built-in or custom reporter that is used to generate test reports.

The following command runs tests in all available browsers and generates a report in `xunit` format:
```
testcafe all tests/sample-fixture.js -r xunit
```
The following command runs tests and generates a test report by using the custom reporter plugin:
```
testcafe all tests/sample-fixture.js -r my-reporter
```
The generated test report is displayed in the command prompt window.

If you need to save the report to an external file, specify the file path after the report name.
```
testcafe all tests/sample-fixture.js -r json:report.json
```
You can also use multiple reporters in a single test run. List them separated by commas.
```
testcafe all tests/sample-fixture.js -r spec,xunit:report.xml
```
Note that only one reporter can write to stdout. All other reporters must output to files.

### -s, --screenshots `<option=value[,option2=value2,...]>`
#### path 
Specifies the base directory where screenshots are saved.
#### takeOnFails
`takeOnFails=true` Takes a screenshot whenever a test fails.
#### pathPattern
Specifies a custom pattern to compose screenshot files' relative path and name.
#### fullPage
Specifies that the full page should be captured, including content that is not visible due to overflow.
### --disable-screenshots
### --video `<basePath>`
Enables TestCafe to record videos of test runs and specifies the base directory to save these videos.
### --video-options `<option=value[,option2=value2,...]>`
### --video-encoding-options `<option=value[,option2=value2,...]>`
### -q, --quarantine-mode
Enables the quarantine mode for tests that fail.
### -d, --debug-mode
### --debug-on-fail
### -e, --skip-js-errors
### -u, --skip-uncaught-errors
### -t `<name>`, --test `<name>`
TestCafe runs a test with the specified name.
### -T `<pattern>`, --test-grep `<pattern>`
TestCafe runs tests whose names match the specified grep pattern.
### -f `<name>`, --fixture `<name>`
TestCafe runs a fixture with the specified name.
### -F `<pattern>`, --fixture-grep `<pattern>`
TestCafe runs fixtures whose names match the specified grep pattern.
### --test-meta `<key=value[,key2=value2,...]>`
TestCafe runs tests whose metadata matches the specified key-value pair.
### --fixture-meta `<key=value[,key2=value2,...]>`
TestCafe runs tests whose fixture's metadata matches the specified key-value pair.
### -a `<command>`, --app `<command>`
Executes the specified shell command before running tests. Use it to set up the application you are going to test. 

An application is automatically terminated after testing is finished.
### --app-init-delay `<ms>`
Specifies the time (in milliseconds) allowed for an application launched using the --app option to initialize.

TestCafe waits the specified time before it starts running tests.

Default value: 1000
### -c `<n>`, --concurrency `<n>`
TestCafe opens n instances of the same browser and creates a pool of browser instances. Tests are run concurrently against this pool, that is, each test is run in the first free instance.
### --selector-timeout `<ms>`
Specifies the time (in milliseconds) within which selectors attempt to obtain a node to be returned. See Selector Timeout.

Default value: 10000
### --assertion-timeout `<ms>`
### --page-load-timeout `<ms>`
Specifies the time (in milliseconds) passed after the DOMContentLoaded event, within which TestCafe waits for the window.load event to fire.

After the timeout passes or the window.load event is raised (whichever happens first), TestCafe starts the test.

Default value: 3000

You can set the page load timeout to 0 to skip waiting for the window.load event.
### --speed `<factor>`
Specifies the test execution speed.

Tests are run at the maximum speed by default. You can use this option to slow the test down.

factor should be a number between 1 (the fastest) and 0.01 (the slowest).
### --cs `<path[,path2,...]>`, --client-scripts `<path[,path2,...]>`
Injects scripts from the specified files into each page visited during the tests. Use this option to introduce client-side mock functions or helper scripts.
### --ports `<port1,port2>`
### --hostname `<name>`
### --proxy `<host>`
### --proxy-bypass `<rules>`
### --ssl `<options>`
### -L, --live
### --dev
### --qr-code
### --sf, --stop-on-first-fail
### --ts-config-path `<path>`
### --disable-page-caching
### --color
### --no-color


# Programming Interface
This section describes the API that allows you to run TestCafe tests from your Node.js modules.
```javascript
const createTestCafe = require('testcafe');
let testcafe         = null;

createTestCafe('localhost', 1337, 1338)
    .then(tc => {
        testcafe     = tc;
        const runner = testcafe.createRunner();

        return runner
            .src(['tests/fixture1.js', 'tests/func/fixture3.js'])
            .browsers(['chrome', 'safari'])
            .run();
    })
    .then(failedCount => {
        console.log('Tests failed: ' + failedCount);
        testcafe.close();
    });
```

## TestCafe Class
## BrowserConnection Class
## Runner Class
## LiveModeRunner Class
## createTestCafe Factory


# Configuration File
TestCafe uses the `.testcaferc.json` configuration file to store its settings. Keep `.testcaferc.json` in the directory from which you run TestCafe. This is usually the project's root directory. 

## browsers
## src
## reporter
## screenshots
### path
### takeOnFails
### pathPattern
### fullPage
## disableScreenshots
## videoPath
## videoOptions
## videoEncodingOptions
## quarantineMode
## debugMode
## debugOnFail
## skipJsErrors
## skipUncaughtErrors
## filter
### test
### testGrep
### fixture
### fixtureGrep
### testMeta
### fixtureMeta
## appCommand
## appInitDelay
## concurrency
## selectorTimeout
## assertionTimeout
## pageLoadTimeout
## speed
## clientScripts
## port1, port2
## hostname
## proxy
## proxyBypass
## ssl
## developmentMode
## qrCode
## stopOnFirstFail
## tsConfigPath
## disablePageCaching
## color
## noColor


# Using TestCafe Docker Image
Todo


# Common Concepts
## Browsers
## Concurrent Test Execution
## Live Mode
## Screenshots and Videos
## Inject Scripts into Tested Pages
## Reporters
## Test HTTPS and HTTP/2 Websites
