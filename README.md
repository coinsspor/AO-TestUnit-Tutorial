# Test-Driven Development (TDD) with Test Unit Guide

## 1. Introduction

### What is Test-Driven Development (TDD)?
Test-Driven Development (TDD) is a software development methodology that requires writing tests before writing the actual code. The main goal of TDD is to improve software quality and identify errors early in the development process. The TDD cycle consists of three main steps:

1. **Red**: First, a test that does not pass is written. This step determines the requirements for the code to be written.
2. **Green**: Minimal code is written to pass the written test. This step ensures that the code works correctly.
3. **Refactor**: The structure of the code is improved without changing its functionality. This step makes the code cleaner and more maintainable.

### Advantages of TDD
- **Fewer Bugs**: Detects bugs at early stages, resulting in a development process with fewer bugs.
- **Better Design**: Improves the structure and design of the code.
- **Higher Code Quality**: Encourages writing clean and maintainable code.
- **More Reliable Software**: Continuously verifies that the software works correctly.

## 2. Introduction to the Test Unit Module

### Module Features
The Test Unit Module provides a simple framework for creating and running unit tests in Lua. This module allows you to create test suites, add test cases, and run the tests. Additionally, it provides detailed results for each test case and a summary of passed/failed tests.

### Installation and Basic Usage
Copy the `Test.lua` file to your project directory.

#### Creating a Test Suite
```lua
local Test = require("Test")
local myTests = Test.new("My Test Suite")
```
#### Adding Tests
```lua
myTests:add("Test Case 1", function()
    assert(1 + 1 == 2, "Math is broken!")
end)

myTests:add("Test Case 2", function()
    assert(type("hello") == "string", "Type check failed!")
end)

```
#### Running Tests
```lua
local results = myTests:run()
print(results)
```
#### Example

```lua
local Test = require("Test")

-- Create a new test suite
local myTests = Test.new("Example Test Suite")

-- Add test cases
myTests:add("Test Addition", function()
    assert(1 + 1 == 2, "Addition failed!")
end)

myTests:add("Test Type", function()
    assert(type("hello") == "string", "Type check failed!")
end)

-- Run the tests and print the results
local results = myTests:run()
print(results)

```
## 3. Step-by-Step TDD Implementation

#### Step 1: Writing Test Cases
First, we write test cases according to the requirements of our project. These tests determine how the functions in our project should work.

#### Step 2: Writing Code and Passing Tests
After writing the test cases, we write minimal code to pass these tests. The goal is to ensure that all the tests we wrote pass successfully.

#### Step 3: Refactoring
After ensuring that all tests pass, we can improve the structure of the code. In this step, we make the code cleaner and more maintainable without changing its functionality.

## 4. Example Project: AI Application
Project Description
This AI application consists of a simple function set that will calculate the average of a given list of numbers.

#### Writing Tests
First, let's write test cases for the AI functions:

```lua
local Test = require("Test")
local aiTests = Test.new("AI Test Suite")

aiTests:add("Test Average Calculation", function()
    assert(ai.calculateAverage({1, 2, 3, 4, 5}) == 3, "Average calculation failed!")
end)

aiTests:add("Test Empty List", function()
    assert(ai.calculateAverage({}) == 0, "Empty list average calculation failed!")
end)

```
#### Writing Code
Let's write our code to pass the tests:

```lua
local ai = {}

function ai.calculateAverage(numbers)
    if #numbers == 0 then
        return 0
    end

    local sum = 0
    for _, num in ipairs(numbers) do
        sum = sum + num
    end

    return sum / #numbers
end

return ai


```

#### Running Tests and Evaluating Results
Let's run our tests and evaluate the results:


```lua
local results = aiTests:run()
print(results)

```
By following these steps, we have implemented the TDD approach. As we developed our project, we wrote our tests at each step and developed our code according to these tests.

## 5. Conclusion and Recommendations
#### Applying TDD in Projects
By applying the TDD approach in your projects, you can develop software with fewer bugs and higher quality. TDD provides a disciplined and systematic approach to the software development process.

#### Best Practices
Write small and manageable test cases.
Continuously run tests at each step to constantly verify the correctness of the code.
Continuously refactor and improve your code.
This guide has shown how to write and apply tests using the Test Unit module with the Test-Driven Development (TDD) approach. By following step-by-step, you can effectively implement the TDD approach in your projects.

#### Example Project Structure
Let's divide our project structure into three main folders: db-admin, manager, and test-unit. Each folder will contain specific tasks and tests.

#### db-admin Folder
The db-admin folder contains modules that manage database operations. In this folder, there are codes that perform operations on the SQLite database and test them.

#### Example Code
db-admin/src/main.lua

