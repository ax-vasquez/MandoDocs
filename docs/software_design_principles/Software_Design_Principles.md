# [Software Development Antipatterns](https://en.wikibooks.org/wiki/Introduction_to_Software_Engineering/Architecture/Anti-Patterns)
It's important to understand that an "anti-pattern" is not necessarily "bad-practice". In fact, good design patterns can become anti-patterns. _The important thing to think about what exactly you are trying to accomplish_.

There must be at least two key elements present to formally distinguish an actual anti-pattern from a simple bad habit, bad practice, or bad idea:
1. Some pattern of action, process or structure that _initially appears to be benefecial, but ultimately produces more bad consequences than beneficial results_
2. A refactored solution exists that is clearly documented, proven in actual practice and repeatable

## Antipattern Table
| Antipattern | Violates | Symptoms |
| ------------ | -------- | ----------- |
| Singleton Overuse | Information Hiding | Dependent on needs |
| Functional Decomposition | OOP Principle | Class names that sound like functions (e.g. `CalculateInterest`), Classes with only one action |
| Poltergeist | Clear-purpose of classes | Classes that are mysterious and/or highly-limited in scope |
| Spaghetti | Your eyeballs | Big, ugly classes with lots of uncommented code |
| Blob | Clear-purpose of classes | Classes with an eclectic, unrelated mix of methods and attributes |
| Copy-and-Paste | DRY Principle | Frequent use of `ctrl+c` and `ctrl+v` - DON'T DO IT |
| Lava Flow | Cognitive Continuity | Classes that work, but have not been touched in a very long time |

### Tools to Combat Antipatterns
1. [SourceMonitor](http://www.campwoodsw.com/sourcemonitor.html)
    - Examines codebase (up to 10,000 lines of code per second) for antipatterns
    - It lists all class names and also lists the functions

## Code Smells
"Code Smells" are similar to anti-patterns, but not quite as formal. Code smells are not necessarily indicative of a problem, but it should highlight areas in the code that should be more-carefully reviewed.
1. Duplicate Code
    - Similar to "Copy-and-Paste" antipattern
2. Long Method
    - Similar to "Spaghetti" antipattern
    - Methods longer than 50 lines are definitely suspicious
3. Indecent Exposure
    - This can happen when a code has too many public methods
    - If a class has more than 50% public methods, then it _may not_ conform to the information hiding policy
4. Lazy Class
    - Classes that do so little that it has no reason for existence
    - Look for classes with fewer than two methods, as these are likely more suspect
5. Large Class
    - Usually, classes should not have _more than 30 methods_ or _more than 400 statements_