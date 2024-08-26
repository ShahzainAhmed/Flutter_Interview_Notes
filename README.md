## Flutter Interview Preperation Notes

## 1) var vs dynamic 
### var: 
The type is inferred at initialization and cannot be changed later (type cannot be changed)
```
var myName = "Shahzain";
// myName = 86; // Error: Cannot assign int to a String variable.
myName = "Shahzain Ahmed"; // Works fine as it's the same type.
```

### dynamic: 
The type can change at runtime. (type can be changed)
```
dynamic myName = "Shahzain";
myName = 86; // Works as dynamic allows type changes.
```
## 2) final vs const 
### final: 
final doesn't use memory until access.
example: final a = 86; // doesn't use memory.
print(a); // now will use memory.

`final a = 86; // Memory is used when accessed.`

### const:
Compile-time constant that is immutable and always uses memory from the start of declaration.

`const a = 86; // Uses memory at declaration.`

### Instance variables: 
Variables declared inside a class can be final, but not const. final can be used, but const needs to be static for class-level constants. 
```
class MyClass {
  final m = 86;
  static const f = 10; // `const` must be static here.
}
```
### Example of final vs const 
```
String myName = "Shahzain";
final name = myName; // Works fine.
const newName = "Shahzain Ahmed"; // Must be a compile-time constant.
```

## 3) Expanded vs Flexible 
### Expanded: 
Fills all available space along the main axis and ignores the child’s height. <br>
### Flexible: 
Takes up only as much space as its child needs, with the option to define the flex fit:
- **FlexFit.tight:** Occupies all available space.
- **FlexFit.loose:** Takes only as much space as required.

## 4) Builder and BuildContext
In Flutter, BuildContext provides access to the location of a widget in the widget tree and allows interaction with its parent widgets. Sometimes, you might need a new BuildContext to interact with widgets that are further up the tree.

For instance, if you need to use Scaffold.of(context) to access the Scaffold widget, it might fail if the context you're using is not within the scope of the Scaffold. In such cases, you can use the Builder widget to create a new BuildContext that is correctly scoped within the Scaffold.

Here's how to use Builder:

In this example, Builder creates a new context (newContext) that can properly access the Scaffold above it. This way, you avoid issues where Scaffold.of(context) might fail due to the context being out of scope.
```
Builder(
  builder: (BuildContext newContext) {
    // Use `newContext` here to access widgets higher up in the widget tree.
    return Scaffold(
      appBar: AppBar(
        title: Text('Title'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Scaffold.of(newContext).showSnackBar(
              SnackBar(content: Text('Hello from Builder!')),
            );
          },
          child: Text('Show SnackBar'),
        ),
      ),
    );
  },
)
```

## 5) List and List Operations 
### Reversing:
```
void main() {
  List numbers = [1, 2, 3, 3, 2, 8, 4, 5, 4, 5];
  List reversedNumbers = numbers.reversed.toList();
  print(reversedNumbers); // Output: [5, 4, 5, 4, 8, 2, 3, 3, 2, 1]
}
```

### Sorting:
```
void main() {
  List numbers = [1, 2, 3, 3, 2, 8, 4, 5, 4, 5];
  numbers.sort();
  print(numbers); // Output: [1, 2, 2, 3, 3, 4, 4, 5, 5, 8]
}
```

### Removing Duplicates:
```
void main() {
  List numbers = [1, 2, 3, 3, 2, 8, 4, 5, 4, 5];
  List uniqueNumbers = numbers.toSet().toList();
  print(uniqueNumbers); // Output: [1, 2, 3, 8, 4, 5]
}
```

## 6) Lists vs Maps 

A List in Dart is an ordered collection of elements that can be of any type. It allows you to store multiple items in a single variable and access them by their index. Lists are zero-based, meaning the first element is at index 0.

### Syntax:
**Creating an empty list:**
```
var list = [];

or with explicit type:

List<int> list = [];
```

**Creating a list with initial values:**
```
var list = [1, 2, 3, 4, 5];

or with explicit type:

List<String> list = ['apple', 'banana', 'cherry'];
```
## Common Operations in List:

### Accessing elements:
`var firstElement = list[0]; // Gets the first element`

### Adding elements:
`list.add(6); // Adds an element at the end`

### Inserting elements:
`list.insert(1, 10); // Inserts 10 at index 1`
### Removing elements:
`list.remove(2); // Removes the first occurrence of 2`

### Iterating:
```
for (var item in list) {
  print(item);
}
```

### Use Cases
- Storing ordered data, such as a list of items in a shopping cart.
- Iterating through elements when order matters.

## Maps in Dart
A Map in Dart is a collection of key-value pairs. Each key in a map is unique, and each key maps to exactly one value. Maps are also known as dictionaries or hash tables in other languages.

### Syntax:

**Creating an empty map:**
```
var map = <String, dynamic>{};
or with type inference:
var map = {};
```

**Creating a map with initial values:**
```
var map = {
  'name': 'John',
  'age': 30,
  'city': 'New York'
};
```

## Common Operations in Maps:

