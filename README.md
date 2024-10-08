# RxJS = Reactive Extensions for JavaScript
### What is a programming paradigm?
A programming paradigm is a relatively high-level way to conceptualize and structure the implementation of a computer program.   
https://en.wikipedia.org/wiki/Programming_paradigm  
Types of programming paradigm:  
![image](https://github.com/user-attachments/assets/2230b708-cc7e-412b-ac5c-48b452619e16)
### Imperative vs declarative programming
> Imperative programming tells us **how to do it**, while Declarative programming tells us **what we want to happen**.
 
Imperative Programming provides flexibility but brings in complexity, Declarative programming hides complexity and provides simplicity.  
In imperative you're dictating instructions, and in declarative the system knows enough to figure out instructions on its own. In an imperative approach, you're working with a blank slate.  
![images](https://github.com/user-attachments/assets/f4cc207a-1727-40a0-ba16-d5cc29991038)  
![image](https://github.com/user-attachments/assets/7b339e65-15bd-403a-b0b8-8e3bc58cdbcd)  

**Examples:**  
- Imperative languages: C, C++, Java and Fortran.
- Declarative languages: HTML, SQL, CSS and XML, and mix of functional and logic programming languages, such as Prolog, Haskell, and Lisp.
### What is declarative programming?
Declarative programming is a programming paradigm, a style of building the structure and elements of computer programs that expresses the *logic of a computation without describing its control flow*.  
Declarative programming is an abstraction of functions that underlying it is an imperative implementation.

### What is reactive programming?
> Reactive programming is programming with **asynchronous data streams.**

Reactive programming is a programming paradigm, or model, that centers around the concept of **reacting to changes in data and events**.  
In computing, reactive programming is a declarative programming paradigm concerned with **data streams and the propagation of change**.   
It relies on asynchronous programming logic.  

-----

### What is RxJS?
RxJS is a powerful library for web development. It's focused on managing asynchronous data streams and enabling reactive programming paradigms.
### Why to use it?  
If anything in your app happens asynchronously, an RxJS Observable will **make your life easier.**   

---
### Concepts of RxJS:
- #### Observable
 > **emits a values or events**
  
 represents the idea of an invokable collection of future values or events.
```
import { Observable } from 'rxjs';
 
const observable = new Observable((subscriber) => {
  subscriber.next(1);
  setTimeout(() => {
    subscriber.next(4);
    subscriber.complete();
  }, 1000);
});
```
```
observable.subscribe({
  next(x) {
    console.log('got value ' + x);
  },
  error(err) {
    console.error('something wrong occurred: ' + err);
  },
  complete() {
    console.log('done');
  },
});
```
**Observables vs functions:**  
Functions **can only return one value**. Observables can return multiple:  
![image](https://github.com/user-attachments/assets/e0f52222-a891-431a-8430-3a9b5e520924)  
![image](https://github.com/user-attachments/assets/d32fae4d-4313-4fc6-8dfe-1e8806dc2f57)  
  
**Observables vs promises:**  
https://stackoverflow.com/questions/37364973/what-is-the-difference-between-promises-and-observables  

A Promise handles a **single** event when an async operation completes or fails.  
An Observable is like a Stream (in many languages) and allows you to pass **zero or more** events where the callback is called for each event.  
Observable is easily **cancellable**.  
While a **Promise starts immediately**, an **Observable** only starts **if you subscribe** to it. This is why Observables are called lazy.  
![image](https://github.com/user-attachments/assets/cd502520-de6e-45d2-9cc2-3f2b9959acbd)


- #### Observer
 > **subscribes to observable**

 is a consumer of values delivered by an Observable.  
 ![image](https://github.com/user-attachments/assets/0592e133-a9c0-4691-8961-ddc3a008098d)


- #### Subscription
   > **saved instance of observer which you can use to unsubscribe**
   
 represents the execution of an Observable, is primarily useful for cancelling (unsubscribing) the execution.  
```
const subscription = observable.subscribe(x => console.log(x));
subscription.unsubscribe();
```


- #### Subject
  > **Observable that can have multiple subscribers**
   
 is equivalent to an EventEmitter, and the only way of multicasting a value or event to multiple Observers.  
 Every Subject is an Observable.  
 > If you try to subscribe > 1 times to the same Observable, **only the last one will get values!** That's the reason why we need Subjects. 
 https://stackoverflow.com/questions/36814995/multiple-subscriptions-to-observable


 ```
import { Subject } from 'rxjs';
const subject = new Subject<number>();
 
subject.subscribe({
  next: (v) => console.log(`observerA: ${v}`),
});
subject.subscribe({
  next: (v) => console.log(`observerB: ${v}`),
});
 
subject.next(1);
// observerA: 1
// observerB: 1
```
**Types of subjects:**
![image](https://github.com/user-attachments/assets/aa2bbad6-2b38-4bf6-87b0-64cbdee1ae61)  

1. **BehaviorSubject** - new subscribers receive last emitted value  
 ```const subject = new BehaviorSubject(0); // 0 is the initial value```  
3. **ReplaySubject** - new subscribers receive last N emitted values  
    ```const subject = new ReplaySubject(3); // buffer 3 values for new subscribers```  
5. **AsyncSubject** - all (old + new) subscribers receive last emitted value **when** AsyncSubject ***completes***  
    ```const subject = new AsyncSubject(); ```

- #### Schedulers
 are centralized dispatchers to control concurrency, allowing us to coordinate when computation happens on e.g. setTimeout or requestAnimationFrame or others.


- #### Operators
 are pure functions that enable a functional programming style of dealing with collections with operations like map, filter, concat, reduce, etc.  

### pipe()
.pipe() method is chained on an Observable to apply pipeable operators and return a new observable
```
of(1, 2, 3).pipe(map(num => num * 10)).subscribe(value => {
   console.log(value);
});
```

Two kinds of operators:
1. **Pipeable** Operators - is a function that takes an Observable as its input and returns another Observable. It is a **pure operation**: the **previous Observable stays unmodified**.
 ```
mediObservable.pipe(someOperator)
```
Ex. when operator is *filter*:  
```
mediObservable.pipe(filter(ev => (ev.id == id));
```
2. **Creation** Operators - can be called as standalone functions to create a new Observable.
   ```
    of(1, 2, 3) // creates an observable that will emit 1, 2, and 3
   ```
   
**Marble diagrams**  
  
Marble Diagrams are visual representations of ***how operators work***, and include the:  
a) **input** Observable(s),   
b) the **operator** and its parameters, and   
c) the **output** Observable 
> In a marble diagram, time flows to the right, and the diagram describes how **values ("marbles")** are emitted on the Observable execution.

![image](https://github.com/user-attachments/assets/da92ccb3-a548-42f6-90e2-4233c1ed3d3b)  


### **Categories of operators**:
#### ***Creation***
- **EMPTY** - A simple Observable that emits no items to the Observer and immediately emits a complete notification. 
```
EMPTY.subscribe({
next: () => console.log('Next'), // this will NOT be shown
complete: () => console.log('Complete!') // this will be shown
});
```


- **of** - Converts the arguments to an observable sequence.  
```
 of(10, 20, 30).subscribe(
{next: value => console.log(value)}
)
```  
- **defer** - Observable emits only when some Observer subscribes
```
const mediDeferObs = defer(() => {
  return new Date(); //will capture date time at the moment of subscription
});
mediDeferObs.subscribe(x => console.log(x)); // it will show date time when this subscription happened  
```
- **interval** - Observable keeps emitting a value, after every 'interval of time' has passed (it's like setInterval in JS = setInterval(code, delay))
The emitted value is *auto incrementing*.  
```
interval(1000).subscribe(x => {
    console.log(x); // we will see a number written to the console every second, starting at 0 and incrementing from there
});
```
- **timer** - delays emitting for a certain interval of time  
 ``` timer(dueTime, interval)```  
  dueTime = time in milliseconds to wait before emit  
  interval = if not provided, it will emit a 0 value and complete immediately, if provided, then it's the same as interval, it will emit a incremented value every interval time (auto incrementing)
```
 timer(1000, 2000).subscribe(val => console.log(val)); // it will print 0, then after every 2 seconds,
//it will increment the previous value (0, 1, 2...)

 timer(1000).subscribe(val => console.log(val)); // it will print 0, and complete

// these two are same, they never complete
timer(0, 1000).subscribe(n => console.log('timer', n)); // 0, 1, 2...
interval(1000).subscribe(n => console.log('interval', n)); // 0, 1, 2...
```  
- **from** - Creates an Observable from an **Array, an array-like object, a Promise, an iterable object, or an Observable-like object.**  
> *of vs from*: of always accepts **only values** and performs no conversion. You can't forward Promise or Observable objects to *of*.  
 ```
const result = from([10, 20, 30]);
result.subscribe(x => console.log(x));

// These 2 are the same (second is getting array as argument, first is not)
Observable.of(1,2,3).subscribe(() => {})
Observable.from([1,2,3]).subscribe(() => {})

// These 2 are not the same, passing array as argument to both
Observable.of([1,2,3]).subscribe(() => {}) // emits whole array at once ([1,2,3])
Observable.from([1,2,3]).subscribe(() => {}) // emits 1 by 1 value (1 2 3)
```
- **fromEvent** - creates observable from event  
 ```
const clicks = fromEvent(document, 'click');
clicks.subscribe(x => console.log(x));
```  
- **throwError** - Just errors and does nothing else.   
> A stream can error out only once. The **stream will not emit** any further values **after an error is thrown**.  
 ```
 return throwError(() => new Error(`Invalid time`));
// or
 throw new Error(`Invalid time}`);

// it will be executed in the error part if implemented
.subscribe({
  next: console.log,
  error: console.error // if you have error implemented, you can do the logic you want, and that will be executed
// if you don't have error implemented, it will just be automatically thrown in console.log
});

```
Creating my own observable (will emit 1, 2, and error):  
```
const observable = new Observable((subscriber) => {
      subscriber.next(1);
      subscriber.next(2);
  throw new Error(`Invalid time}`);
});
// 1st implementation
    observable.subscribe(x => {
      console.log(x) // here we don't implement error, and by default it will be logged in console log, even if we don't have this line console.log
    })
// 2nd implementation
    observable.subscribe({
      next: console.log,
      error: console.error // here we implemented error, it will console error 
    })
// 3rd implementation
  observable.subscribe({
    next(x) {
      console.log('got value ' + x);
    },
    error(err) {
      console.error('something wrong occurred: ' + err); //  here we implemented error, we could add logic, whatever we want, this will shown error in the way we implemented it
    },
    complete() {
      console.log('done');
    },
  });
```

#### ***Join Creation***  
- **forkJoin** - wait for multiple observables completion, and use last emitted values as result  

If we send **object**, the result is object. We can name however we want (the properties of the object)

```
const observable = forkJoin({
  medi1: of(1, 2, 3, 4),
  medi2: Promise.resolve(8),
  daci: timer(4000)
});
observable.subscribe({
 next: value => console.log(value),
 complete: () => console.log('This is how it ends!'),
});
 
// { medi1: 4, medi2: 8, daci: 0 } after 4 seconds, object
// 'This is how it ends!' immediately after
```
If we send **array**, result is array:  
```
const observable = forkJoin([
  of(1, 2, 3, 4),
  Promise.resolve(8),
  timer(4000)
]);
// [4, 8, 0] array
```
- **concat** - emits all values from first observable until it completes, then from the second same  

```
concat(of(0,1), of(3,4)).subscribe(x => console.log(x))
// 0,1,3,4
```
- **merge** - running multiple observables at the same time, creates new observable that merge them  
It is similar like concat, but concat waits for the first one to complete, while merge emits as obs emits, and does not wait for one to be completed, it fires emissions as they come. 
```
merge(of(1), of(3, 4)).subscribe(x => console.log(x))
// 1, 3, 4
```
#### ***Transformation***  
- **map** - it's like map in js, iterates trough values of observable, and returns new observable  
In JS ```map vs forEach```:  
  ```map``` - returns new list, and does not mutate original one  
  ```forEach``` - mutates original list

```
of(1,2,3).pipe(map(x => x*2))
.subscribe(x => console.log(x))
// 2, 4, 6
```
- **switchMap** - for every value of the first observable, it does the operation defined in the output observable (switchMap), and emits the modified values

```
of(1,2,3).pipe(switchMap(x => of(x**2, x**3)))
.subscribe(x => console.log(x))
// 1, 1, 4, 8, 9, 27
```
- **expand** - used for recursion until defined condition is met 

```
.pipe(
     expand((data) =>
       data.length < limit // condition
         ? EMPTY           // end the recursion
         : this.getDashboardDataForApplicants((offset += limit)) // continue to call recursive function
     )
```

#### ***Filtering***  
- **debounceTime** - It's like delay, but passes only the most recent notification from each burst of emissions.  
In delay, it only delays the emitting of the value. All values are emitted.    
In debounce time, it does not emit value, until time is elapsed, and there is no new value emitted during that time.  
If a new value is emitted during that time, then time starts ticking again before emitting the most recent one.  

```
this.searchControl.valueChanges
.pipe(debounceTime(2000)) // don't emit value, until 2 sec passed. Then emit latest value
.subscribe(x => console.log(x))
```
- **distinct** - Returns an Observable that emits all items emitted by the source Observable that are distinct by comparison from previous items.

```
of(1, 1, 2, 2, 2, 1, 3, 4, 3)
  .pipe(distinct())
  .subscribe(x => console.log(x));
// 1, 2, 3
```
- **filter** - Filter items emitted by the source Observable by only emitting those that satisfy a specified predicate.

```
 let id = 3;
 of(1, 2, 3, 4).pipe(filter(x => (x == id)))
.subscribe(x => console.log(x));
// 3
```
- **take** - Takes the first count values from the source, then completes.

```
of(1,2,3).pipe(take(1)).subscribe(x => console.log(x));
// 1
```
- **skip** - Returns an Observable that skips the first count items emitted by the source Observable.

```
of(1,2,3).pipe(skip(2)).subscribe(x => console.log(x));
// 3
```

#### ***Join***  

#### ***Multicasting***  

#### ***Error Handling***  
- **catchError** - Catches errors on the observable to be handled by:
  
a) returning a new observable
```
// emit 1, 2, 3, on 4 error is thrown
  catchError(err => of('new', 'obs', 'emit')) // catch, create new obs, continue to emit values
// 1, 2, 3, new, obs, emit
```
   b) throwing an error 
```
 catchError(err => {
      throw 'error Medi: ' + err; // catch, edit error, rethrow
    })
// 1, 2, 3, error Medi: four! 
```
- **retry** - *if an error happens*, it will resubscribe to the observable for the N times, and return combined values.
> If there is no error occurred, retry will not be executed

All items emitted by the source Observable will be emitted by the resulting Observable, even those emitted during failed subscriptions.  
 For example, if an Observable fails at first but emits [1, 2] then succeeds the second time and emits: [1, 2, 3, 4, 5, complete] then the complete stream of emissions and notifications would be: [1, 2, 1, 2, 3, 4, 5, complete].

```
    of(1, 2).pipe(
      tap(n => {
        console.log(n)
        throw new Error('medi err'); // if we did not have this line, retry would not be executed
      }),
      retry(3)
    ).subscribe();
// 1, 1, 1, 1
```
#### ***Utility***  
- **tap** - Used to perform side-effects actions for values recieved from observable  
The most common use of tap is actually **for debugging**. You can place a *tap(console.log)* anywhere in your observable pipe, log out the notifications as they are emitted by the source returned by the previous operation.
```
 of(1, 2).pipe(
   tap(n => {
     console.log(n)
   })
 ).subscribe();
// or
of(1, 2).pipe(tap(console.log)).subscribe();
```
- **delay** - Delays the emission of values from the Observable by a given timeout (or until a given Date)  

```
.pipe(delay(1000))
```
#### ***Conditional and Boolean***  
#### ***Mathematical and Aggregate***  
- **reduce** - Applies an accumulator function over the source Observable, and returns the accumulated result when the source completes, given an optional seed (initial) value.  
Use it to combine values, for example, to sum numbers, or to get multiple arrays, and accumulate them into one.  
![image](https://github.com/user-attachments/assets/dd322eb1-f2c9-4aff-a441-93372ba6ff35)  
```
reduce((acc, val: any) => {
       acc = [...acc, ...val.body]; // acc contains 1st initial value, and after that accumulated returned value
       return acc;                  // modified acc will be forwarded to the next iteration until the observable completes
     }, [])                         // empty arr is seed (initial) value
```
- **max, min, count** - when observable completes, it returns max value, min value, or count of emitted values
------
# If you are unsure which operator do you need, you can use Operator Decision Tree
https://rxjs.dev/operator-decision-tree  
It's very cool, it asks you questions, what do you want to achieve, and returns the needed operator.  

------
# Coding examples
## 1. Keep triggering HTTP calls in Angular until a condition is met
Usually when you have paging or something, APIs are created to get specific amount of data per request, for example we have a limit of getting 100 records/objects per request.  
Call this API recursively with RxJS functions.  

Effect: 
```
  getDashboardWithApplicants$ = createEffect(() => {
    return this.actions$.pipe(
      ofType(actions.getDashboardWithApplicants),
      concatLatestFrom(() => [this.store.select(selectState)]),
      switchMap(([action, state]) => {
        return this.getAllDashboardDataForApplicants().pipe(
          map((dashboardData: any) => {
            this.addInitialsAndAvatarColors(dashboardData);
            this.getColorForUserAvatar(dashboardData);
            return actions.getDashboardWithApplicantsSuccess({ data: dashboardData });
          })
        );
      })
    );
  });
```  
Implementation of recursive calls of API:  
***offset*** - in APIs, **ignore first N objects**, ```someApi?offset=10```, will skip first 10 records, and return the rest
```
 getDashboardDataForApplicants(offset = 0): Observable<any> {
   return this.bgcService.getBackgroundCheckDashboard(SubjectTypeEnum.Applicant, offset);
 }

 getAllDashboardDataForApplicants() {
   let offset = 0;
   return this.getDashboardDataForApplicants(offset).pipe(
     // expand is used to call recursively until condition is met
     expand((data) =>
       data.body.length < limitForDashboardData
         ? EMPTY
         : this.getDashboardDataForApplicants((offset += limitForDashboardData))
     ),
     // use the reduce operator to emit the accumulated array with all items
     reduce((acc, val: any) => {
       acc = [...acc, ...val.body];
       return acc;
     }, [])
   );
 }
```
RxJS:  
- **expand** - for recursion  
  Recursively projects each source value to an Observable which is merged in the output Observable.
- **reduce** - to combine multiple responses into one.  
  Applies an accumulator function over the source Observable, and returns the accumulated result when the source completes, given an optional seed (initial) value.
- **EMPTY** - A simple Observable that emits no items to the Observer and immediately emits a complete notification.
