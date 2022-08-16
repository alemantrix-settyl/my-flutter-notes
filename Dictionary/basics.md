# Flutter Basics

`SDK:` A collection of tools that allows you to write one codebase. </br>
`Flutter:` A "tool" that allows you to build native cross-platform apps with one programming language and codebase.</br>

### Overview of files and folders
- `.idea:` For Android Studio
- `Android:` Flutter compiled this for android
- `build:` It holds output of flutter apps
- `lib:` Our library
- `test:` Allow to write test for oru application
- `pubspec.lock:` Details of dependencies
- `README.md:` Info. about our project.
---
- You have to make `type` of everything in dart else it become dynamic.
- EVERYTHING IS OBJECT (having property and methods)
- If variable value changes frequently then use `var` keyword.
- `num` works has `int` and `double`
- `.toString():` Convert to string.
- `class` name must be Pascal Case.
- `context` is a special type of object which be passed into the build method automatically, flutter will call it whenever it needs to draw something onto screen.
- `runApp():` Special function to run apps that calls build constructor. </br>
- `@required:` to require scope in constructor.
- `MaterialApp` doesn't have any styling. `Scaffold` creates a base page desing.
- `ctrl + space:` get list of arguments of widget.
- `row:` items next to each other.
- `void` is type but `null` is a value.
- All data, widgets, functions should belongs into the same class, not outside the class, so that the widget is a standalone unit.
- WRONG: `onPressed: answerQues()` this will get called on run time 
- RIGHT: `onPressed: answerQues` this function will execute when the button get pressed.
- In most programming languages, list indexes start at 0 (not 1). The first element is accessed via index 0.
```
var questionIndex = 0;

void answerQuestion () {
    questionIndex = questionIndex + 1;
    print(questionIndex);
}
```
- This will increment when button get pressed
- cause state is not changing, so `build` won't run again, so `questionIndex` will not set to 0 again.
- if state changes then `questionIndex` will be 0;
- `./` look in same folder.
- `setState():` is a function that forces flutter to re-render the UI, not the entire UI of the entire app, `it just call build again when setState changes`.
- without setState you change the data but flutter never executed build again.
- `_` Private property
- one widget per file.
- if we import other file then all its properties and methods will get imported unless you use _ (underscore).
- `final:` tells dart that this value will never change after its initialization here in the constructor.
---
### Lifting the state up
You manage the state on the shared, on the common denominator of the different widgets that need this state and that is direct parent of these widgets that need the state.</br>
The answer for this problem is simple - Just as we can pass text to a widget, we can pass a pointer at a function to a widget. The function(address) we're passing around is also known as a "callback" - because the receiving widget calls it in the future.
- The map method executes a function which you have to pass as an argument to map on every element in the list on which you are calling map. map returns a new list.
- `Spread operator (...):` They take a list and they pull all the values in the list out of it and add them to the surrounding list as individual values.
---
`final` is runtime constant. Use when we don't know the value at the point of time we're writing this code.
`const` is compile time constant.
`pointer == address`
```
const question = const [];
// where to add const?
```
The difference is that if you add it in front of the variable name, the variable is constant, if you add it in front of the value, the value is constant.

```
var question = const []
// address will vary
//  value is constant

const question = var []
// address is const
// value will vary
```
- We are actually storing address in variables instead of values.
- If you have data where you know it will not change once it has its initial value, then make it `final`.
- Things inside of `build` is only available to itself not for the entire `class`.
---
`null` is a value built into dart which you can use if you might expect another type of value, but then something in your application happens and you want to reset that to an uninitialized state. </br>
```
var userName = "Max";

// something happen
userName = null;
userName = "yo";
```
### Ternary Operator
`<condition> ? <execute-if-true> : <execute-if-false>`;

---
`Getter:` is a special type of property, its basically a mixture of property and method. </br>
It never receive any argument, we always return something in getter.

```
String get result {
    String resultText;
    if (resultScore <= 8) {
        resultText = "Awesome";
    }
    return resultText;
}
```
### Reset the quiz
```
void _resetQuiz() {
    setState(() {
        _questionIndex = 0;
        _totalScore = 0;
    });
}
```
### Handler
```
FlatButton(
    child: Text('Restart Quiz!'),
    onPressed: resetHandler // this function comes from constructor.
)
```
---
Logical error that won't show error but affect the app during runtime.
- by Print() `debugger`
- run app debugger `debugger` </br>
You can add breakpoint to a line. Breakpoint will halt the code execution whenever it reaches this code line.
---
