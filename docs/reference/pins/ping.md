# Ping

Sends a pulse on a pin and measure the time/distance for the response.

```sig
pins.ping(DigitalPin.P0, DigitalPin.P1, PingUnit.Centimeters)
```

### Parameters

* **trigger**: the trigger pin
* **receiver**: the receiver pin
* **unit**: the unit of the returned number
* **max centimeter distance**: the maximum distance to scan for

### Example

This program shows the measured distance in centimeters when the user presses the ``A`` button.

```blocks
input.onButtonPressed(Button.A, () => {
    basic.showNumber(pins.ping(DigitalPin.P0, DigitalPin.P1, PingUnit.Centimeters));
})
```

### See also

[@boardname@ pins](/device/pins)
