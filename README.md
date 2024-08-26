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


