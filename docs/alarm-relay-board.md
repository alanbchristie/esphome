# The Alarm Relay Device
I am frustrated with the 'closed' system that is my house alarm, and I want to be able
to use the external sounder - i.e. trigger a house alarm based on something I might
develop in [Home Assistant]. Maybe an advanced external intruder detector or a panic
button.

Sadly there's no _API_ or external interface to the alarm box in the house ... but there
are spare "sensor" inputs. SO, I can create my own "sensor" - basically an output
from Home Assistant - that I can use to trigger the house alarm. The alarm supports
several patterns - sensors that only sound the alarm when the alarm is "armed" and
sensors that sound the alarm even when it isn't a;arms (like panic buttons). The
sensors ate just relays.

So - create a Pico-based device, and have it connected to one (or two) relays.
The best (simplest) device for me was the Waveshare [Pico-Relay-B] board. You just
add a Pico and you have access to eight relays.

the board is about £19 from [PiHut].

You just need power and some 2-core wires to run to the alarm box and you then have
the ability to control eight 10A relays.

In my device file I plan to use relays 7 and 8 to trigger an alarm when armed and
for a panic button.

---

[home assistant]: https://www.home-assistant.io
[pico-relay-b]: https://thepihut.com/products/industrial-8-channel-relay-module-for-raspberry-pi-pico
[pi-hut]: https://thepihut.com
