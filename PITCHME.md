---?image=assets/img/kotlin-logo2.png&size=cover

---
@snap[span-100]
### Agenda
@snapend

* History of Kotlin
* Why Kotlin?
* Code - feature walkthrough
* Summary

---?image=assets/img/kart1.png&size=65% auto

---?image=assets/img/kart2.png&size=65% auto

---?image=assets/img/kotlin-github.jpg&size=contain
---?image=assets/img/kotlin-digi.jpg&size=contain
---?image=assets/img/hvorfor-kotlin.jpg&size=contain

---?image=assets/img/most-loved.jpg&size=contain
@snap[south text-black span-100]
**Stack overflow survey 2018**
@snapend

---
@snap[span-100]
### Why Kotlin?
@snapend

* Interoperability with Java
  * Java -> Kotlin
  * Kotlin -> Java

---

* Less code to achieve the same functionality
  * *Rough estimates indicate approximately a 40% cut in the number of lines of code* (JetBrains)

---

* Useful features:
  * Null safety
  * Extension functions
  * Data classes

---

* Multi-paradigm
  * Object oriented
  * Functional
  * You decide what works best 4 U!

---

* Excellent tool support

---

*Kotlin is in itself a polyglot language. It brings together the powerful capabilities from many different languages. The creators of Kotlin took the good parts from various languages and combined them into one highly approachable and pragmatic language.*
- Venkat Subramaniam

---?image=assets/img/kotlin-usage.png&size=cover

---

![Show me the code](assets/img/talk-is-cheap-show-me-the-code.jpg)

---
@snap[span-100]
### Code
@snapend
---
@snap[span-100]
### Ranges
@snapend

---

```kotlin
// 1 up to including 10
for (number in 1..10) {
    println("Number: $number")
}

// Output:
// Number: 1
// Number: 2
// Number: 3
// Number: 4
// Number: 5
// Number: 6
// Number: 7
// Number: 8
// Number: 9
// Number: 10
```

---

```kotlin
// 1 to 10 (non-inclusive)
for (number in 1 until 10) {
    println("Number: $number")
}

// Output:
// Number: 1
// Number: 2
// Number: 3
// Number: 4
// Number: 5
// Number: 6
// Number: 7
// Number: 8
// Number: 9
```

---

```kotlin
for (number in 10 downTo 1 step 2) {
    println("Number: $number")
}

// Output:
// Number: 10
// Number: 8
// Number: 6
// Number: 4
// Number: 2
```

---
@snap[span-100]
### Classes
@snapend

---

```kotlin
// classes are final by default
class YouShallNotSubclass

// use open to make it subclassable
open class SubclassMe
```

---

* Many framworks including Spring needs open classes (plugins exist!)
* Let's live-code!


---
@snap[span-100]
### Clever casting - `is` and `as`
@snapend

```kotlin
// Smart casts
if (someObject is String) {
	// no need for casting! someObject can be treated as a String in this scope!
}

// Unsafe casting
val castedObject = someObject as String
val castedObject2 = someObject as? String ?: "default string"
```

---
@snap[span-100]
### Singleton pattern - Java vs Kotlin
@snapend

---?image=assets/img/singleton.jpg&size=auto 70%

---

#### Java
```java
public class Singleton {
    private static final Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null)
            instance = new Singleton();
        return instance;
    }
	
	// ... methods ...
}
```
---

#### Kotlin
```kotlin
object Singleton {
	// ... methods ... 
}
```

---
@snap[span-100]
### Companion object - closest to static you will come
@snapend

---?image=assets/img/companion-cube.jpg&size=cover
@snap[north]
#### Companion cube from Portal - kinda static
@snapend

---
* No static methods in Kotlin
* Functions doesn't cover your needs? Use companion objects

---

```kotlin
// companion object (or how to do "static" in Kotlin)
class ClassWithCompanion {
	// other methods that aren't "static"...?

    companion object {
        fun staticMethod() = "Im static..."
    }
}
```

---
@snap[span-100]
### DSL - domain specific languages
@snapend

