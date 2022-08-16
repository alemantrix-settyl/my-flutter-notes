## Section 7: Navigation & Multiple Screens

- Instead of 'screen', you can also use 'page' or 'route' - these terms are commonly used.
```
GridView(
    children: <Widget>[],
    gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
        maxCrossAxisExtent: 200,
        // if device has 300px width, it shows 1 item only. If it has 500px width, it shows two item side by side. 
        childAspectRatio: 3/2,
        // it define how the items should be sized regarding their height and width relation.
        crossAxisSpacing: 20, // space between children
        mainAxisSpacing: 20, // space between children
    )
)
// Slivers are scrollable areas on the screen
// gridDelegate -> for structuring, layout of grid
// withMaxCrossAxisExtent -> It's preconfigured class allow to define a maximum width for each grid item. then it automatically creates as many columns as we can fit items with that provided width next to each other.
```

## Provide a default value to constructor if no value is provided.
```
Category({this.id, this.title, this.color = Colors.orange})
```
```
const [] // we get error, if items inside this list is not a const constructor.
```
```
padding: const EdgeInsets.all(15)
```
```
BoxDecoration(gradient: LinearGradient())
```
- `.withOpacity(0.7):` Method by flutter to make thing transparent.

```
decoration: BoxDecoration(
    gradient: LinearGradient(
        colors: [
            Color.withOpacity(0.7),
            Color
        ],
        begin: Alignment.topLeft,
        end: Alignment.bottomRight
    ),
    borderRadius: BorderRadius.circular(15)
)
```

`copyWith():` To replace default value with your own settings.

## Navigating to a new page
Navigator is a class which helps you with navigating between your screen. It need `of(context)` cause it need to know the current screen so it can add this page you want to go to as a screen on top of it.

### MaterialPageRoute() takes 4 arguments
- builder (It defines which widget should be build with the help of MaterialPageRoute)
- fullscreenDialog (a bool, if you want to have that default animation where the new page kind of slides in)
- maintainState
- settings

```
void selectCategory(BuildContext ctx) {
    Navigator.of(ctx).push(MaterialPageRoute(
        builder: (_) {
            return CategoryMealsScreen();
        }
    ))
}
```

- Top-most page is visible.

## Named Routes
```
routes: {
    '/category-meals': (ctx) => CategoryMealsScreen()
}
```
Problem: but this need an argument to pass but how to get the data from there? </br>
Solution: Remove constructor of CategoryMealsScreen() and...
```
void selectCategory(BuildContext ctx) {
    Navigator.of(ctx).pushNamed(
        '/category-meals',
        arguments: {
            'id': id,
            'title': title
        }
    )
}

// ModalRoute -> You can get info. about the route that was loaded to display this widget.

Widget build(BuildContext context) {
    final routeArgs = ModalRoute.of(context).settings.arguments as Map<String, String>;
    final categoryTitle = routeArgs['title'];
    final categoryId = routeArgs['id'];
}
```
## Diving deeper into Named Routes
- home always has an automatically named route which is just slash(/).
```
routes: {
    '/': (ctx) => CategoriesScreen()
}
```
- If '/' route automatically not showing then use `initialRoute: '/'`, set it.
## Static routes in class
```
class CategoryMealsScreen extends StatefulWidget{
    static const routeName = '/category-meals';
}

// In routes
routes: {
    CategoryMealsScreen.routeName: (ctx) => CategoryMealsScreen()
    // you don't need to instantiating class to use static variables. 
}
```
## Meal model
```
import "package:flutter/foundation.dart"; // to unlock @required

enum Complexity {
    simple, challenging, hard
}

class Meal {
    final String id;
    final List<String> categories;
    final String title;
    final Complexity complexity;
    final bool isVegan;

    Meal({
        @required this.id,
        @required this.categories,
        @required this.title,
        @required this.complexity,
        @required this.isVegan;
    })
}
```
- `enum:` It is human redable selections but stored as numbers.
---
## ClipRRect()
You can use any widget as a child and force it into a certain form.

```
Image.network(
    imageUrl, 
    height: 250,
    width: double.infinity,
    fit: BoxFit.cover // it nicely fits into the card and crop accordingly.
)
```

```
Text(
    softWrap: true, // text will wrap if length is long
    overflow: TextOverFlow.fade, // if text get overflowed then it get faded
)
```
```
Positioned(
    bottom: 20,
    right: 10,
    child: Text(...)
)
// It allows to position child widget in an absolute coordinate.
// bottom -> how far from bottom
```
- If you also want to make both items equally sized, you can wrap an Expanded() widget around your nested Row()

