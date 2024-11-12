# Selenium Python
* Selenium Python bindings provides a simple API to write functional/acceptance tests using Selenium WebDriver. Through Selenium Python API you can access all functionalities of Selenium WebDriver in an intuitive way.
* Selenium Python bindings provide a convenient API to access Selenium WebDrivers like Firefox, IE, Chrome, Remote etc. The current supported Python versions are 3.5 and above.

# Pytest and its advantages
* Pytest is a mature full-featured Python testing tool that helps you write better programs.
* Make us easy to write small, readable tests, and can scale to support complex functional testing for applications and libraries.
* Features
  * Detailed info on failing assert statements
  * Auto-discovery of test modules and functions
  * Modular fixtures for managing small or parametrized long-lived test resources
  * Rich plugin architecture, with over 800+ external plugins and thriving community

# Installation
* Install Python (3.5 and above), then check version
  * Check Python version: `python –version`
  * Python installation path: `C:\Users\{username}\AppData\Local\Programs\Python\Pythonxx`
* Install pip (standard package manager for Python to install additional packages and come when install Python by default), then check version
  * Check pip version: `pip –version`
  * Install if not exist: `py get-pip.py`
* Install Editor
  * Visual Studio Code with adding extensions
    * Python
    * Pylance
  * OR Pycharm Community