### Accessing values:
`var name = map['name']; // Gets the value for the key 'name'`

### Adding/Updating entries:
map['occupation'] = 'Developer'; // Adds or updates the 'occupation' key

### Removing entries:
`map.remove('city'); // Removes the entry with key 'city'`

### Iterating:
```
map.forEach((key, value) {
  print('$key: $value');
});
```
### Use Cases:
- Storing data where you need to quickly look up values based on unique keys.
- Representing configurations, settings, or any data where each value is associated with a specific key.

## Summary
## Lists
- **Definition:** Ordered collection of elements.
- **Access:** By index.
- **Common Use:** Storing sequences of items.
  
Lists in Dart are ordered collections of elements that can be accessed by their index. You can create a list with initial values or start with an empty list and add elements later. Lists are useful when you need to maintain an ordered sequence of items, such as user input or a collection of records.

## Maps
- **Definition:** Collection of key-value pairs.
- **Access:** By key.
- **Common Use:** Associating values with unique keys for quick lookups.

Maps in Dart store data as key-value pairs where each key is unique and maps to a specific value. You can create a map with predefined keys and values or start with an empty map and add entries as needed. Maps are ideal for situations where you need to retrieve data based on a unique identifier, such as user settings or configuration options.

## 7) FutureBuilder vs StreamBuilder
### FutureBuilder
- **Purpose:** Used for one-time asynchronous operations.

- **Common Uses:** Ideal for tasks such as making HTTP requests, fetching data from a database, or performing one-off operations like capturing an image or reading device battery levels.

- **Behavior:** Handles a single result that is returned once. You can listen to the state of the future, which can be either:
  - waiting: The future is still being processed.
  - done: The future has completed with either success or an error.
    
  ```
  FutureBuilder(
    future: fetchData(), // Function that returns a Future
    builder: (BuildContext context, AsyncSnapshot snapshot) {
      if (snapshot.connectionState == ConnectionState.waiting) {
        return CircularProgressIndicator(); // Loading indicator
      } else if (snapshot.hasError) {
        return Text('Error: ${snapshot.error}');
      } else {
        return Text('Data: ${snapshot.data}');
      }
    },
  );
  ```
### StreamBuilder
- **Purpose:** Used for continuous data streams that can update over time.

- **Common Uses:** Suitable for handling ongoing updates like real-time location tracking, live chat messages, or data that frequently changes.

- **Behavior:** Listens to a stream of data, updating the UI as new data is emitted. It can react to multiple data points over time

```
StreamBuilder(
  stream: dataStream(), // Function that returns a Stream
  builder: (BuildContext context, AsyncSnapshot snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator(); // Loading indicator
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text('Data: ${snapshot.data}');
    }
  },
);
```
**Choosing Between Them:**

Use FutureBuilder when you need to handle a single, one-time asynchronous result.

Use StreamBuilder when you need to handle ongoing or continuous data updates

## 8) Error Handling:
Use try-catch blocks to handle exceptions:
```
try {
  // Code that might throw an exception
} catch (e) {
  // Handle the exception
}
```

## 9) REST APIs:

### GET API:
```
 static getApi() async {
    final uri = Uri.parse("https://reqres.in/api/users");
    var response = await http.get(uri);

    if (response.statusCode == 200) {
      print('Response data: ${response.body}');
    } else {
      print('Failed to load data. Status code: ${response.statusCode}');
    }
  }
```
### POST API:
```
static postApi() async {
    final uri = Uri.parse("https://reqres.in/api/register");
    final response = await http.post(
      uri,
      body: {
        "email": "eve.holt@reqres.in",
        "password": "pistol",
      },
    );

    if (response.statusCode == 200) {
      print('Response postApi(): ${response.body}');
    } else {
      print('Failed to load data. Status code: ${response.statusCode}');
    }
  }
```

## 10) SharedPreferences:
SharedPreferences is a key-value storage system used for storing simple data types persistently across app sessions. It's commonly used to save user preferences, settings, or simple data like login information.

- **Purpose:** To persistently store data that needs to be accessed across app restarts.

- **Common Uses:** Saving user settings, app preferences, small amounts of data, and flags.

### Basic Operations:

### Adding/Updating Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
await prefs.setString('key', 'value');  // Save a string
await prefs.setInt('key', 123);         // Save an integer
```
### Retrieving Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
String? value = prefs.getString('key');  // Retrieve a string
int? number = prefs.getInt('key');       // Retrieve an integer
```
### Removing Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
await prefs.remove('key');  // Remove data associated with 'key'
```

### Checking for Existence:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
bool containsKey = prefs.containsKey('key'); // Check if a key exists
```
### Example:
```
void saveUserPreference() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  await prefs.setBool('isLoggedIn', true); // Save a boolean value
}

void loadUserPreference() async {
  SharedPreferences prefs = await SharedPreferences.getInstance();
  bool isLoggedIn = prefs.getBool('isLoggedIn') ?? false; // Default to false if not found
}
```

**Note:** SharedPreferences is suitable for small amounts of data. For larger data sets or more complex data structures, consider using other storage solutions like databases.

