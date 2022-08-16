## Section 4: Widgets, styling, adding logic - building a real app

`Flutter widget catalog:` API Reference -> get details of every widget.

`ListView` is Scrollable. 
### App/Page setup
- MaterialApp / CupertinoApp
- Scaffold / CupertinoPageScaffold

### Layout
- Container
- Row
- Column

### Row / Column Children
- Flexible
- Expanded

### Content Container
- Stack
- Card
### Repeat Elements
- ListView
- GridView
- ListTile
### Content Types
- Text
- Image
- Icon
### User Input
- TextField
- RaisedButton / FlatButton
- GestureDetector
- InkWell
---
- use `ListView.builder()` to get optimized item rendering for very long lists. 
- `map` always gives an iterable so you have to transform it into a list so use `.toList()` at last of map.
```
Container(
        margin: EdgeInsets.symmetric(vertical: 10, horizontal: 15),
        decoration: BoxDecoration(
          border: Border.all(color: Colors.black, width: 2),
        ),
        padding: EdgeInsets.all(10),
        child: Text('Something'),
      ),
```
### String Interpolation
```
Text('\$${tx.amount}'); // \$ to use special character normally
```
---
- `intl` package to format the date. This deals with internationalized/ localized messages, date, number formatting.
- If the package would not get installed automatically, you can run `flutter packages get` in your terminal (in the project folder).

```
DateFormat().format(tx.date);
DateFormat('yyyy-MM-dd').format(tx.date); // for date
DateFormat.yMMMd().format(tx.date);
```
`stateful:` change data internally as well as in UI
`stateless:` change data internally but not in UI
---
`Text()` for output text
`TextField()` for input text

### Fetching user input
- `onChanged:` it fires with every keystokes
- `onChanged: (val) {}:` val is automatically a string by dart.
```
onChanged: (val) {
    titleInput = val;
}
// that means we overrite the current value of title input with the input the user change here.
```
```
final titleController = TextEditingController();

TextField(
    controller: titleController
)
// Flutter automatically connects the controllers with our textfield and these controllers in the end listen to the user input and save it.
```
---
Problem: Wanna pass `double` in function but controller is returning string. </br>
Solution: double.parse(amountController.text)

### Making List Scrollable
// Wrap your column in `SingleChildScrollView` and it add scrolling functionality but it won't be able to identity the containers height in which it should scroll.

```
Container(
        height: 30,
        child: SingleChildScrollView(
          child: Column(),
        ),
      )
// it will scroll within a container
// problem: opening keyboard giving yellow lines
SOLUTION
SingleChildScrollView(
    child: Column(...)
)
```
### Working with ListViews
He set column to ListView but It gives an error -> Vertical viewport was given unbounded height.</br>
=> it can't have a fixed height, its infinite. You need to give an constraint or height.</br>
sol-> put it in Container with fixed height.
```
Container(
    height: 300,
    child: ListView()
)
```

### ListView
```
ListView.builder(
    itemBuilder: (context, index) {},
    itemCount: transaction.length
)
```

### Input & Output styling and configuration
`TextField(keyboardType: TextInputType.select)`

```
onSubmitted: (val) => submitData
// submitData is f(x)

OR

onSubmitted: (_) => submitData
// _ means I accept an argument but I don't care about it. I don't plan to use it.
```
- Don't provide return in `if` or `else`, cause it stops the execution of the function that is next to it.
- if function don't want to execute and data is not void then use `return`.
```
if (enteretedTitle.isEmpty || enteredAmount <= 0){
    return;
}
// it will throw control out of the current block
```
```
onSubmitted: (_) => submitData()
// Now it will execute because this anonymous function is passed as a reference to onSubmitted, when this anonymous function executes, we have to manually trigger our function but on onPressed we don't wrap in anonymous function we just pass reference to our function.
```

```
Text('\$${trasaction[index].amount.toStringAsFixed(2)}')
// This will give amount to only with two decimal places.

AppBar(actions: <widgets>[IconButton()])

Scaffold(floatingActionButton: FloatingActionButton(
    floatingActionButtonLocation: FloatingActionButtonLocation.centerFloat
), )
```

