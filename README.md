# RxJS = Reactive Extensions for JavaScript
### What is a programming paradigm?
A programming paradigm is a relatively high-level way to conceptualize and structure the implementation of a computer program.   
https://en.wikipedia.org/wiki/Programming_paradigm  
Types of programming paradigm:  
![image](https://github.com/user-attachments/assets/2230b708-cc7e-412b-ac5c-48b452619e16)
### Imperative vs declarative programming
### What is declarative programming?
Declarative programming is a programming paradigm—a style of building the structure and elements of computer programs that expresses the logic of a computation without describing its control flow.
### What is reactive programming?

### What is RxJS?
RxJS is a powerful library for web development. It's focused on managing asynchronous data streams and enabling reactive programming paradigms.
### Why to use it?

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
- expand  
- reduce  
- EMPTY
