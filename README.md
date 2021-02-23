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
