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
