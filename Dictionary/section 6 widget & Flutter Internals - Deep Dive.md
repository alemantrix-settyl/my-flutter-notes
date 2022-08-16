## Section 6: Widgets & Flutter Internals - Deep Dive

- Flutter draws one time and show old data cause it changes the UI on 60 FPS and calculating each time is resource consuming.
- Flutter has not only widget tree, it has element tree and render tree. 
- We control widget tree.
- Element is an object that is managed in memory by Flutter which holds a reference at its widgets. 
- Element itself has no internal configuration.
- The `build` method is called by flutter whenever your state changes. 
- All Widgets are actually a `class` which we are instantiating. 
- We are just overriting the properties of class by removing its old memory.
- Also with `MediaQuery`, widgets automatically rebuild when it changes.

## How Flutter rebuilds & repaints the screen
State is connected to the element not to widget. when we call setState, we add something to state and make MyStateful to dirty then NewMyStateful is created which gets stored in state and that gets rendered too. And new one replace the old one. </br>
- state don't get rebuild. it calls `build` which reinstantiate all the classes. 
- An element knows which kind or which type of widget it was connected to.
- Every element knows its child element, that is connected to it and which type of widget held the configuration. 
- Reference is actually updated & old reference is cleared. 
- Any widget which uses MediaQuery will rebuild no matter what data you then access from there.
- Whenever mediaQuery changes, everything that depened on it will rebuild.

## Using `const` widgets & constructor
- It's good to avoid to rebuild unnecessary widgets.
- put `const` in front of constructor. 

```
final String label;
final double spendingAmount;

const ChartBar(this.lable, this.spendingAmount);
// you can use `const` if you bind all your arguments here in the constructor to final properties.
```
- If you use const before constructor then it is clear that your intention with this classes, that every new object that's created based on it is **immutable** only change with new instance. 
- In Theory, every stateless widget you build is immutable and you can add const in front of the constructor. 
```
label: const Text("Delete");
// when build gets called then it won't get reconstructed. #good_performance
```
- Don't use too much if's, make a function and use conditions there.
```
Widget _buildLandscapeContent(MediaQueryData mediaQuery, AppBar appBar) {
}

if(isLandscape) _buildLandscapeContent()
```

## Widget Lifecycle

- `super:` is a keyword in dart which refers to the parent class. 
- call `super.initState` first, then write code inside initState. #official_recommendation.
```
@override
void didUpdateWidget(NewTransaction oldWidget){
    super.didUpdateWidget(oldWidget);
}
// This gets called when the widget that is attached to the state changes.
```
```
dispose(): when widget is removed from screen then it called.
```
Q) When would you use that? </br>
`initState` is used for fetching some initial data you need in your widget. </br>
`didUpdateWidget` is used less often, you could use it if you know something changed in your parent widget and you need to refetch data in your state. </br>
`dispose` is great for cleaning up data. Cleaning up listeners or live connections.

## Stateless widget
`Constructor function > build()`

## Stateful Widget
`Constructor function > initState() > build() > setState() > didUpdateWidget() > build() > dispose()`

- **It is recommended to call super.initState first.**

- `mixin:` To add certain property or method that other class has without fully inheriting this other class | by using 'with'

```
class _MyHomePageState extends state<MyHomeState> with widgetsBindingObserver {...}

@override
void initState() {
    WidgetsBinding.instance.addObserver(this);
    super.initState();
}

@override
void didChangeAppLifecycleState(AppLifecycleState state) {}

@override
dispose () {
    WidgetsBinding.instance.removeObserver(this); // call this to avoid memory leaks
    super.dispose();
}
```
With this line, you are telling Flutter: hey whenever my lifecycle state changes, I want you to go to a certain observer and call the `didChangeAppLifeCycleState` method, that's what Widgets binding tells Flutter.

| Lifecycle State Name | When is it hit? |
| ------------ | :-----------: |
| inactive     |   App is inactive, no user input received   |
| paused    |   App not visible to user, running in background   |
| resumed     |   App is (again) visible and responding to user input   |
| suspending     |   App is about to be suspended (exited)   |

## Understanding the context

- Every widget has a context.
- context is the element of a widget in the element tree, and has some meta information about the widget and its location in the widget tree.
- context gives you a direct communication channel across your entire widget tree.
- It has meta information on the widget & its location in the widget tree. 

## Using keys
If you have list and in list you have stateful widgets, situation like this, only need to use key when it's the topmost item in a list or wrong state attach to your element.
```
TransactionItem(
    key: UniqueKey()
    // it automatically creates a unique key for every items.
)
```

- `super(key: key):` You instantiating the parent class. To forward to parent widget. 
- don't need to do through your own, only need if you want to pass some extra data to the parent class. 
- with new keys generated constantly flutter finds no widgets for the elements that now look for widget type + key. Hence, new state objects are created all the time.
- `ValueKey():` That is a key where you pass your own identifier all it be a value that is unique to every list item you have. #better_than_UniqueKey();

```
key: ValueKey(tx.id); // id basically
```
