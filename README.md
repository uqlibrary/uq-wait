# uq-wait
A simple object allowing tests to wait for an element before continuing. This plugin uses callbacks, to be as generic
as possible and not tie itself to one particular testing framework. It was originally designed and tested with Polymer
Web Component Tester, as it does not come standard with this feature.

This plugin aims to remove all timeouts from tests, in particular timeouts on page load and after starting an action.
Old tests would have to set a predetermined timeout that was inflated to account for the speed of automated testing
platforms like Codeship.

## Functions
UQ Wait currently supplies 3 standard ways of waiting for an event, and one general "custom" function that allows for
any custom logic.

### UQWait.presenceOf(identifier, maxWait, callback)
Waits until an element is present in the DOM before invoking the callback function.

```
UQWait.presenceOf('#someDiv', 5000, function () {
    expect(element.items.length).to.be.equal(3);
});
```

### UQWait.visible(identifier, maxWait, callback)
Waits until an element becomes visible before invoking the callback function. This works only partly with Polymer, as it
uses generally strange logic to determine whether something is visible.

```
UQWait.visible('#someModal', 5000, function () {
    expect(1).to.be.equal(2);
});
document.querySelector('#saveButton').click();
```

### UQWait.hidden(identifier, maxWait, callback)
Waits until an element is hidden before invokin the callback function. As with UQWait.visible, it only works partially
with Polymer. It is important to note that if the element is not present in the dom, it counts as being hidden.

```
UQWait.hidden('#someModal', 5000, function () {
    expect(1).to.be.equal(2);
});
document.querySelector('#cancelModalButton').click();
```

### UQWait.custom(expression, maxWait, callback)
Waits until the custom expression returns true.

```
UQWait.custom(function () {
    return element._bookingTimeSlots.length > 0;
}, 5000, function () {
    expect(1).to.be.equal(1);
});
```

### Notes ###
Please note that Mocha's "done" callback can be passed straight to UQWait for more readable code. This is especially
useful in the setup:

```
setup(function (done) {
    element = fixture('element');
    element.activate();

    UQWait.custom(function () {
        return element._bookingTimeSlots.length > 0;
    }, 5000, done);
});
```
