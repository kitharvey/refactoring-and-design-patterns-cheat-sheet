# **Refactoring Cheat Sheet**
###### Stolen from  [Refactoring Guru](http://refactoring.guru)

<br/>
<br/>
<br/>
<br/>
<br/>

## **Organizing Data**
<br/>
<br/>

**Self Encapsulate Field**  
Problem: You use direct access to private fields inside a class.
```typescript
class Range {
  private low: number
  private high: number;
  includes(arg: number): boolean {
    return arg >= low && arg <= high;
  }
}
```

Solution: Create a getter and setter for the field, and use only them for accessing the field.
```typescript
class Range {
  private low: number
  private high: number;
  includes(arg: number): boolean {
    return arg >= getLow() && arg <= getHigh();
  }
  getLow(): number {
    return low;
  }
  getHigh(): number {
    return high;
  }
}
```
<br/>
<br/>

**Replace Data Value with Object**  
Problem: A class (or group of classes) contains a data field. The field has its own behavior and associated data.   
![Replace Data Value with Object - Before](https://refactoring.guru/images/refactoring/diagrams/Replace%20Data%20Value%20with%20Object%20-%20Before.png)

Solution: Create a new class, place the old field and its behavior in the class, and store the object of the class in the original class.   
![Replace Data Value with Object - After](https://refactoring.guru/images/refactoring/diagrams/Replace%20Data%20Value%20with%20Object%20-%20After.png)
<br/>
<br/>

**Change Value to Reference**  
Problem: So you have many identical instances of a single class that you need to replace with a single object.      
![Change Value to Reference - Before](https://refactoring.guru/images/refactoring/diagrams/Change%20Value%20to%20Reference%20-%20Before.png)

Solution: Convert the identical objects to a single reference object.   
![Change Value to Reference - After](https://refactoring.guru/images/refactoring/diagrams/Change%20Value%20to%20Reference%20-%20After.png)
<br/>
<br/>

**Change Reference to Value**  
Problem: You have a reference object that’s too small and infrequently changed to justify managing its life cycle.      
![Change Reference to Value - Before](https://refactoring.guru/images/refactoring/diagrams/Change%20Reference%20to%20Value%20-%20Before.png)

Solution: Turn it into a value object.   
![Change Reference to Value - After](https://refactoring.guru/images/refactoring/diagrams/Change%20Reference%20to%20Value%20-%20After.png)
<br/>
<br/>

**Replace Array with Object**
This refactoring technique is a special case of Replace Data Value with Object.
Problem: You use direct access to private fields inside a class.
```typescript
let row = new Array(2);
row[0] = "Liverpool";
row[1] = "15";
```

Solution: Create a getter and setter for the field, and use only them for accessing the field.
```typescript
let row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```