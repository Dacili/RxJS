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
### What is RxJS?
RxJS is a powerful library for web development. It's focused on managing asynchronous data streams and enabling reactive programming paradigms.
### Why to use it?  
If anything in your app happens asynchronously, an RxJS Observable will **make your life easier.**   

### Concepts:
- #### Observable
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

- #### Observer
 is a consumer of values delivered by an Observable.  
 ![image](https://github.com/user-attachments/assets/0592e133-a9c0-4691-8961-ddc3a008098d)

- #### Subscription
 represents the execution of an Observable, is primarily useful for cancelling (unsubscribing) the execution.  
```
const subscription = observable.subscribe(x => console.log(x));
subscription.unsubscribe();
```
- #### Operators
 are pure functions that enable a functional programming style of dealing with collections with operations like map, filter, concat, reduce, etc.
- #### Subject
 is equivalent to an EventEmitter, and the only way of multicasting a value or event to multiple Observers.
- #### Schedulers
 are centralized dispatchers to control concurrency, allowing us to coordinate when computation happens on e.g. setTimeout or requestAnimationFrame or others.


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