# Project Setup
* Open the folder that contains the automation project. Refer Getting Started with Python in VS Code (https://code.visualstudio.com/docs/python/python-tutorial)
* Create a virtual environment (to isolated installed packages from other environments for different projects
  * opening the Command Palette (Ctrl+Shift+P)
  * typing and selecting the Python: Create Environment command
![image](https://github.com/user-attachments/assets/15c2bf84-6685-4184-987c-e7a2961df9a5)

* Select Python Interpreter to run Python code by 
  * opening the Command Palette (Ctrl+Shift+P)
  * typing and selecting the Python: Select Interpreter command 
![image](https://github.com/user-attachments/assets/ceb7a451-9bc2-4120-912b-56cd87cf6765)

* Installation (additional automation packages)
  * Run command line in VS code: `View > Terminal`
    
![image](https://github.com/user-attachments/assets/1117ee40-0153-42bc-8dc0-d700f643c346)

  * Selenium (automate web browser interaction from Python)
    * Instructions: selenium · PyPI (https://pypi.org/project/selenium/)
    * Installation: `pip install selenium`
  * WebdriverManager (automatically download the webdriver binary for different browsers)
    * Instructions: webdriver-manager · PyPI (https://pypi.org/project/webdriver-manager/)
    * Installation: `pip install webdriver-manager`
  * Pytest (automation framework makes to write small, readable tests to complex functional testing)
    * Instructions: How-to guides — pytest documentation (https://docs.pytest.org/en/stable/how-to/index.html)
    * Installation: `pip install pytest`
  * Supported packages:
    * HTML report: `pip install pytest-html `
    * Data Reader: `pip install openpyxl`
  * Show package installed: `pip show <packageName>`

# How to run tests
* Using Test Explorer
![image](https://github.com/user-attachments/assets/95fc6531-2fdc-4001-a223-2debaa08177a)

* Using Command line: pytest
  * Run all tests: `pytest`
  * Run tests in a module: `pytest <filename>.py`
  * Run tests in a directory: `pytest <foldername>/`
  * Run tests in a group (using mark/tag in pytest): `pytest -m <grouping>`
  * Additional options:
    * -v : show more test information, e.g: directory, test status of each module..
    * -s : show console print
    * --html=report.html : generate HTML report after execution
    * Custom options

# Pytest Fixtures
## Fixtures (hooks):
* Definition
  * Executed before or after test execution e.g: setup, teardown, getdata..
  * Create fixture function with the `@pytest.fixture()` marker
  * The name of the fixture function can later be referenced to test modules or classes 
  * Fixtures can use `yield` the code block that is executed as teardown code regardless of the test outcome
* Scope of fixture
  * Scope by test method: run before/after specific test method executed only
    * Use fixture names as input arguments to test methods
```
def test_<testname>(<fixturename>)
```
* 
  * Scope by class: run before/after all class methods executed
    * Automatically apply for each method of class, no need to reference to fixture name to every class method
    * Using:
```
@pytest.fixture(scope="class") // before the fixture function
@pytest.mark.usefixtures(“<fixturename>") // before class
```

## conftest.py
  * Declare fixed fixtures to generalize and avoid duplication
  * These fixtures will be available to use to all test files in same folder
  * `request.cls.driver = driver`: to store driver to class variable to use across the requesting test context, because we cannot return before/after `yield` block

  ![image](https://github.com/user-attachments/assets/9d3097e7-2292-4622-9239-3f265af1ba28)

## Data driven Fixtures
* Fixture function should return the set of test data
![image](https://github.com/user-attachments/assets/7f1db5b5-f4cb-4ddd-838f-e1dbc7f24523)
* Fixture function should be referenced to test module to access dataset

![image](https://github.com/user-attachments/assets/97fbce04-5467-4de9-8086-b923944cdcf0)

# Custom command line options
* Support to run tests with multiple options/configurations
* `pytest_addoption(parser, pluginmanager)`: Register options and config values, called once at the beginning of a test run. This function should be implemented only in plugins or `conftest.py` files situated at the tests root directory

![image](https://github.com/user-attachments/assets/0c73468b-25ea-4459-b492-312c42843a2f)

* `config.getoption(name)` to retrieve the value of a command line option.

![image](https://github.com/user-attachments/assets/2673e7d7-5813-489e-a00c-6de2d3b2ec3a)

* Run tests: `pytest --browser_name chrome`

# Page Object Modal
## Why to use
* Common design pattern across all automation frameworks
* Apply inheritance concept in OOP
* Complete test involves multiple pages. Each page has many operations (click, type, select..) to many objects (shop link, save button, email textbox, category dropdown..) that should be move into one single class
* More readable and maintainable
## How to appy
### BasePage Class
* Create `BasePage` class (as parent class) to call setup/teardown fixture (in `conftest.py`) to initialize `driver` to class variable and contains some custom utilities (e.g: logging, selenium wrapper..)
![image](https://github.com/user-attachments/assets/091d0055-6f7a-487f-8dea-13b40d47f5e9)

### SpecificPage Class
* Define many elements (shop link, save button, email textbox, category dropdown..) and corresponding methods (click, type, select, get..) that belongs to same page
  * Should be in `pageObjects` folder and has an `__init__.py` file (to show it's a Python package)
  * Constructor `__init__` to initialize webdriver
  * Class variables to webelements
  * Class methods to actions to webelements
  
![image](https://github.com/user-attachments/assets/8f465b4f-7a76-427e-937b-407c7d990626)

### SpecificTest Class 
* Represents as test suite and defined many test cases 
  * Import corresponding modules in pageObjects files
  * Test class inherited from BasePage
  * Test methods will initialize the corresponding page class and inherit BasePage class variable self.driver
  * Call corresponding page methods to build your test

![image](https://github.com/user-attachments/assets/404942b6-c8dd-4f6f-9fcb-1bb801baf467)

### Data-driven tests
* Create class to read test data
  * Use `openpyxl` package to read excel
  * Loading excel data from file to list of dictionary
  * Use `@staticmethod `to access method directly without create class object

![image](https://github.com/user-attachments/assets/28b35c83-3527-4333-a340-024ed93e2e3a)

* Create fixture to get data

![image](https://github.com/user-attachments/assets/73ee8ffd-639f-40d4-aa69-475d1369c831)

* Test method:
  * Pass fixture name as parameter
  * Use fixture name include dictionary key to access test data

![image](https://github.com/user-attachments/assets/654a90c8-f7d8-480b-b755-16cffa7ec12c)

# Reporting and Logging
## Logging

![image](https://github.com/user-attachments/assets/86a78787-ddda-4a0c-903c-41274ba687b1)

## Reporting with logging: `pytest --html=report.html`

![image](https://github.com/user-attachments/assets/4e442620-3773-48d3-a7c6-d704a32d789e)

## Capture screenshot
Create global driver variable to access anywhere in hook file

![image](https://github.com/user-attachments/assets/7f3f498d-6854-480a-8362-dbe0dbf37bd8)

![image](https://github.com/user-attachments/assets/306e6aaa-aec9-4d9d-9cf4-893d59a96f05)
