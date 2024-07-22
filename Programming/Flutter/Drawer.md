![[Pasted image 20230914225842.png]]
```dart
        drawer: Drawer(
          child: ListView(
            children: [
              DrawerHeader(
                decoration: BoxDecoration(color: Colors.redAccent[200]),
                child: Row(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    Icon(Icons.account_circle, size: 80, color: Colors.white),
                    Text(
                      'John Doe',
                      style: TextStyle(color: Colors.white, fontSize: 20),
                    ),
                    CircleAvatar(
                      backgroundImage: NetworkImage(
                          'https://encrypted-tbn0.gstatic.com/images?zc'),
                      radius: 40,
                    ),
                  ],
                ),
              ),
              ListTile(
                leading: Icon(
                  Icons.notifications,
                  color: Colors.black,
                ),
                title: Text(
                  'Notification',
                  style: TextStyle(color: Colors.black),
                ),
              ),
              ListTile(
                leading: Icon(
                  Icons.settings,
                  color: Colors.black,
                ),
                title: Text(
                  'Settings',
                  style: TextStyle(color: Colors.black),
                ),
              ),
              ListTile(
                leading: Icon(
                  Icons.person,
                  color: Colors.black,
                ),
                title: Text(
                  'My ID',
                  style: TextStyle(color: Colors.black),
                ),
                onTap: () => Navigator.of(context).push(
                  MaterialPageRoute(
                    builder: (BuildContext context) => ShowId(),
                  ),
                ),
              ),
            ],
          ),
        ),
```
