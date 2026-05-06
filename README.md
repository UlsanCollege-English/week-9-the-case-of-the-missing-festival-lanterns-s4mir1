[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/QtC5AQlU)
# Week 9 Homework: The Case of the Missing Festival Lanterns

## Student Info

Name: Festival Lantern Analyst  
Student number: 2024001  
GitHub username: lantern-analyst  

---

## Summary

The `analyze_lanterns` function solves the problem of tracking festival lanterns across different sections of a festival grounds. It receives three inputs: a set of expected lantern names, a log of (lantern, section) records, and a dictionary of where each lantern should correctly be placed. The function processes these inputs and returns a detailed report dictionary. The report identifies which lanterns were seen, which are missing, which appeared unexpectedly, which appeared more than once, how many lanterns were recorded per section, and which expected lanterns ended up in the wrong section.

---

## Approach

- First, I created empty collections: a `seen_lanterns` set, a `seen_once` set to track duplicates, a `duplicate_lanterns` set, a `count_by_section` dictionary, and a `wrong_section_lanterns` dictionary.
- During the loop, I added each lantern name to `seen_lanterns`, used `seen_once` to detect if a lantern had already appeared (marking it as a duplicate on the second visit), incremented the section count in `count_by_section`, and checked if an expected lantern was placed in the wrong section.
- For wrong-section checking, I only checked lanterns that exist in `expected_lanterns`, and only recorded the first wrong section by guarding with `if lantern_name not in wrong_section_lanterns`.
- After the loop, I used set difference (`-`) to compute `missing_lanterns` and `unexpected_lanterns`.
- Finally, I returned all six results packed into a single dictionary.

---

## How I Used Dictionaries and Sets

1. **Sets used**: `seen_lanterns` (all lanterns encountered), `seen_once` (to detect duplicates), `duplicate_lanterns` (lanterns seen more than once), `expected_lanterns` (passed in as input). Set difference (`-`) was used to find missing and unexpected lanterns after the loop.
2. **Dictionaries used**: `count_by_section` maps each section name to a count of how many log entries it has. `wrong_section_lanterns` maps a lantern name to a nested dictionary containing its expected and actual section. `correct_sections` (passed in as input) was used for O(1) lookup of where each lantern should be.
3. **Why better than lists**: Sets allow O(1) membership testing (`in` checks), which makes duplicate detection and set difference operations fast and clean. Dictionaries allow O(1) key lookup and direct grouping by section, which would require nested loops or sorting if done with only lists.

```text
Sets were used for seen tracking and duplicate detection.
Dictionaries were used for section counting and wrong-section recording.
Both are faster and cleaner than lists for lookup-heavy tasks.
```

---

## Complexity

```text
Time complexity: O(n)
Space complexity: O(n)
Explanation: The solution loops through lantern_log exactly once (n = number of records).
There are no nested loops. Inside the loop, all operations — set membership checks,
dictionary lookups, and insertions — are O(1) on average.
Extra space is used for seen_lanterns, seen_once, duplicate_lanterns (all sets, up to n entries),
count_by_section (up to n unique sections), and wrong_section_lanterns (up to n entries).
So both time and space scale linearly with the size of lantern_log.
```

---

## Edge-Case Checklist

Check the cases your solution handles.

- [x] empty `lantern_log`
- [x] empty `expected_lanterns`
- [x] no missing lanterns
- [x] no unexpected lanterns
- [x] duplicate lanterns
- [x] wrong-section lanterns
- [x] unexpected lanterns ignored for wrong-section checking

Add one more edge case you thought about:

```text
A lantern appears three or more times — it should still only appear once in
duplicate_lanterns, not be added multiple times. The set handles this correctly
since adding an existing element to a set has no effect.
```

---

## Tests I Added

The starter tests are already provided.

You must add at least one meaningful test of your own in:

```text
tests/test_challenges.py
```

```python
def test_analyze_lanterns_all_correct():
    """All expected lanterns are present and in the correct section."""
    expected_lanterns = {"river-dragon", "blue-crane"}
    lantern_log = [
        ("river-dragon", "North Gate"),
        ("blue-crane", "River Walk"),
    ]
    correct_sections = {
        "river-dragon": "North Gate",
        "blue-crane": "River Walk",
    }

    result = analyze_lanterns(expected_lanterns, lantern_log, correct_sections)

    assert result["seen_lanterns"] == {"river-dragon", "blue-crane"}
    assert result["missing_lanterns"] == set()
    assert result["unexpected_lanterns"] == set()
    assert result["duplicate_lanterns"] == set()
    assert result["wrong_section_lanterns"] == {}
```

```text
Test name: test_analyze_lanterns_all_correct
What it checks: When all expected lanterns are present and in the right section,
                all problem-report fields come back empty.
Why it matters: It verifies the "happy path" — a perfectly run festival should
                produce no warnings or errors in the report.
```

---

## How to Run the Tests

```bash
pytest -q
```

Paste your final test result here:

```text
7 passed in 0.03s
```

---

## Assistance and Sources

```text
AI used? Y
What it helped with: Explaining the approach, reviewing edge cases, and helping complete the README.
Other sources used: Python documentation for set operations and dict.get()
```

---

## Submission Self-Check

Before submitting, check:

- [x] I completed `analyze_lanterns` in `src/challenges.py`.
- [x] I added at least one meaningful test of my own.
- [x] `pytest -q` passes.
- [x] I completed this README.
- [x] I pushed my latest work to GitHub.