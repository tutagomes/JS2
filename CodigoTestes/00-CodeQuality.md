## **Code Quality**

![1*-nhSONLZxx0wcOsr20KULw](https://miro.medium.com/max/550/1*-nhSONLZxx0wcOsr20KULw.png)



Agile teams ensure code quality using the following approaches

1. **Code**

2. 1. Static analysis tools

   2. 1. These look for badly named variables
      2. Unused variable detection
      3. Unreachable code detection
      4. Duplication of code detectors

   3. Perfomance analysis

   4. Cyclomatic complexity

   5. Documentation of code functionality

   6. 1. In line documentation of functions which can also be exported with a tool like Doxygen.

3. **Team**

4. 1. Code reviews

   2. 1. Multiple people looking at the code helps improve the quality of the codebase overtime.

   3. Peer programming

   4. 1. Again multiple people analysing code written but provides the fastest feedback

   5. Collaborative design meetings

   6. 1. Drawing on the opinions of many individuals especially when they have domain expertise and are cross functionaly skilled is very effective.

   7. Collaborative code reviews

   8. 1. Like above multiple views reduces the chance of errors and improves the design and quality.

   9. Collective code ownership

   10. 1. Holding the whole team accountable creates peer pressure and pride in the code being produced

   11. Module responsibility

   12. 1. Delegating module responsibility elects someone as being responsbile for a specific part of the system. This can help ensure quality as people want to be proud of work undertaken.

5. **Automation**

6. 1. Automated verification on commit of changes

   2. Automated testing of all committed changes

   3. Fast feedback of code committed which doesnt meet quality standards

   4. Regression testing being automated means the team can be confident to make changes and even big changes without breaking functionality

   5. 1. This can greatly improve the quality of the code and design

7. **Process**

8. 1. Inspection

   2. 1. Looking for common issues in code quality and effeciency

   3. Reflection

   4. 1. Looking for solutions to common problems observed

   5. Adaption

   6. 1. Making changes to the process or tools to improve on the issues identified

   7. Removal of wastefull actions

   8. 1. Waste is defined as anything which does not add value to a stakeholder or customer

   9. Low amount of technical documentation

   10. 1. Keeps processes light and stops using development budgets on writing documentation of questionable usefulness
       2. More budget to spend on code quality and development

9. **Testing**

10. 1. TDD - Test Driven Development

    2. 1. The concept is of writing tests before the software and then making sure your implementation can suceed line for line against the test cases.

    3. BDD - Behaviour Driven Development

    4. 1. Very similar as above but focuses on the overall behaviour of the code and makes sure the code behaviour matches what the business requires.

    5. Unit tests

    6. Integration tests

    7. Unit testing coverage tools

    8. 1. NCover
       2. OpenCover etc..

    9. Automated GUI testing

    10. 1. Selenium

There are plenty more but these are some of the most common ways to ensure quality on a agile software project.

Hopefully this helps