### Showing a modal button sheet
```
void _startAddNewTranslation(BuildContext ctx) {
    showModalBottomSheet(context: ctx, builder: (_) => GestureDetector(
      onTap: () {},
      behavior: HitTestBehavior.opaque,
      child: Container(),
    ));
}
// behavior: with this setting you can catch the tap event and avoid that it's handled by anyone else and this avoids that the sheet closes when you tap on the sheet itself.
```
### Problem
Stateful widget makes two class, how do we access its class's method and property? </br>
Solution: `widget.<method-of-class-in-state-object>`

`Navigator.of(context).pop()`

### Configuring & Using Themes

Why you change your color, different places why not use it globally, just change it one place. That's possible by using theme.
```
MaterialApp(
    theme: ThemeData(<A-lot-of-configuration>)
)
```
`primary color:` is one single color like blue, red.</br>
`primary swatch:` is based on one color but it automatically generates different shades of that color automatically.
`of(context):` here's my context object and context is something, It get passed to the build method automatically by flutter.

```
color: Theme.of(context).primaryColor
// use it in different parts of the app
```
A `accentColor` is an alternative color. There is no accentSwatch, so you can only set the accentColor! </br>

### Custom Fonts & Working with Text Themes
```
// pubspec.yaml
 fonts:
     - family: Schyler
       fonts:
         - asset: fonts/Schyler-Regular.ttf
         - asset: fonts/Schyler-Italic.ttf
           style: italic

// In dart file
ThemeData(
    fontFamily: 'Schyler'
)
```
### Adding Images to the app
```
Image.asset('assets/image/wow.png', fit: BoxFit.cover)

// if image is not according to screen then it will fit.
// But in this case the image is inside the column which has no size, that's why it won't work. 
Solution: So put Image inside the container.
```
Soluction: `SizedBox()`

### Looping through lists

`List.generate(length, generator)` </br>
// This constructor generates us new list of values

```
List.generate(4, (index)=> SomeWidget(property: index))
```
## `.where()` 
It allows you to run a function on every argument in a list and if that function returns true, the item is kept in a newly returned list otherwise its not included in that newly returned list. 
 
## FractionallySizedBox()
This allows me to create a box that is sized as a fraction of another value.</br>
height factor (0 - 1)

`.subString(0, 1)` works on every string, cut element from 0 to 1. </br>
`.toStringAsFixed(0)` no decimal places.

## Stack
First widget on stack will be the bottom most widget.
```
Stack(
      children: <Widget>[
        Container(
          child: Text('Bottom most widget'),
        ),
        FractionallySizedBox(
          child: Text('Top Most widget'),
        )
      ],
    )
```

## (data['amount'] as double) - to access double methods and properties.

---
### Flexible()
It takes a fit argument and these, you pass a Flexfit value to enum where you have two values. </br>
- with `loose`, we already restrict how much space this may take a bit. 
- with `tight`, we force a child into its assigned width or into its assigned size.
- also can add flex property.

```
Flexible(
    fit: FlexFit.loose,
    child: Container()
)
// loose: it means that the child of that flexible item basically should keep its size and use that size in the surrounding row or column.

// FlexFit.tight -> it only takes as much space as fits in there without squeezing any items of the screen.
```

### FittedBox()
This widget forces its child into the available space and if that child is a text, it even shrinks the text. So, it creates a widget that scales and positions its child within itself.

---
List.reversed.toList()

### Showing a DatePicker
```
void _presentDatePicker () {
  showDatePicker (
    context: context,
    initialDate: DateTime.now(),
    firstDate: DateTime(2019),
    lastDate: DateTime.now()
  )
}
```
Future is a class built in dart not in flutter. we creates object with future class and it give us a value in future. The cool thing about future is that the function we pass in here is stored in memory and the other code will still execute immediately and not wait for this future to resolve.
```
showDatePicker(...).then((pickedDate) {
  if (pickedDate == null){
    return;
  }
  setState(() {
    _selectedDate = pickedDate;
  })
})
```