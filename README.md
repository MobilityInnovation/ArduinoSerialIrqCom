# README

This is a small library to let an OS-based computer (e.g. windows or linux) communicate with an Arduino through simple python calls, without relying on the Serial interrupt routine. It is thus suitable to be used in combination with an LED library like FastLED.

# How do I get set up?

Arduino:

* Currently no dependencies
* Just include the header file, call setup() in the Arudino's setup routine and receive in the loop routine, whenever you are ready to receive something.

```(C)
#include "SerialIrqCom.h"

void setup() {
    SerialIrqCom::setup();
}

void loop() {
    // Receives one char at a time
    SerialIrqCom::ReturnType msgState = SerialIrqCom::receive();

    if (msgState == SerialIrqCom::ReturnType::NewCommandAvailable) {
        // The full command/msg received is now available as a string via  the char array SerialIrqCom::msg
    } else if (msgState == SerialIrqCom::ReturnType::Idle) {
        // Do something else, which might be resource costly and/or ignores interrupts
    }
    // msgState might also be of type ReturnType::Receiving.
    // In this case, don't do anything and directly go back to the receive routine...
}
```

Python Script

* pySerial required (see requirements.txt)
* Import the SerialIrqCom file and create an instance of Class SerialIrqCom.

```(Python)
import SerialIrqCom

com_interface = 'Com1'
baudrate = 9600
timeout_time = 10.0

# Create an instance of this type
sic = SerialIrqCom.SerialIrqCom(com_interface, baudrate=baudrate, timeout_time=timeout_time)
sic.open()

# Send something (blocking)
sic.send_message("Hello Arduino")

# Receive something (blocking)
array_of_lines_received = sic.receive_message()

# Send something and receive the answer (blocking)
array_of_lines_answered = sic.send_receive_message("Hello Arduino")

# Close the connection
sic.close()
```

# License

Distributed under MIT license. See `LICENSE` for more information.

# Who do I talk to?

* Lukas Block - [lukas.block(a)iao.fraunhofer.de]()