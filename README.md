# tau-behavior-driven-python-with-pytest-bdd

- [Behaviour Driven Python with Pytest-BDD](https://testautomationu.applitools.com/behavior-driven-python-with-pytest-bdd/)  
- [Introduction To Behaviour Driven Python with Pytest-BDD by Andrew Knight](https://testautomationu.applitools.com/pytest-tutorial/)

## Requirements for this course

- Python must be installed 3.8.1 - [Python can be downloaded from here](https://www.python.org/downloads/)

Useful URLs

- [Automation Panda - BDD](https://automationpanda.com/bdd/)
- [Python BDD Framework Comparison](https://automationpanda.com/2019/04/02/python-bdd-framework-comparison/)
- [Cucumber](https://cucumber.io/)
- [Pytest](https://docs.pytest.org/en/latest/)
- [Pytest Repo](https://github.com/pytest-dev/pytest-bdd)
- [Pytest-BDD Documentation](https://pytest-bdd.readthedocs.io/en/stable/)
- [Requests Package](https://docs.python.requests.org/)
- [Requests: HTTP for Humans](https://requests.readthedocs.io/en/master/)
- [Instant Answer API](https://duckduckgo.com/api)

URLs carried over from Selenium WebDriver with Python Course

- [Selenium WebDriver With Pyton](https://testautomationu.applitools.com/selenium-webdriver-python-tutorial/)
- [TAU - Repo](https://github.com/AndyLPK247/tau-intro-selenium-py)
- [Beneficial To Have Followed Previously](https://testautomationu.applitools.com/python-tutorial/)
- [Python can be downloaded from Python.Org](https://www.python.org/downloads/)
- [WebDriver API - Read the Docs](https://selenium-python.readthedocs.io/api.html)
- [Waits - Implicit and Explicity](https://selenium-python.readthedocs.io/waits.html)

Further Reading

- Python Testing with pytest by Brian Okken
- pytest Quick Start Guide by Bruno Oliveira

## Commands To Run In The Terminal

- `pipenv install`
- `pipenv install pytest`
- `pipenv install pytest-bdd`

## Chapter 3 - Writing A Basic Test

Added `cucumbers.py` file to manage cucumbers, known as a cucumber basket.

Added folders to within the tests folder:

```bash
    - features
    - step_defs
```

### VS Code Extension Install

Through Visual Studio Code Extensions search for and install

- `Pytest-BDD`

### Error Traps

If when running the tests you get a failure, it is worth checking the formatting in the your feature file ensuring that the `When` and `Then` steps are not indented like below.

```bash
Scenario: Add cucumbers to a basket
  Given the basket has 2 cucumbers
    When 4 cucumbers are added to the basket
    Then the basket contains 6 cucumbers
```

The format need to be like so

```bash
Scenario: Add cucumbers to a basket
  Given the basket has 2 cucumbers
  When 4 cucumbers are added to the basket
  Then the basket contains 6 cucumbers
```

When running tests, remember, it is good practice to have test in the name so renaming `cucumbers_steps.py` to `test_cucumbers_steps.py` unless you give the command for the full path check out the below commands.

- Using this command `pipenv run python -m pytest` means you need test in the name
- Passing in the full path `pipenv run python -m pytest tests/step_defs/test_cucumbers_steps.py` would mean you wouldn't need to have `test` in the name of the `step definition`

## Chapter 4 - Parametrizing Steps

- Withina feature file, a parameter can be indicated by using doulbe quotes around what we want to parametrize `Given the basket has "2" cucumbers`
- We also need to import `Parsers module` from `pytest_bdd package` this can be done like so `from pytest_bdd import scenario, parsers, given, when, then`

## Chapter 5 - Scenario Outline

### Fature File Changes To Support Scenario Outlines

The 1st steps to take is to plan out you scenario's in the feature file like in the below example.

Note: An examples table has been added, as well angle brack `<initial>` where the value would normally be

```bash
  Scenario Outline: Add cucumbers to a basket
    Given the basket has "<initial>" cucumbers
    When "<some>" cucumbers are added to the basket
    Then the basket contains "<total>" cucumbers

    Examples: Amounts
      | initial | some | total |
      | 2       | 4    | 6     |
      | 0       | 3    | 3     |
      | 5       | 5    | 10    |
```

### Step Defintion Changes

Added Converters dictionary and applied to the scenarios for the entire feature file

```bash
CONVERTERS = {
    'initial': int,
    'some': int,
    'total': int,
}

scenarios('../features/cucumbers.feature', example_converters=CONVERTERS)
```

Example of changes to given step definition added/updated.  
The reason we have 2 given steps for the same thing is so that we can use both if the need arises in future.

```bash
@given(parse_num('the basket has "{initial:Number}" cucumbers'), target_fixture='basket')
@given('the basket has "<initial>" cucumbers')
```

## Chapter 8 - Filtering With Tags

Tags can be added at `Feature`, `Scenario` or both `Feature and Scenario` level.
`Feature` level tags give the benefit of all scenarios inheriting the tag.

If you run these the tests now there will be warnings, something the course doesn't tell you do is create a `pytest.ini` at
the root of the project and add the below into it.

```bash
[pytest]
junit_family = xunit2
markers =
  add
  cucumber-basket
  duckduckgo
  remove
  service
  web
testpaths = tests
```

### Commands For Filering

You can run tests based on a folder or filter to specific tests

- `python -m pytest tests` - runs tests at folder level
- `python -m pytest -k "cucumber-basket"` - runs tests that contain a certain the substring 'one'
- `python -m pytest -k "add"` - runs tests that contain the `add` tag in this case the tag is set at scenario level
- `python -m pytest tests/step_defs/test_cucumbers_steps.py` - runs tests with the `cucumber-basket` tag
- `python -m pytest -k "duckduckgo and service"` - runs tests with the `duckduckgo` and `service` tags
- `python -m pytest -k "cucumber-basket or service"` - runs tests with `cucumber-basket or service` tags
- `python -m pytest -k "not web"` - runs tests by exluding tags, which means anything that doesn't have a `web` tag

### Command Console Output

Some of these may not work in this repo as the necessary insalls may not have taken place.  Imported from another course in this series.

- `python -m pytest --help`
- `python -m pytest` - runs all tests
- `python -m pytest --verbose or -v` - pytest prints more data, enables you to see more data at a glance
- `python -m pytest --quite` - Gives dots of F if a test fails e.g ....F...
- `python -m pytest --exitfirst or -exit`- Stops after the 1st failure
- `python -m pytest --maxfail` - Gives flexibility on how many tests can fail prior to execution exiting
- `python -m pytest --junit-xml report.xml` - will generate a file called report.xml

## Chapter 9 - Shared Steps and Hooks

Added a `confest.py` file within the `step_defs` folder and populated it with contents from `test_web_steps.py` unless otherwise stated

### Imports

```bash
import pytest

from pytest_bdd import given
from selenium import webdriver
```

### Constants

```bash
DUCKDUCKGO_HOME = 'https://duckduckgo.com/'
```

New hook written.

### #Hooks

```bash
def pytest_bdd_step_error(request, feature, scenario, step, step_func, step_func_args, exception):
    print(f'Test Step Failed: {step}')
```

### Browser Fixture

```bash
@pytest.fixture
def browser():
    # For this example, we will use Firefox
    # You can change this fixture to use other browsers, too.
    # A better practice would be to get browser choice from a config file.
    b = webdriver.Chrome()
    b.implicitly_wait(10)
    yield b
    b.quit()
```

### Given Step

```bash
@given('the DuckDuckGo home page is displayed', target_fixture='ddg_home')
def ddg_home(browser):
    browser.get(DUCKDUCKGO_HOME)
```

#### Testing Failure

To prove these changes work in terms of the failure, I added a new `Scenario` with `@fails tag` ensuring to update `pytest.ini` with the new tag and finally a `Then Step`.

#### Scenario

```bash
  @fails
  Scenario: Basic DuckDuckGo Search Expected Fail
    When the user searches for "panda"
    Then results are wrong for "panda" intentionally fails to demonstrate
```

#### Then Step

```bash
@then(parsers.parse('results are wrong for "{phrase}" intentionally fails to demonstrate'))
def results_have_two(browser, phrase):
    xpaths = "//div[@id='links']//*[contains(text(), '%s')]" % phrase
    resultss = browser.find_elements_by_xpath(xpaths)
    assert len(resultss) < 0
```

#### Command To Test New Failing Scenrio

`python -m pytest -k "fails`
