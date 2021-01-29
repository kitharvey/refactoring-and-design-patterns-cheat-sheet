# **Refactoring Cheat Sheet**
###### Stolen from  [Refactoring Guru](http://refactoring.guru)

<br/>
<br/>
<br/>
<br/>
<br/>

## **Composing Methods**
<br/>
<br/>

**Extract Method**  
Problem: You have a code fragment that can be grouped together.
```typescript
printOwing(): void {
  printBanner();

  // Print details.
  console.log("name: " + name);
  console.log("amount: " + getOutstanding());
}
```

Solution: Move this code to a separate new method (or function) and replace the old code with a call to the method.
```typescript
printOwing(): void {
  printBanner();
  printDetails(getOutstanding());
}

printDetails(outstanding: number): void {
  console.log("name: " + name);
  console.log("amount: " + outstanding);
}
```
<br/>
<br/>

**Inline Method**  
Problem: When a method body is more obvious than the method itself, use this technique.
```typescript
class PizzaDelivery {
  // ...
  getRating(): number {
    return moreThanFiveLateDeliveries() ? 2 : 1;
  }
  moreThanFiveLateDeliveries(): boolean {
    return numberOfLateDeliveries > 5;
  }
}
```

Solution: Replace calls to the method with the method’s content and delete the method itself.
```typescript
class PizzaDelivery {
  // ...
  getRating(): number {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
```
<br/>
<br/>

**Extract Variable**  
Problem: You have an expression that’s hard to understand.
```typescript
renderBanner(): void {
  if ((platform.toUpperCase().indexOf("MAC") > -1) &&
       (browser.toUpperCase().indexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

Solution: Place the result of the expression or its parts in separate variables that are self-explanatory.
```typescript
renderBanner(): void {
  const isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
  const isIE = browser.toUpperCase().indexOf("IE") > -1;
  const wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) {
    // do something
  }
}
```
<br/>
<br/>

**Inline Temp**  
Problem: You have a temporary variable that’s assigned the result of a simple expression and nothing more.
```typescript
hasDiscount(order: Order): boolean {
  let basePrice: number = order.basePrice();
  return basePrice > 1000;
}
```

Solution: Replace the references to the variable with the expression itself.
```typescript
hasDiscount(order: Order): boolean {
  return order.basePrice() > 1000;
}
```
<br/>
<br/>

**Replace Temp with Query**  
Problem: You place the result of an expression in a local variable for later use in your code.
```typescript
 calculateTotal(): number {
  let basePrice = quantity * itemPrice;
  if (basePrice > 1000) {
    return basePrice * 0.95;
  }
  else {
    return basePrice * 0.98;
  }
}
```

Solution: Move the entire expression to a separate method and return the result from it. Query the method instead of using a variable. Incorporate the new method in other methods, if necessary.
```typescript
calculateTotal(): number {
  if (basePrice() > 1000) {
    return basePrice() * 0.95;
  }
  else {
    return basePrice() * 0.98;
  }
}
basePrice(): number {
  return quantity * itemPrice;
}
```
<br/>
<br/>

**Split Temporary Variable**  
Problem: You have a local variable that’s used to store various intermediate values inside a method (except for cycle variables).
```typescript
let temp = 2 * (height + width);
console.log(temp);
temp = height * width;
console.log(temp);
```

Solution: Use different variables for different values. Each variable should be responsible for only one particular thing.
```typescript
const perimeter = 2 * (height + width);
console.log(perimeter);
const area = height * width;
console.log(area);
```
<br/>
<br/>

**Remove Assignments to Parameters**  
Problem: Some value is assigned to a parameter inside method’s body.
```typescript
discount(inputVal: number, quantity: number): number {
  if (quantity > 50) {
    inputVal -= 2;
  }
  // ...
}
```

Solution: Use a local variable instead of a parameter.
```typescript
discount(inputVal: number, quantity: number): number {
  let result = inputVal;
  if (quantity > 50) {
    result -= 2;
  }
  // ...
}
```
<br/>
<br/>

**Replace Method with Method Object**  
Problem: You have a long method in which the local variables are so intertwined that you can’t apply *Extract Method*.
```typescript
class Order {
  // ...
  price(): number {
    let primaryBasePrice;
    let secondaryBasePrice;
    let tertiaryBasePrice;
    // Perform long computation.
  }
}
```

Solution: Transform the method into a separate class so that the local variables become fields of the class. Then you can split the method into several methods within the same class.
```typescript
class Order {
  // ...
  price(): number {
    return new PriceCalculator(this).compute();
  }
}

class PriceCalculator {
  private _primaryBasePrice: number;
  private _secondaryBasePrice: number;
  private _tertiaryBasePrice: number;
  
  constructor(order: Order) {
    // Copy relevant information from the
    // order object.
  }
  
  compute(): number {
    // Perform long computation.
  }
}
```
<br/>
<br/>

**Substitute Algorithm**  
Problem: So you want to replace an existing algorithm with a new one?
```typescript
foundPerson(people: string[]): string{
  for (let person of people) {
    if (person.equals("Don")){
      return "Don";
    }
    if (person.equals("John")){
      return "John";
    }
    if (person.equals("Kent")){
      return "Kent";
    }
  }
  return "";
}
```

Solution: Replace the body of the method that implements the algorithm with a new algorithm.
```typescript
foundPerson(people: string[]): string{
  let candidates = ["Don", "John", "Kent"];
  for (let person of people) {
    if (candidates.includes(person)) {
      return person;
    }
  }
  return "";
}
```

