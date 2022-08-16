## Section 5: Responsive & Adaptive user interface and apps

To get the available device height.
`MediaQuery.of(context).size.height` </br>

To get 20% of available device height.
`MediaQuery.of(context).size.height * 0.2` </br>
```
const appBar = AppBar(...)
Container(
    height: (MediaQuery.of(context).size.height - appBar.preferredSize.height - MediaQuery.of(context).padding.top) * 0.4
)
// the padding because of notch or status bar
```
## Working with textScaleFactor.

## Using the LayoutBuilder Widget
Flutter uses this concept of 'builder functions' quite often. YOu saw something similar on ListView.builder, for ex- </br>
`Constraints` </br>
`0 < height < infinity` </br>
`0 < width < infinity` </br>

Q) If not sure how much space you have? use `LayoutBuilder`.</br>

```
LayoutBuilder (
    builder: (ctx, constraints){
        return Column(children: [
            Container(
                height: constraints.maxHeight * 0.6
            )
        ])
    }
)
// The constraints argument in LayoutBuilder takes a constraint which give the information about the constraints.
```
## Controlling the device Orientation

```
import "package:flutter/services.dart";

void main(){
    WidgetsFlutterBinding.ensureInitialized();
    SystemChrome.setPreferredOrientations([
        DeviceOrientation.portraitUp,
        DeviceOrientation.portraitDown
    ]);
    runApp(MyApp())
}
// now the app won't be Landscape, only portrait up & down will available. 
```

## Showing different content based on device orientation

```
final isLandscape = MediaQuery.of(context).orientation == Orientation.landscape;

if(isLandscape) Row(...)
if(!isLandscape) Container(...)
```

## Respecting the softkeyboard insets
`viewInsets:` This gives information about anything that's lapping into our view, typically that's the soft keyboard.

```
Container(
    padding: EdgeInsets.only (
        top: 10,
        left: 10,
        right: 10,
        bottom: MediaQuery.of(context).viewInsets.bottom + 10
        // This will give 10px bottom padding when keyboard comes up but yellow lines comes up because its not scrollable. 
        // Solution: Wrap container in SingleChildScrollableView()
    )
)
```
## SingleChildScrollableView()

It has the information about how much space is taken up by the bottom keyboard to still move everything such that users can interact with your application and reach all the inputs.

## Using the device size in conditions
```
trailing: MediaQuery.of(context).size.width > 360 ? FlatButton.icon(icon: Icon(Icons.delete)) : Text('Delete')
```

## Tip
Don't use too much `MediaQuery` in a widget instead use it as a variable. Define in at the start of your build method, simply add variable here.</br>
`final mediaQuery = MediaQuery.of(context);`

## Checking the device platform
```
// This will give switch UI according to android and IOS.
Switch.adaptive(
    value: _showChart,
    onChange: (val) {}
)
```
```
import 'dart:io';

floatingActionButton: Platform.isIOS ? Container() : FloatingActionButton();
```
```
final preferredSizeWidget appBar = Platform.isIOS;
// the type here is compulsory else error.
```
```
return Platform.isIOS ? CupertinoApp() : MaterialApp()
```
```
ThemeData(
    primarySwatch: Platform.isIOS ? Colors.red : Colors.purple
)
```

## Using the `SafeArea`
Some of the Available space on the screen is reserved and can't be used for positioning the widgets.