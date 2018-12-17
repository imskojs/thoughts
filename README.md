# thoughts

1) Elm forces you to think in functional programming paradigm. Knowing Elm helps in learning Rx.
2) Easiest way to read `const setHTML = id => val => (document.getElementById(id).innerHTML = val);` is, `setHTML` is a function that takes `id`, `val` as arguments and return `(document.getElementById(id).innerHTML = val)` with a capability of currying.
