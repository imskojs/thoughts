# thoughts

1) Elm forces you to think in functional programming paradigm. Knowing Elm helps in learning Rx.
2) Easiest way to read `const setHTML = id => val => (document.getElementById(id).innerHTML = val);` is, `setHTML` is a function that takes `id`, `val` as arguments and return `(document.getElementById(id).innerHTML = val)` with a capability of currying.
3) *Cold Observable*: Has the producer created inside the observable, so any subscription on cold observable have its own seperate producer each time it gets subscribed to. This means a producer can only send to one subsciption (observer) making it unicast.
4) *Hot Observable*: Has the producer outside the observable. Any number of subscription will share the reference to the producer. Since the producer is outside of the observable we cannot teardown the producer. For this particular reason we do not use the hot observable. But rather make a cold observable hot
5) `share()` operator will recycle dead subjects to enable resubscription to the resulting observable.
