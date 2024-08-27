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
Can be assigned once, but value is not known at compile time. final doesn't use memory until accessed.
example: final a = 86; // doesn't use memory.
print(a); // now will use memory.

`final a = 86; // Memory is used when accessed.`

### const:
A compile-time constant is a value that is fixed and immutable, known and determined at compile time. It is allocated memory as soon as it is declared and remains constant throughout the program.

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

- ### Accessing elements:
`var firstElement = list[0]; // Gets the first element`

- ### Adding elements:
`list.add(6); // Adds an element at the end`

- ### Inserting elements:
`list.insert(1, 10); // Inserts 10 at index 1`
- ### Removing elements:
`list.remove(2); // Removes the first occurrence of 2`

- ### Iterating:
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

- ### Adding/Updating Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
await prefs.setString('key', 'value');  // Save a string
await prefs.setInt('key', 123);         // Save an integer
```
- ### Retrieving Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
String? value = prefs.getString('key');  // Retrieve a string
int? number = prefs.getInt('key');       // Retrieve an integer
```
- ### Removing Data:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
await prefs.remove('key');  // Remove data associated with 'key'
```

- ### Checking for Existence:
```
SharedPreferences prefs = await SharedPreferences.getInstance();
bool containsKey = prefs.containsKey('key'); // Check if a key exists
```
- ### Example:
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

## 11) Dependency Injection:
Dependency Injection (DI) is a design pattern used in software development where a class does not create its own dependencies but instead receives them from an external source. This approach promotes modularity, testability, and flexibility in your code.

### Real-Life Analogy: The Coffee Shop

- **Without Dependency Injection**
  
You are the coffee maker (class) and must handle everything: grinding beans, boiling water, and brewing coffee (dependencies).
You are tightly coupled with the coffee-making process, meaning any change (like a new grinder) requires you to change your approach.

- **With Dependency Injection**
  
Imagine having an assistant (dependency injector) who prepares everything for you:
Provides pre-ground coffee and hot water (injected dependencies).
You focus solely on combining the ground coffee with hot water to make coffee.

### Key Takeaways

**Modular:** The coffee maker focuses on making coffee, while a class focuses on its main task, not on creating dependencies.

**Testable:** You can easily replace coffee beans or water (dependencies) without changing how the coffee maker works, just like swapping dependencies for testing in programming.

**Flexible:** If the assistant changes the coffee type, you can still make coffee without altering your method. In programming, you can change dependencies without modifying the core logic of the class.

This analogy illustrates how Dependency Injection simplifies management, testing, and adaptability in programming.

### Dependency Injection in Flutter with GetX:
When using GetX in Flutter, if you're using methods like Get.put(), Get.lazyPut(), or Get.find() to manage your controllers and services, you are applying Dependency Injection.

```
class RiderHomeScreenController extends GetxController {
    void sendOtp() {
        // Code to send OTP
    }

    void showSnackbar() {
        // Code to show snackbar
    }
}

// In your screen
final controller = Get.find<RiderHomeScreenController>();
controller.sendOtp();
```
In this example, RiderHomeScreenController manages the logic for sending OTP and showing a snackbar. The RiderHomeScreen class retrieves the controller using Get.find(), which demonstrates Dependency Injection because the screen does not create the controller but relies on it being provided.

### Get.lazyPut() and the fenix Property

- **Usage:** The Get.lazyPut() method allows you to register a dependency that will only be created when it’s needed.
- **fenix Property**: When set to true, if the controller is disposed of (e.g., when navigating away from a screen), it will be automatically recreated if needed again. If fenix is false, you need to manage the re-creation manually.

### Summary
**Dependencies:** These are the components (like services or controllers) that a class requires to function. In Dependency Injection, instead of the class creating its own dependencies, they are provided from an external source.

**Benefits:** This pattern makes code more modular, easier to test, and more flexible. It allows for better management of dependencies, particularly in larger applications. 

# Draft to fix:
```
Will popscope: Handles back button actions by popping routes off the navigation stack.

Get.offAll: Removes all previous routes and navigates to a new route.

Get.off: Removes only the top route in the stack and navigates to a new route.

