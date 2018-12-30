# thoughts

1) Elm forces you to think in functional programming paradigm. Knowing Elm helps in learning Rx.
2) Easiest way to read `const setHTML = id => val => (document.getElementById(id).innerHTML = val);` is, `setHTML` is a function that takes `id`, `val` as arguments and return `(document.getElementById(id).innerHTML = val)` with a capability of currying.
3) *Cold Observable*: Has the producer created inside the observable, so any subscription on cold observable have its own seperate producer each time it gets subscribed to. This means a producer can only send to one subsciption (observer) making it unicast.
4) *Hot Observable*: Has the producer outside the observable. Any number of subscription will share the reference to the producer. Since the producer is outside of the observable we cannot teardown the producer. For this particular reason we do not use the hot observable. But rather make a cold observable hot
5) `share()` operator will recycle dead subjects to enable resubscription to the resulting observable.
6) *Warm Observable*: Has more than one producers, one outside, another inside Observable. This maybe necessary when a multiplexed web socket for example, must share a socket, but send its own subscription and filter out a data stream.
7) `throttleTime`: emits first value waits for a given time. repeat
8) `debounceTime`: emits latest value after given time and no more event happens. everytime event happens time is reset. repeat.
9) `auditTime`: Similar to `debounceTime` but do not reset time. Emits latest value after given time. repeat.
10) `ng2-stompjs` uses `rx-stomp` which uses `stomp.js` which can be used together with `sock.js`
    * `sock.js` is a polyfill for websocket
    * `stomp.js` is a implementaion of a STOMP standard on top of Websockets protocol.
    * `rx-stomp` exposes `stomp.js` props and methods as observables.
    * `ng2-stompjs` uses `rx-stomp` to make available injectable services.  
11) Current problem when working with observables is guessing whether it is Cold or Hot. In my opinion
we should make two types to distinguish this, for example;
```ts
type ColdObservable<T> = Observable<T>
type HotObservalbe<T> = Observable<T>

const myHotObservable$: HotObservable<Data> = rxStompService.watch('/room/messages');
const myColdObservalbe$: ColdObservable<Data> = http.get('http://meh.com/user/id');
```
12) It's generally a bad idea to delete an Observable. Since Observable is really just a function, it will be passed around by reference hence deleting it with something like `delete` keyword will only delete the current reference. Since observables are usually composed together to make other observables, other references to the same observable will most likely exist. This may lead to memory leak.  For example  
```ts
const x = {obs1: myObservable$.pipe(takeUntil(cancelSub$)) }
const myComposedObservable$ = merge(x.obs1, myOtherObservable$)

// Some cancelling logic
cancelSub$.next('Unsub')  // this does not kill myObservable$, just unsubscribes existing Observer
delete x.obs1  // deletes one reference to myObservable$ another refrence exists as `merge` used x.obs1 before deletion.

// Instead of reusing myObservable$ we create another exactly same observable in difference memory location.
if(!x.obs1) {
  x.obs1 = myNewObservable$
}
// Now we have two observables doing exactly the same thing in different memory locations leading to memory leak.

```
