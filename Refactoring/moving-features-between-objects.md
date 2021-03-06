# **Refactoring Cheat Sheet**
###### Stolen from  [Refactoring Guru](http://refactoring.guru)

<br/>
<br/>
<br/>
<br/>
<br/>

## **Moving Features between Objects**
<br/>
<br/>

**Move Method**  
Problem: A method is used more in another class than in its own class.   
![Move Method - Before](https://refactoring.guru/images/refactoring/diagrams/Move%20Method%20-%20Before.png)

Solution: Move this code to a separate new method (or function) and replace the old code with a call to the method.   
![[Move Method - After]](https://refactoring.guru/images/refactoring/diagrams/Move%20Method%20-%20After.png)
<br/>
<br/>

**Move Field**  
Problem: A field is used more in another class than in its own class.   
![[Move Field - Before]](https://refactoring.guru/images/refactoring/diagrams/Move%20Field%20-%20Before.png)

Solution: Create a field in a new class and redirect all users of the old field to it.   
![[Move Field - After]](https://refactoring.guru/images/refactoring/diagrams/Move%20Field%20-%20After.png)
<br/>
<br/>

**Extract Class**  
Problem: When one class does the work of two, awkwardness results.  
![Extract Class - Before](https://refactoring.guru/images/refactoring/diagrams/Extract%20Class%20-%20Before.png)

Solution: Instead, create a new class and place the fields and methods responsible for the relevant functionality in it.    
![Extract Class - After](https://refactoring.guru/images/refactoring/diagrams/Extract%20Class%20-%20After.png)
<br/>
<br/>

**Inline Class**  
Problem: A class does almost nothing and isn’t responsible for anything, and no additional responsibilities are planned for it.  
![Inline Class - Before](https://refactoring.guru/images/refactoring/diagrams/Inline%20Class%20-%20Before.png)

Solution: Move all features from the class to another one.    
![Inline Class - After](https://refactoring.guru/images/refactoring/diagrams/Inline%20Class%20-%20After.png)
<br/>
<br/>

**Hide Delegate**  
Problem: The client gets object B from a field or method of object А. Then the client calls a method of object B.  
![Hide Delegate - Before](https://refactoring.guru/images/refactoring/diagrams/Hide%20Delegate%20-%20Before.png)

Solution: Create a new method in class A that delegates the call to object B. Now the client doesn’t know about, or depend on, class B.    
![Hide Delegate - After](https://refactoring.guru/images/refactoring/diagrams/Hide%20Delegate%20-%20After.png)
<br/>
<br/>

**Remove Middle Man**  
Problem: A class has too many methods that simply delegate to other objects.  
![Remove Middle Man - Before](https://refactoring.guru/images/refactoring/diagrams/Remove%20Middle%20Man%20-%20Before.png)

Solution: Delete these methods and force the client to call the end methods directly.    
![Remove Middle Man - After](https://refactoring.guru/images/refactoring/diagrams/Remove%20Middle%20Man%20-%20After.png)
<br/>
<br/>

**Introduce Foreign Method**  
Problem: A utility class doesn’t contain the method that you need and you can’t add the method to the class.
```typescript
class Report {
  // ...
  sendReport(): void {
    let nextDay: Date = new Date(previousEnd.getYear(),
      previousEnd.getMonth(), previousEnd.getDate() + 1);
    // ...
  }
}
```

Solution: Add the method to a client class and pass an object of the utility class to it as an argument.
```typescript
class Report {
  // ...
  sendReport() {
    let newStart: Date = nextDay(previousEnd);
    // ...
  }
  private static nextDay(arg: Date): Date {
    return new Date(arg.getFullYear(), arg.getMonth(), arg.getDate() + 1);
  }
}
```
<br/>
<br/>

**Introduce Local Extension**  
Problem: A utility class doesn’t contain some methods that you need. But you can’t add these methods to the class.  
![Introduce Local Extension - Before](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Local%20Extension%20-%20Before.png)

Solution: Create a new class containing the methods and make it either the child or wrapper of the utility class.    
![Introduce Local Extension - After](https://refactoring.guru/images/refactoring/diagrams/Introduce%20Local%20Extension%20-%20After.png)