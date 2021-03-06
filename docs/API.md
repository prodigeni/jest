---
id: api
title: API Reference
layout: docs
category: Reference
permalink: docs/api.html
---

#### The `jest` object

  - [`jest.autoMockOff()`](#jest-automockoff)
  - [`jest.autoMockOn()`](#jest-automockon)
  - [`jest.clearAllTimers()`](#jest-clearalltimers)
  - [`jest.dontMock(module)`](#jest-dontmockmodulename)
  - [`jest.genMockFromModule(moduleObj)`](#jest-genmockfrommodule-moduleobj)
  - [`jest.genMockFunction()`](#jest-genmockfunction)
  - [`jest.genMockFn()`](#jest-genmockfn)
  - [`jest.mock(moduleName)`](#jest-mockmodule-name)
  - [`jest.runAllTicks()`](#jest-runallticks)
  - [`jest.runAllTimers()`](#jest-runalltimers)
  - [`jest.runOnlyPendingTimers()`](#jest-runonlypendingtimers)
  - [`jest.setMock(moduleName, moduleExports)`](#jest-setmock-modulename-moduleexports)

#### Mock functions

  - [`mockFn.mock.calls`](#mockfn-mock-calls)
  - [`mockFn.mock.instances`](#mockfn-mock-instances)
  - [`mockFn.mockClear()`](#mockfn-mockclear)
  - [`mockFn.mockImplementation(fn)`](#mockfn-mockimplementation-fn)
  - [`mockFn.mockImpl(fn)`](#mockfn-mockimpl-fn)
  - [`mockFn.mockReturnThis()`](#mockfn-mockreturnthis)
  - [`mockFn.mockReturnValue(value)`](#mockfn-mockreturnvalue-value)
  - [`mockFn.mockReturnValueOnce(value)`](#mockfn-mockreturnvalueonce-value)

#### Config options

  - [`config.collectCoverage` [boolean]](#config-collectcoverage-boolean)
  - [`config.collectCoverageOnlyFrom` [object]](#config-collectcoverageonlyfrom-object)
  - [`config.modulePathIgnorePatterns` [array<string>]](#config-modulepathignorepatterns-array-string)
  - [`config.rootDir` [string]](#config-rootdir-string)
  - [`config.scriptPreprocessor` [string]](#config-scriptpreprocessor-string)
  - [`config.setupEnvScriptFile` [string]](#config-setupenvscriptfile-string)
  - [`config.setupTestFrameworkScriptFile` [string]](#config-setuptestframeworkscriptfile-string)
  - [`config.testDirectoryName` [string](#config-testdirectoryname-string)
  - [`config.testFileExtensions` [array<string>]](#config-testfileextensions-array-string)
  - [`config.testPathDirs` [array<string>]](#config-testpathdirs-array-string)
  - [`config.testPathIgnorePatterns` [array<string>]](#config-testpathignorepatterns-array-string)
  - [`config.unmockedModulePathPatterns` [array<string>]](#config-unmockedmodulepathpatterns-array-string)

#### Globally injected variables

  - `afterEach(fn)`
  - `beforeEach(fn)`
  - `describe(name, fn)`
  - `it(name, fn)`
  - `it.only(name, fn)` executes [only](https://github.com/davemo/jasmine-only) this test. Useful when investigating a failure
  - [`jest`](#the-jest-object)
  - `pit(name, fn)` [helper](https://www.npmjs.org/package/jasmine-pit) for promises
  - `require(module)`
  - `require.requireActual(module)`
  - `xdescribe(name, fn)`
  - `xit(name, fn)`

#### `expect(value)`

  - `.not` inverse the next comparison
  - `.toThrow(?message)`
  - `.toBe(value)` comparison using `===`
  - `.toEqual(value)` deep comparison. Use [`jasmine.any(type)`](http://jasmine.github.io/1.3/introduction.html#section-Matching_Anything_with_<code>jasmine.any</code>) to be softer
  - `.toBeFalsy()`
  - `.toBeTruthy()`
  - `.toBeNull()`
  - `.toBeUndefined()`
  - `.toBeDefined()`
  - `.toMatch(regexp)`
  - `.toContain(string)`
  - `.toBeCloseTo(number, delta)`
  - `.toBeGreaterThan(number)`
  - `.toBeLessThan(number)`
  - `.toBeCalled()`
  - `.toBeCalledWith(arg, um, ents)`
  - `.lastCalledWith(arg, um, ents)`

-----

### `jest.autoMockOff()`
Disables automatic mocking in the module loader.

After this method is called, all `require()`s will return the real versions of each module (rather than a mocked version)

This is usually useful when you have a scenario where the number of dependencies you want to mock is far less than the number of dependencies that you don't. For example, if you're writing a test for a module that uses a large number of dependencies that can be reasonably classified as "implementation details" of the module, then you likely do not want to mock them.

Examples of dependencies that might be considered "implementation details" are things ranging from language built-ins (e.g. Array.prototype methods) to highly common utility methods (e.g. underscore/lo-dash, array utilities, class-builder libraries, etc)

### `jest.autoMockOn()`
Re-enables automatic mocking in the module loader.

It's worth noting that automatic mocking is on by default, so this method is only useful if that default has been changes (such as by previously calling [`jest.autoMockOff()`](#jest-automockoff))

### `jest.clearAllTimers()`
Removes any pending timers from the timer system.

This means, if any timers have been scheduled (but have not yet executed), they will be cleared and will never have the opportunity to execute in the future.

### `jest.dontMock(moduleName)`
Indicates that the module system should never return a mocked version of the specified module from `require()` (e.g. that it should always return the real module).

The most common use of this API is for specifying the module a given test intends to be testing (and thus doesn't want automatically mocked).

### `jest.genMockFromModule(moduleObj)`
Given a module exports object, use the automatic mocking system to generate a mocked version of the object for you.

This is useful when you have an object that the module system does not know about, and you want to automatically generate a mock for it.

### `jest.genMockFunction()`
Returns a freshly generated, unused [mock function](#mock-functions).

### `jest.genMockFn()`
Shorthand alias for [`jest.genMockFunction`](#jest-genmockfunction).

### `jest.mock(moduleName)`
Indicates that the module system should always return a mocked version of the specified module from `require()` (e.g. that it should never return the real module).

This is normally useful under the circumstances where you have called [`jest.autoMockOff()`](#jest-automockoff), but still wish to specify that certain particular modules should be mocked by the module system.

### `jest.runAllTicks()`
Exhausts the micro-task queue (usually interfaced in node via `process.nextTick`).

When this API is called, all pending micro-tasks that have been queued via `process.nextTick` will be executed. Additionally, if those micro-tasks themselves schedule new micro-tasks, those will be continually exhausted until there are no more micro-tasks remaining in the queue.

This is often useful for synchronously executing all pending promises in the system.

### `jest.runAllTimers()`
Exhausts the macro-task queue (i.e., all tasks queued by `setTimeout()` and `setInterval()`).

When this API is called, all pending "macro-tasks" that have been queued via `setTimeout()` or `setInterval()` will be executed. Additionally if those macro-tasks themselves schedule new macro-tasks, those will be continually exuasted until there are no more macro-tasks remaining in the queue.

This is often useful for synchronously executing setTimeouts during a test in order to synchronously assert about some behavior that would only happen after the `setTimeout()` or `setInterval()` callbacks executed. See the [Timer mocks](/jest/docs/timer-mocks.html) doc for more information.

### `jest.runOnlyPendingTimers()`
Executes only the macro-tasks that are currently pending (i.e., only the tasks that have been queued by `setTimeout()` or `setInterval()` up to this point). If any of the currently pending macro-tasks schedule new macro-tasks, those new tasks will not be executed by this call.

This is useful for scenarios such as one where the module being tested schedules a `setTimeout()` whose callback scheduls another `setTimeout()` recursively (meaning the scheduling never stops). In these scenarios, it's useful to be able to run forward in time by a single step at a time.

### `jest.setMock(moduleName, moduleExports)`
Explicitly supplies the mock object that the module system should return for the specified module.

On occaison there are times where the automatically generated mock the module system would normally provide you isn't adequate enough for your testing needs. Normally under those circumstances you should write a [manual mock](/jest/docs/manual-mocks.html) that is more adequate for the module in question. However, on extremely rare occasions, even a manual mock isn't suitable for your purposes and you need to build the mock yourself inside your test.

In these rare scenarios you can use this API to manually fill the slot in the module system's mock-module registry.

### `mockFn.mock.calls`
An array that represents all calls that have been made into this mock function. Each call is represented by an array of arguments that were passed during the call.

For example: A mock function `f` that has been called twice, with the arguments `f('arg1', 'arg2')`, and then with the arguments `f('arg3', 'arg4')` would have a `mock.calls` array that looks like this:

```javascript
[
  ['arg1', 'arg2'],
  ['arg3', 'arg4']
]
```

### `mockFn.mock.instances`
An array that contains all the object instances that have been instantiated from this mock function.

For example: A mock function that has been instantiated twice would have the following `mock.instances` array:

```javascript
var mockFn = jest.genMockFunction();

var a = new mockFn();
var b = new mockFn();

mockFn.mock.instances[0] === a; // true
mockFn.mock.instances[1] === b; // true
```

### `mockFn.mockClear()`
Resets all information stored in the [`mockFn.mock.calls`](#mockfn-mock-calls) and [`mockFn.mock.instances`](#mockfn-mock-instances) arrays.

Often this is useful when you want to clean up a mock's usage data between two assertions.

### `mockFn.mockImplementation(fn)`
Accepts a function that should be used as the implementation of the mock. The mock itself will still record all calls that go into and instances that come from itself – the only difference is that the implementation will also be executed when the mock is called.

For example:

```javascript
var mockFn = jest.genMockFunction().mockImplementation(function(scalar) {
  return 42 + scalar;
});

var a = mockFn(0);
var b = mockFn(1);

a === 42; // true
b === 43; // true

mockFn.mock.calls[0][0] === 0; // true
mockFn.mock.calls[1][0] === 1; // true
```

### `mockFn.mockImpl(fn)`
Shorthand alias for [`mockFn.mockImplementation(fn)`](#mockfn-mockimplementation-fn).

### `mockFn.mockReturnThis()`
Just a simple sugar function for:

```javascript
.mockImplementation(function() {
  return this;
});
```

### `mockFn.mockReturnValue(value)`
Just a simple sugar function for:

```javascript
.mockImplementation(function() {
  return value;
});
```

### `mockFn.mockReturnValueOnce(value)`
Just a simple sugar function for:

```javascript
var valueReturned = false;
.mockImplementation(function() {
  if (!valueReturned) {
    valueReturned = true;
    return value;
  }
});
```

### `config.collectCoverage` [boolean]
(default: `false`)

Indicates whether the coverage information should be collected while executing the test. Because this retrofits all executed files with coverage collection statements, it may significantly slow down your tests.

### `config.collectCoverageOnlyFrom` [object]
(default: `undefined`)

An object that, when present, indicates a set of files for which coverage information should be collected. Any files not present in this set will not have coverage collected for them. Since there is a performance cost for each file that we collect coverage information from, this can help prune this cost down to only the files in which you care about coverage (such as the specific modules that you are testing).

### `config.modulePathIgnorePatterns` [array<string>]
(default: `["/node_modules/"]`)

An array of regexp pattern strings that are matched against all module paths before those paths are to be considered 'visible' to the module loader. If a given module's path matches any of the patterns, it will not be `require()`-able in the test environment.

### `config.rootDir` [string]
(default: The `pwd` the CLI is being executed from)

The root directory that Jest should scan for tests and modules within. If you put your Jest config inside your `package.json` and want the root directory to be the root of your repo, the value for this config param will default to the directory of the `package.json`.

Oftentimes, you'll want to set this to `'src'` or `'lib'`, corresponding to where in your repository the code is stored.

### `config.scriptPreprocessor` [string]
(default: `undefined`)

The path to a module that provides a synchronous function from pre-processing source files. For example, if you wanted to be able to use a new language feature in your modules or tests that isn't yet supported by node (like, for example, ES6 classes), you might plug in one of many transpilers that compile ES6 to ES5 here.

Examples of such compilers include [jstransform](http://github.com/facebook/jstransform), [recast](http://github.com/facebook/recast), [regenerator](http://github.com/facebook/regenerator), and [traceur](https://github.com/google/traceur-compiler).

### `config.setupEnvScriptFile` [string]
(default: `undefined`)

The path to a module that runs some code to configure or set up the testing environment before each test. Since every test runs in it's own environment, this script will be executed in the testing environment immediately before executing the test code itself.

It's worth noting that this code will execute *before* [`config.setupTestFrameworkScriptFile`](#config-setuptestframeworkscriptfile-string).

### `config.setupTestFrameworkScriptFile` [string]
(default: `undefined`)

The path to a module that runs some code to configure or set up the testing framework before each test. Since [`config.setupEnvScriptFile`](#config-setupenvscriptfile-string) executes before the test framework is installed in the environment, this script file presents you the opportunity of running some code immediately after the test framework has been installed in the environment.

For example, Jest ships with several plug-ins to `jasmine` that work by monkey-patching the jasmine API. If you wanted to add even more jasmine plugins to the mix (or if you wanted some custom, project-wide matchers for example), you could do so in this module.

### `config.testDirectoryName` [string]
(default: `'__tests__'`)

The name of directories that Jest should expect to find tests in.

For example, many node projects prefer to put their tests in a `tests` directory.

### `config.testFileExtensions` [array<string>]
(default: `['js']`)

An array of file extensions that test files might have. Jest uses this when searching for tests to run.

This is useful if, for example, you are writting test files using CoffeeScript with a `.coffee` file extension. In such a scenario, you can use `['js', 'coffee']` to make Jest find files that end in both `.js` and `.coffee`. (Don't for get to set up a coffeescript pre-processor using [`config.scriptPreprocessor`](#config-scriptpreprocessor-string) too!)

### `config.testPathDirs` [array<string>]
(default: The `pwd` the cli is being executed from)

A list of paths to directories that Jest should use to search for tests in.

There are times where you only want Jest to search in a single sub-directory (such as cases where you have a `src/` directory in your repo), but not the rest of the repo.

### `config.testPathIgnorePatterns` [array<string>]
(default: `["/node_modules/"]`)

An array of regexp pattern strings that are matched against all test paths before executing the test. If the test path matches any of the patterns, it will be skipped.

### `config.unmockedModulePathPatterns` [array<string>]
(default: `[]`)

An array of regexp pattern strings that are matched against all modules before the module loader will automatically return a mock for them. If a module's path matches any of the patterns in this list, it will not be automatically mocked by the module loader.

This is useful for some commonly used 'utility' modules that are almost always used as implementation details almost all the time (like underscore/lo-dash, etc). It's generally a best practice to keep this list as small as possible and always use explicit `jest.mock()`/`jest.dontMock()` calls in individual tests. Explicit per-test setup is far easier for other readers of the test to reason about the environment the test will run in.

It is possible to override this setting in individual tests by explicitly calling `jest.mock()` at the top of the test file.
