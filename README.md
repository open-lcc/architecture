# Architecture

## Microcontroller roles

When using Open LCC, there's usually more than one microcontroller involved. The microcontrollers serve different roles, and they communicate with eachother to serve these roles.

### Systems Controller
The systems controller handles the grunt work. Running PID algorithms, refilling boilers when low, PWM:ing gear pumps, all of that stuff. The systems controller is an RP2040. It communicates with the HID Controller and the IO Expander via UARTs.

### RP2040 IO Expander
The Systems Controller doesn't necessarily have all the IO needed by modern, advanced espresso machines. The RP2040 IO Expander aims to fix that.

### ESP32-S3 IO Expander
The ESP32-S3 IO Expander serves the same role as the RP2040 IO Expander, but allows access to the wireless world of Bluetooth and Wi-Fi.

### Gicar 8.5.04 IO Expander
Because this project started out on Lelit Machines, there is a special place in it's heart for Gicar 8.5.04 controllers, allowing those controllers to be used as IO Expanders too.

### HID Controller (and ESP32-S3 IO Expander)
The HID controller presents a user interface. This might be a display with buttons and knobs, it might be an app or website communicating over Wi-fi or Bluetooth, it might be something completely different. The HID Controller is an ESP32-S3.

Of course, no architecture would be complete without quirks, and one of those is that the HID Controller can also act as an ESP32-S3 IO Expander.

### RP2040 UART Passthrough
Sometimes you might want an RP2040 to just grab data from one UART and pass it along to another. That's what the RP2040 UART Passthrough role is for.

## PCBs

### Open LCC Main Board
The Open LCC Main Board has the same form factor as a LCC. It contains one RP2040, and one ESP32-S3. It also features an SD Card reader, and a separate settings flash.

The RP2040 µC is intended to serve one of the following roles:

* Systems Controller
* RP2040 UART Passthrough

The ESP32-S3 µC is intended to serve the HID Controller role.

### All-Purpose Espresso Controller
The All-Purpose Espresso Controller also contains one RP2040 and one ESP32-S3. It has the same form factor as a Gicar 8.5.04.91 controller. It has one UART bus that can connect either to an Open LCC Main Board, or to a UART based display (something like a Nextion).

The RP2040 µC is intended to serve one of the following roles:

* Systems Controller
* RP2040 IO Expander

The ESP32-S3 µC is intended to serve one of the following roles:

* ESP32-S3 IO Expander
* HID Controller

### Combining PCBs
Depending on what you want to do with your espresso machine, you may want to combine different PCBs, running the µCs in different roles. Here are some examples.

#### Lelit Bianca with stock hardware
In this configuration, you'd use the Open LCC Main Board to replace the stock LCC.

* Open LCC Main Board RP2040 - Systems Controller
* Open LCC Main Board ESP32-S3 - HID Controller
* Gicar 8.5.04 - Gicar 8.5.04 IO Expander

#### Lelit Bianca with a gear pump
In this configuration, you'd still replace the stock LCC with an Open LCC Main Board, but you'd also replace the Gicar 8.5.04 with an All-Purpose Espresso Controller. Because the Open LCC Main Board has a settings flash and an SD Card reader, the recommended configuration is as follows:

* Open LCC Main Board RP2040 - Systems Controller
* Open LCC Main Board ESP32-S3 - HID Controller
* All Purpose Espresso Controller RP2040 - RP2040 IO Expander
* All Purpose Espresso Controller ESP32-S3 - ESP32-S3 IO Expander

#### Rancilio Silvia

In this configuration, an Open LCC Main Board becomes unnecessary, and you'd just use an All Purpose Espresso Controller.

* All Purpose Espresso Controller RP2040 - Systems Controller
* All Purpose Espresso Controller ESP32-S3 - HID Controller