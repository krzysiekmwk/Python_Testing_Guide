# <u><span style="color:red">Page under construction</u></span>
Python_Testing_Guide is a comprehensive tutorial designed to help beginners and intermediate Python developers master the art of unit testing. This repository provides theoretical knowledge, practical exercises, and solutions to help you write effective unit tests in Python.

# Unit Testing: A Brief Overview
<details>
  <summary>Expand</summary>

## Quick dive into
<details>
  <summary>Expand</summary>
Unit tests are written by developers during the development phase. They allow for low-level documentation and help ensure that changes do not damage the code. Often referred to as the best form of documentation, unit tests can be run at any time to check if everything is still working correctly and according to assumptions.

Unit tests should be short, focus on a specific case, and contain a structure like:
“Given arrange (preparation) -> when act (execution) -> then assert (check)”.

Like production code, unit tests should adhere to the Don’t Repeat Yourself (DRY) principle. Similar tests should be parameterized. It’s good practice to check many cases to cover as many scenarios as possible and detect as many errors as possible.

Reproducing bugs with unit tests can help avoid mistakes in the future, and other tests ensure that the code still works correctly.

The Test-Driven Development (TDD) approach follows this cycle: WRITE UT (will fail) -> WRITE CODE (turns green) -> IMPROVE CODE (UT should work) -> WRITE UT (new).

When testing, focus on public interfaces, not implementations! heck sample values, edge case behaviors, object creation, correct exceptions, and in dynamically typed languages - whether the received type is correct.

The method’s implementation should not matter. We only test the public interface so that if something changes underneath in the future, the tests should still work.

This does not forbid to test private methods. When it is necessary to verify private method - consider if this shouldn't be in public interface. But when the setup for test is very time consuming/ hard/ need to prepare a lot of mocks - it's still better to verify fully the private method, than not or only partially.

We create so-called doubles or mocks - example objects that do not exist in the real implementation, to simplify testing or speed up the code.
</details>

## Unit tests Vs Integration tests Vs E2E
<details>
  <summary>Expand</summary>
For good practice, these tests should be separate. Like: tests/unit_tests ; tests/integrations ; tests/e2e. Unit tests should be very fast, integration test can be slower (and run after unit tests). E2E should mocks as fiew things as possible.

Unit tests are designed to test a small piece of code, which could be a function or a method within a class. The purpose of a unit test is to validate that each unit of the software performs as expected.
Integration tests are designed to test how different pieces of the system work together. Unlike unit tests, integration tests are not concerned with the details of how individual components work. Instead, they focus on the interfaces between components to ensure they interact as expected.

If you want to test individual functions or methods in isolation, use unit tests. If you want to test how different parts of your system work together under different scenarios, use integration tests. In practice, a good testing strategy involves a combination of both unit and integration tests. E2E tests involve testing of a complete application environment in a situation that mimics real-world use, such as interacting with a database, network, hardware, or other applications.

#### Example
Let's say we have a WebPage, and we wrote a 'login_user(user, password)' method, which connects to database to verify if user exists and then redirect to user page. All by some form on webpage.

Unit Test - We will verify if 'login_user(user, password)' function behaves as expected when given a set of inputs. For example, it might check that given a correct username and password, the function returns a success message and updates the user’s session data. This would be done in isolation from any actual databases or other external systems.

Integration Test - Here we will prepare database with existed user. By calling 'login_user(user, password)' we will verify if received information from database is proper, and e.g. if session data for given user is updated.

E2E - Here we will simulate user experience. Using test framework (e.g. Selenium) we will open WebPage, to fill form, click some "Login" button and verifies if page is redirected to proper page.

This is a very basic example, and sometimes there will be needed of mix of those types.

For example, there are 'module' tests - something between unit tests and integration tests - we still don't have connection to database - but (TODO) / aka component + wrzuć tabelkę testów (jako obrazek).
</details>

## Good practice of testing (theory)
<details>
  <summary>Expand</summary>
Keep tests small and independent: Tests should be independent of each other, meaning the result of one test should not affect the result of another.
Test one thing at a time: Each test should focus on one specific aspect of a function. If a function has multiple tasks, write multiple tests for it.
Ensure tests are deterministic: Tests should always give the same result no matter how many times they are run. Avoid dependencies on external states or data that may change between runs.
Write tests before code: This technique, known as Test-Driven Development (TDD), involves writing tests before writing the code itself. This helps ensure all functions are adequately tested. About TDD there are other scholars (see attached links).
Use assertions appropriately: Assertions help determine whether a test has passed or failed. Each test should include at least one assertion.
Clean up after your tests: If your test creates temporary files or makes changes to a database, make sure these changes are undone after the test is over.
Don't use 'if' statements: When you find yourself wanting to use an ‘if’ statement in a test, it often indicates that the test is trying to accomplish too much and could be split into multiple smaller tests. Each test should ideally have a single reason to fail.
example of bad test
def test_add():
    result = add(2, 3)
    if isinstance(result, int):
        assert result == 5
    else:
        assert result == '5'
Test split into two tests
def test_add_integers():
    assert add(2, 3) == 5
 
def test_add_strings():
    assert add('2', '3') == '5'
Test split into two tests
TODO - PARAMETRIZE (arg1, arg2, result)
def test_add_integers():
    assert add(2, 3) == 5
 