* Several ways to create fluent DSLs in Kotlin
* They feel like they are part of the language if you do it correctly! :)

---

#### Function or Extension function with function parameter
```kotlin
fun String.applyToEachLetter(func : (char: Char) -> Char) : String {
	return this.toCharArray().map(func).joinToString("")
}


// example usage: 
"AsFf".applyToEachLetter {
	it.toLowerCase()
};
// returns "asff"
```

---

#### Functions evaluated in the context of a class
```kotlin
// using class bodies to create DSLs
class FunctionContext {
	fun highFive() = println("High five!")
}

fun highFiveBlock(body : FunctionContext.() -> Unit) {
	val functionContext = FunctionContext()
	functionContext.body()
}

// usage:
highFiveBlock {
	// just high-five 3 times for some reason...
	highFive()
	highFive()
	highFive()
}
```


---
@snap[span-100]
### Reified generics / inline functions
@snapend

---?image=assets/img/generics.jpg&size=auto 70%

---

* Now possible to get type info from generics at runtime!
* Inline functions => function code is actually put inline (in the middle of your code)

---

```kotlin
// usuaully you do not have access to class reference when  using generics (lost at runtime)
fun <T> noClassReference(someVal : T) = println("something")

// reified generics can do it! but they have to be 
inline fun <reified T> classReference(someVal : T) = T::class.java
```


---
@snap[span-100]
### Operator overloading
@snapend
 
* Like in C++, you can overload various operators (+, -, += etc.)
* Creating a 3D game? Simply multiply two vectors to get a cross-product!
 
---

#### Example of defining one yourself
```kotlin
class Vector(val x : Float, val y: Float, val z : Float) {
    // ... other method ...

    // cross product
    operator fun times(otherVec : Vector) : Vector {
        return Vector(y*otherVec.z - z*otherVec.y,
                      z*otherVec.x - x*otherVec.z,
                      x*otherVec.y - y*otherVec.x)
    }
}

// ---
// can multiply two vector objects and get a new vector back (the cross product)
val crossProd = Vector(0.0, 1.0, 0.0)*Vector(1.0, 0.0, 0.0)
```

---

#### Definition using extension function
```kotlin
operator fun Float.times(vector : Vector) : Vector {
    return Vector(this*vector.x, this*vector.y, this*vector.z)
}
```

---

#### Example in standard library - list add
```kotlin
val primes = mutableListOf(2, 3, 5)
primes += 7
// list now contains: 2, 3, 5, 7
```

---
@snap[span-100]
### Kotlin scripting
@snapend

```kotlin
#!/usr/bin/env kotlinc-jvm -script

import java.io.File

// slightly modified example from Venkats Programming Kotlin
// we always do file operations in scripts, so it is such a good example anyway :P 
File(".").walk().filter {
	it.extension.isNotEmpty()
}.groupingBy{
	it.extension
}.eachCount().forEach { (key, value) ->
    println("$key - $value")
}
```

---

* Syntax-check happens before run
* Use any Kotlin JVM functionality
* For more advanced features consider using KScript

---


@snap[span-100]
### Kotlin isn't simply a JVM language...
@snapend

* Compiles to...
   * JavaScript
   * WebAssembly
   * Android
   * KotlinNative (iOS, Android, native binaries etc.)

---
@snap[span-100]
### What we didn't cover
@snapend

[comment]: <> (Remove if we actually cover one of these)
* Sealed classes
* Pattern matching with `when`
* Init-blocks and multiple constructors
* Standard OOP concepts that are similar to Java (interfaces, abstract classes and how they are used)


---
@snap[span-100]
### Summary
@snapend
---

@snap[span-100]
### Kotlin is awesome!
@snapend

---
@snap[span-100]
### Kotlin resources
@snapend

![Kotlin in action](assets/img/kotlin-in-action.jpg))

---
@snap[span-100]
### Kotlin resources
@snapend

![Programming Kotlin](assets/img/programming-kotlin.jpg)

---
