The Scaffold is a widget in Flutter used to implements the basic material **design visual layout structure**.
In other words, we can say that it is mainly responsible for creating a base to the app screen on which the child widgets hold on and render on the screen.

1. `appBar`: It is a horizontal bar that is mainly displayed at the top of the Scaffold widget. It is the main part of the Scaffold widget and displays at the top of the screen. Without this property, the Scaffold widget is incomplete.
```dart
Widget build(BuildContext context)   
{  
  return Scaffold(  
    appBar: AppBar(  
      title: const Text('First Flutter Application'),  
    ), ),  
}  
```

2. `body`: It is the other primary and required property of this widget, which will display the main content in the Scaffold. It signifies the place below the appBar and behind the floatingActionButton & drawer.
```dart
return Scaffold(   
    appBar: AppBar(   
    title: const Text('First Flutter Application'),   
    ),   
    body: const Center(   
    child: Text("Welcome to Javatpoint",   
        style: TextStyle( color: Colors.black, fontSize: 30.0,   
        ),   
         ),   
    ),  
} 
```

3. `bottomNavigationBar`: It is located at the bottom & has buttons like home, settings, etc.
```dart
bottomNavigationBar: BottomNavigationBar(
            items: const [
              BottomNavigationBarItem(
                  label: 'Home',
                  icon: Icon(Icons.home),
                  backgroundColor: Colors.black),
              BottomNavigationBarItem(
                label: 'Settings',
                icon: Icon(Icons.settings),
                backgroundColor: Colors.amber),
	          BottomNavigationBarItem(
                label: 'Audio Track',
                icon: Icon(
                    Icons.audiotrack,
                    color: Colors.green,
                    size: 30.0,
                  ),
                backgroundColor: Colors.amber),
            ],
          )
```

-  We can display only a small number of widgets in the bottom navigation that can be 2 to 5.
-  It must have *at least two bottom navigation items*. Otherwise, we will get an error.
-  It is required to have the icon and title properties, and we need to set relevant widgets for them.

4. `ElevatedButton`: Elevated Buttons cannot be styled i.e. you cannot modify the color of the button, font size, text style, etc explicitly like raised buttons.
```dart
body: Center(
            child: ElevatedButton(
              onPressed: () {
                print("Dhruv Rathee");     // This will print in the DEBUG CONSOLE
              },
              child: const Text('Click'),
            ),
          )
```



----
# Wrapper
##### Wrap with Column

```dart
body: Column(
		mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Center(
                child: ElevatedButton(
                  onPressed: () {
                    setState(() {
                      buttonName = 'Clicked!';
                    });
                  },
                  child: Text(buttonName),
                ),
              ),
            ],
          ),
```

To align them, use :-
```dart
mainAxisAlignment: MainAxisAlignment.center,
```