def test_add_strings():
    assert add('2', '3') == '5'
How to avoid 'ifs' in tests?: Mock objects properly (return_value, side_effect) or split tests into other tests or parameterize test, to run with other conditions.
</details>

## unittest vs pytest
<details>
  <summary>Expand</summary>
unittest and pytest are both testing frameworks in Python, but they have some key differences:

Test Implementation: In unittest, tests are written as methods in a class that inherits from unittest.TestCase. In pytest, tests are more simple and flexible. They’re written as functions, and you don’t need a class or inheritance. This makes the test cases in pytest more pythonic. Just start test name with "def test_".

Setup/Teardown: unittest uses setUp and tearDown methods within the test class to set up and tear down test state. These methods run before and after each test method execution. In contrast, pytest uses fixtures, which are more explicit, modular, and scalable than setUp and tearDown. Fixtures also have scope levels (function, class, module, package or session) which provides more flexibility. Fixtures can works as setUp/tearDown - just use 'yield', to makes them as generators. Code before yield will be executed (setUp part), and on the end code after yield (tearDown).

Assertions: unittest uses special assertion methods like assertEqual(a, b), assertTrue(x), etc. On the other hand, pytest uses plain assert statements, which are easier to write and read. Also, there is possibility to write assertion, based on Mocks (eg: obj.assert_called_once).
</details>

## Coverage
<details>
  <summary>Expand</summary>
Coverage.py is a tool for measuring code coverage of Python programs. It monitors your program, noting which parts of the code have been executed, then analyzes the source to identify code that could have been executed but was not.

Coverage measurement is typically used to gauge the effectiveness of tests. It can show which parts of your code are being exercised by tests, and which are not.

There is possibility to use coverage or pytest plugin.
pip install coverage
coverage run my_program.py arg1 arg2
You can report on it using 'coverage report', or create a detailed HTML report with 'coverage html'.
coverage report -m ; coverage html

Using with pytest:
pip install pytest-cov
python -m pytest --cov=my_program tests/
Or more specific with html report, specified configuration, choosen directory to monitor and where tests are located.
python -m pytest --cov-config=.coveragerc --cov-report html --cov=tests/robot tests/ut/
</details>

</details>

# Unit Testing Workshop: A Structured Approach

## Basic

## Introduction to Mocking
Understand what a mocker is and how to use mocker.patch to return a mock.
Learn about monkeypatch, with patch (as), @patch, and with patch.object.
Understand that a mocked class returns an object.
Learn the correct order of patching and how to do multiple patching under one with.

Plusy i minusy danych rozwiązań. że np with pod blockiem a @patch że cały test

Proste return_value (bądź nie).

## Mocking Properties
Learn how to mock a property that returns an object.

## Return Values - fixtura - new=DEFAULT
Understand the use of new="nt" for mocking, not as a return value, but without wrapping in an argument.

## Parameterizing Tests
Learn how to parameterize tests using pytest.mark.parametrize.

## Using Fixtures
Understand what a fixture is and how it can be used to return an object or clean up tests before and after (yield / return). (stworzenie i usunięcie pliku) + return
Learn about parameter injection and parameterizing fixtures. (indirect)

## Testing Exceptions
Pokolorować headery (żeby łatwiej było szukać).

Warstwa teoretyczna:

Learn how to test exceptions using pytest.raises.
Understand why it’s important to raise specific exceptions instead of generic ones like ValueError or Exception.

KOD:

def raisable_example():

  raise MY_EXCEPTION("jakis string do sprawdzenia)

(ścieżka w sandboxie)

Treść:

tutaj Warto sprawdzić match=regex

Drugi patent: assert "STR" in str(err.value)

rozwiązanie - 2 testy. - 2x collapsible match / assert

W rozwiązaniu napisać o tym:

NIE:

with pytest.raises(Exception) ale: with pytest.raises(MY_EXCEPTION)

## Side Effect
Metoda "próba logowania" - za 1 razem odrzuć, za drugim zaloguj.

Napisana metoda - która próbuje zalogować (wielokrotnie) - max 3.

Try: try_logg()...

W TEŚĆIE - ZADANIE - sprawdzić czy wystąpiły 2 próby zalogowania. Użyć side_effect. Hint: [TimeoutError, True]

Side_effect 3x - wtedy zobaczyć czy metoda rzuci "INNY_ERROR" (LogginFailed_czyCosTakiego). → sprawdź call 3x + with raises.

## Assertions on Mocks
Learn how to use assert has call (call()) and other assertions on mocks (e.g. assert_called_once).
Understand the concept of attach mocks for testing flow in large interface methods.

## Inne pomysły - tematy powiązane.
DATETIME (freezegun) -> ciekawostka

np sprawdzenie czy aktualnie mamy 2022rok.

bardziej scenariusz - mamy człowieka urodzonego w X czasie, czy ma 18 lat?



request + request mock - ciekawostka.

Przykład z api - callowanie kotków



W kodzie mamy if isinstance(x, DUPA): return True

to do mocka przekazać MagicMock(spec_type=DUPA)



with patch("builtins.open") as d... ręczne mokowanie __enter__ __exit__ 

with patch("builtins.open", mock_open(read_data=json.dumps(data_sync))):

powiedzmy czytanie requirementsów.