SliverAppBar: A flexible app bar that can expand, contract, or remain pinned.

BuildContext: Provides access to widget's location in the widget tree and its inherited widgets.

List.generate vs ListView.builder vs ListView.separated:

List.generate: Creates a list with a fixed number of items.
ListView.builder: Lazily builds list items on demand.
ListView.separated: Builds list items with separators between them.
SizedBox vs Container:

SizedBox: Simple box with a fixed size, often used for spacing.
Container: More versatile, allows for decoration and constraints.
SizedBox vs Spacer:

SizedBox: Provides fixed spacing.
Spacer: Flexibly takes up remaining space in a row or column.
Stack: Allows widgets to overlap each other in a Z-order.

Types of Colors:

Hex code (e.g., #FF5733)
General color names (e.g., Colors.red)
Define a Color in a Class:

dart
Copy code
static const kPrimaryColor = Colors.red;
const keyword: Defines compile-time constants that cannot be changed.

const vs final:


Responsiveness other than ScreenUtil: Use MediaQuery to adapt layouts based on screen size and orientation.

Stateful Widget inside Stateless Widget: Instantiate a StatefulWidget within a StatelessWidget if needed.

GetX Controller: Manages state and business logic using GetX for efficient state management.

APIs: Interface for different software systems to communicate and perform operations.

OOP Pillars:

Encapsulation
Inheritance
Polymorphism
Abstraction
Data Structures: Organize and store data efficiently (e.g., Lists, Maps).

Expanded vs Flexible:

Expanded: Takes all available space in its axis.
Flexible: Takes only the space it needs.
initState: Method called when a StatefulWidget is inserted into the widget tree.

Immutable vs Mutable:

Immutable: Cannot be changed after creation.
Mutable: Can be modified after creation.
Multilingual App: Supports multiple languages by using localization files and internationalization.

APK vs IPA:

APK: Android package file.
IPA: iOS application file.
Stateful Widget Lifecycle: Methods include initState, build, didChangeDependencies, dispose.

Stateful vs Stateless:

Stateful: Can change its state over time.
Stateless: Immutable and cannot change its state.
Var vs Dynamic:

var: Type is inferred at compile time and cannot be changed.
dynamic: Type can change at runtime.
MVVM vs MVC:

MVVM: Model-View-ViewModel, separates UI from business logic.
MVC: Model-View-Controller, separates the application into models, views, and controllers.
DartPad Task:

dart
Copy code
void main() {
  List numbers = [1, 2, 3, 3, 2, 8, 4, 5, 4, 5];
  List uniqueNumbers = numbers.toSet().toList();
  uniqueNumbers.sort();
  print(uniqueNumbers);
}
Dio Package: A powerful HTTP client for making network requests.

SharedPreferences: Stores simple key-value pairs persistently.

Pagination: Loads data in chunks or pages to optimize performance and user experience.

Statemanagement: Keeps track of and updates the state of the application.

dispose(): Cleans up resources used by a StatefulWidget when it is removed from the widget tree.

Notifications: Alerts users about events or updates.

Animations: Adds visual movement or transitions to widgets.

Payment Integration: Incorporates payment methods and processing into the app.

Google Maps: Embeds and interacts with Google Maps in your app.

Firebase Authentication: Handles user sign-in and sign-up using Firebase.

Inkwell vs GestureDetector:

Inkwell: Provides material touch feedback.
GestureDetector: Detects gestures without visual feedback.
Global Keys: Allows access to a widget's state from anywhere in the widget tree.

Provider: A state management solution for providing data to widgets.

Get.put vs Get.lazyPut:

Get.put: Instantiates and injects a dependency immediately.
Get.lazyPut: Instantiates the dependency only when it's needed.
Abstract Class vs Static Class:

Abstract Class: Cannot be instantiated directly; meant to be subclassed.
Static Class: Not a Dart concept; typically refers to a class with only static members.
How to Explain Classes and Objects: Classes are blueprints for creating objects, which are instances of these blueprints.

setState() vs markNeedsBuild():

setState(): Notifies Flutter to rebuild the widget with updated state.
markNeedsBuild(): Internal method to mark a widget for rebuilding.
```