### onGenerateRoute & onUnknownRoute
```
onGenerateRoute: (settings) {
    return MaterialPageRoute(builder: (ctx) => CategoriesScreen());
}
// this function is executed if you're going to a named route, with pushNamed, that is not registered in the routes table.
// settings.name = for the name of the page.
```
```
onUnknownRoute: (settings) {
    return MaterialPageRoute(builder: (ctx) => CategoriesScreen())
}
// onUnknownRoute is reached when flutter failed to build a screen will all other measure when you define nothing in the routes table overall, last option, flutter will try to use this to show something on the screen.
// Its like your 404 Page.
```
---
`.firstWhere():` It gives one item for which the condition in this function return true.

## Divider()
Draws a horizontal line.

## TabBar
Two ways to add tab.

```
Widget build(BuildContext context) {
    return DefaultTabController(length: 2, 
    child: Scaffold(
        appBar: AppBar(title: Text('Meals'),
        bottom: TabBar(tabs: <Widget>[
            Tab(icon: Icon(Icons.category), text: Text("Category")),
            Tab(icon: Icon(Icons.category), text: Text("Category"))
        ])
        )
    )
    )
}

DefaultTabController is automatically connected to TabBar.

body: TabBarView(
    children: <Widget>[
        CategoryScreen(),
        FavoriteScreen()
    ]
)

// You don't have to provide pages on Scaffold cause TabBarView will manage it.

DefaultTabController(
    initialIndex: 0
)
// After the app opens, this(0) index tab will open first.
```
## Bottom TabBar
```
void _selectPage(int index){
    final List<Widget> _pages = [
        CategoriesScreen(),
        FavoritesScreen()
    ]

    int _selectPageIndex = 0;

    void _selectedPage(int index) {
        setState(() {
            _selectedPageIndex = index;
        })
    }
}

final List<Map<String, Object>> _pages = [
    {'page': CategoriesScreen(), 'title': 'Categories'},
    {'page': FavoriteScreen(), 'title': 'Your Favorites'},
]

Scaffold(
    appBar: AppBar(title: Text(_pages[_selectedPageIndex]['title']))
    body: _pages[_selectedPageIndex]['page']
    bottomNavigationBar: BottomNavigationBar(
        unSelectedItemColor: Colors.white,
        selectedItemColor: Theme.of(context).accent,
        currentIndex: _selectedPageIndex,
        type: BottomNavigationBarType.shifting,
        tap: _selectPage,
        backgroundColor: Theme.of(context).primaryColor,
        items: [
            BottomNavigationBarItem(
                icon: Icon(Icons.Category),
                title: Text("Categories")
            ),
            BottomNavigationBarItem(
                icon: Icon(Icons.Category),
                title: Text("Favorites")
            ),
        ]
    )
)
```

## Adding a custom Drawer 

```
Scaffold(
    drawer: Drawer()
)
```
`.backdrop:` It is that thing that grays out the background behind the drawer and allows you to add content to that drawer </br>

- Problem: We are adding pages on top of pages but this makes our app slow.
- Solution: `Navigator.of(context).pushReplacementNamed('/')` for named route.
- `Navigator.of(context).pushReplacement(...)` for non-named route.

## Popping pages & passing data back
- `Navigator.of(context).pop():` Its useful to remove a dialog or some other overlay from the screen in code.
- `popAndPushNamed():` It first remove then add new one.
- `canPop():` whenever there actually is a page to pop.
```
Navigator.of(context).pop(mealId);
// You can fetch it with the help of Future.

// use case
void selectMeal(BuildContext context) {
    Navigator.of(context).pushNamed(
        MealDetailScreen.routeName,
        arguments: id
    ).then((result) {
        print(result); // you get this after the pop of the page.
    })
}
```
`.removeWhere():` It removes items which fits certain criteria.

## God level problem, god level solution
- `initState()` throws error after putting ModalRoute.of(context) in it cause it ran before `build` and it didn't get the `context`.
- Solution: use `didChangeDependencies()` instead of `initState()`.

## didChangeDependencies() {}
It triggered whenever the references of the state change, also called when the widget that belongs to the state has been fully initialized and we can tap into context.

## SwitchListTile()
It allows you for switch that have text next to it. But we have to manage its internal state.

```
SwitchListTile(
    title: Text('Something'),
    value: _something,
    subtitle: Text("Something Something"),
    onChanged: (newValue){
        setState(() {
            _something = newValue;
        })
    }
)
```