```lua
local sqlite3 = require("lsqlite3")

local dbAdmin = {}

function dbAdmin.open(dbfile)
    local db = sqlite3.open(dbfile)
    return db
end

function dbAdmin.createTable(db, tableName)
    local sql = string.format("CREATE TABLE IF NOT EXISTS %s (id INTEGER PRIMARY KEY, content TEXT)", tableName)
    db:exec(sql)
end

function dbAdmin.insertRecord(db, tableName, content)
    local stmt = db:prepare(string.format("INSERT INTO %s (content) VALUES (?)", tableName))
    stmt:bind_values(content)
    stmt:step()
    stmt:finalize()
end

function dbAdmin.getRecordCount(db, tableName)
    local stmt = db:prepare(string.format("SELECT COUNT(*) FROM %s", tableName))
    stmt:step()
    local count = stmt:get_uvalues()
    stmt:finalize()
    return count
end

return dbAdmin


```

#### Test Codes
db-admin/test/main.test.js

```javascript
import { test } from 'node:test'
import * as assert from 'node:assert'
import { Send } from './aos.helper.js'
import fs from 'node:fs'

test('dbAdmin test suite', async () => {
    const mainLua = fs.readFileSync('./src/main.lua', 'utf-8')
    const exampleLua = fs.readFileSync('./src/example.lua', 'utf-8')

    // Load dbAdmin
    const loadResult = await Send({
        Action: "Eval",
        Data: `local function _load() ${mainLua} end _G.package.loaded["DbAdmin"] = _load() return "loaded.."`
    })
    assert.equal(loadResult.Output.data.output, 'loaded..')

    // Run example
    const exampleResult = await Send({ Action: "Eval", Data: exampleLua })
    assert.equal(exampleResult.Output.data.output, '{ \x1B[32m"test"\x1B[0m }')

    // Check record count
    const countResult = await Send({ Action: "Eval", Data: `dbAdmin:count('test')` })
    assert.equal(countResult.Output.data.output, '3')

    // Check records
    const recordsResult = await Send({ Action: "Eval", Data: `require('json').encode(dbAdmin:exec('SELECT * FROM test'))` })
    assert.equal(recordsResult.Output.data.output, '[{"content":"Hello Lua","id":1},{"content":"Hello Sqlite3","id":2},{"content":"Hello ao!!!","id":3}]')
})

```
#### manager Folder
The manager folder contains client operations and management functions. In this folder, there are codes that manage operations between the client and the server.

#### Example Code
manager/src/client.lua

```lua
local client = {}

function client.connect(server)
    print("Connecting to " .. server)
    -- Connection codes here
end

function client.sendMessage(message)
    print("Sending message: " .. message)
    -- Message sending codes here
end

return client


```

#### Test Codes
manager/test/main.test.js


```javascript
import { test } from 'node:test'
import * as assert from 'node:assert'
import { Send } from './aos.helper.js'
import fs from 'node:fs'

test('client test suite', async () => {
    const clientLua = fs.readFileSync('./src/client.lua', 'utf-8')

    // Load client
    const loadResult = await Send({
        Action: "Eval",
        Data: `local function _load() ${clientLua} end _G.package.loaded["Client"] = _load() return "loaded.."`
    })
    assert.equal(loadResult.Output.data.output, 'loaded..')

    // Test connect
    const connectResult = await Send({ Action: "Eval", Data: `client.connect('localhost')` })
    assert.ok(connectResult.Output.data.output.includes('Connecting to localhost'))

    // Test send message
    const messageResult = await Send({ Action: "Eval", Data: `client.sendMessage('Hello, Server!')` })
    assert.ok(messageResult.Output.data.output.includes('Sending message: Hello, Server!'))
})


```
#### test-unit Folder
The test-unit folder contains test modules and unit tests. In this folder, there are various test modules and test scenarios.

#### Example Code
test-unit/src/test.lua

```lua
local Test = require("Test")

local myTests = Test.new("Example Test Suite")

myTests:add("Test Addition", function()
    assert(1 + 1 == 2, "Addition failed!")
end)

myTests:add("Test Type", function()
    assert(type("hello") == "string", "Type check failed!")
end)

return myTests

```

#### Test Codes
test-unit/test/test.test.js

```javascript
import { test } from 'node:test'
import * as assert from 'node:assert'
import { Send } from './aos.helper.js'
import fs from 'node:fs'

test('test unit suite', async () => {
    const testLua = fs.readFileSync('./src/test.lua', 'utf-8')

    // Load Test Unit
    const loadResult = await Send({
        Action: "Eval",
        Data: `local function _load() ${testLua} end _G.package.loaded["Test"] = _load() return "loaded.."`
    })
    assert.equal(loadResult.Output.data.output, 'loaded..')

    // Run tests
    const runResult = await Send({ Action: "Eval", Data: `return myTests:run()` })
    assert.ok(runResult.Output.data.output.includes('Passed: 2, Failed: 0'))
})


```



