Test-Driven Development.

## xUnit架构：
- Test Runner: 运行测试并报告测试结果
- Test Case: 最基本类，所有单元测试都继承于此
- Test Fixtures: 也称Test Context, 是precondition(是一个条件或谓词predicate——必须在执行前保证为true)集合，或运行一个测试所需的状态。开发者需要在测试前设置好已知正确状态，在测试后返回最初状态。
- Test Suite: 共享相同Fixture的测试集合。测试的顺序无所谓。
- Text Execution: 独立的单元测试按照Setup(初始化) -> Test -> TearDown(清理CleanUp)进行
- Test Result formater :报告输出格式
- Assertions: 断言是验证行为（或状态）的函数或宏。

## Test Framework
C#/Unity: [NUnit3](https://nunit.org/)
C++: [GoogleTest(gtest)](https://github.com/google/googletest), [Cache2](https://github.com/cacheorg/cache2) [doctest](https://github.com/doctest/doctest)


