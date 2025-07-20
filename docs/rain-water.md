# The Rain/Water Device
I had a timer-based watering system some years ago. Driven by an outside tap
under mains pressure. One time, while awy, it failed - with the valve separating from
the tap. I lost a lot of water that year! Luckily a neighbour heard the water and had
the courtesy to climb into the garden and turn the tap off.

Now I want to water some key plants but many years have elapsed and we have
extremely powerful and cheap micro-controllers and automation frameworks
(like Home Assistant).

## Components
We need a few components to bring the rain-sensing automated watering system together.
At a total cost of about £81, you'll need the following, along with some wiring and a
board to mount the devices on (one day I wil get myself a 3D printer to make a proper
housing).

For now I'm happy to lay the components out on a board with some neat wiring.

### Electric 12 solenoid valve and hosepipe adapters
Rather than a plastic device I decided to go with a 1/2 inch BSP solenoid valve.
This should allow sufficient water through without resorting to micro-bore
pipes. It's operated by applying 12v DC and will draw about 3A when the solenoid is
activated (allowing the flow of water).

It's a Tailonz Pneumatic brass electric [solenoid valve].

- Cost: About £25.49 from Amazon

I also intend to connect a standard hosepipe to the valve with an approximate internal
diameter of 13mm and external diameter of 17mm. The supplied BSP adapters are too small
so I need a suitable brass 1/2 inch adapter (and jubilee clips). I get these from
[The Hosemaster]. A pair of Male Taper BSP Brass Hose Tail Adaptors
(MH08/08 - see their NH.pdf document for precise details) and a box of 10 stainless
steel 13mm-20mm jubilee clips (I might need more jubilee clips).

- Cost: £36.36, including shipping.

### RP2040
For ESPHome I chose to use a wireless Raspberry Pi Pico (RP2040), a super-cheap
micro-controller that's part of the Raspberry Pi family. Currently the ESPHome
framework does not support the RP2040 2 so it has to tbe the first generation
wireless device. But, with 12v required and 3A circuitry a bare RP2040 was not enough.
Fortunately Pimoroni make a neat-little development board with a number of buffered
inputs (required for the rain sensor). It also accepts a generous power-supply voltage
range of between 6 and 40 volts! It has a little too much peripheral hardware for our
needs but it's relatively cheap and at least has options I might need.

So I chose to use the Automation 2040 W [Mini] (Pico W Aboard)

- Cost: About £19.50 from [Pimoroni]

### 5v Relay capable of 3A or more
We need a switching relay that can carry at least 3A. The one on the Pimoroni board
is not powerful enough so a cheap external device is needed. This will be used
to switch 12V onto the water solenoid, which needs 3A. The chosen relays
are 5v and easily controlled from a GPIO Pin (and the Pico board also has
a convenient 5v output to power the relay circuitry.

I chose the single-channel Innfeeltech 3pcs DC 5V [Relay Module]

- Cost: About £7.49 for 3 from Amazon

### LED 12v driver (capable of at least 5A)
The neatest and most convenient way to get 12v power (delivering 5A) is to buy
an LED driver. The 60W [Reylax] 12V LED Driver is perfect. Two wires in (mains) and
two wires out (12v).

- Cost: About £12.99 from Amazon

### Rain sensor
This looks like a rugged sensor, designed to operate skylights. It also operates
off 12V, although it draws very little current. It's made by H-Tronic, their RS 12
[Rain Sensor].

The `OUT` terminal of the sensor is at 12V when dry. When the sensor is wet the
sensor plate is heated (and it does get quite warm) and the `OUT` terminal it taken
to ground. So you just have to wire the `OUT` terminal to one of the RP2020
development board's two inputs. You don't need to do anything else -
the input is at 12V (**ON**) when it's dry and at 0V (**OFF**) when it's raining.
We **invert** the input so that it's logically **ON** when raining.

- Cost: About £20.66 from Rapid Electronics

## Sensor and solenoid wiring
Part of the wiring will be outside and although it's low voltage (12V) we still need
to protect from the weather, so UV-tolerant wiring wil be essential. It's not **mains**
so we don't have to worry about conduits and protection.

for the rain sensor (which needs 3 wires) I chose a 3-core **LappKabel 3-core truck cable**
distributed by Rapid Electronics. It fits the sensor grommet perfectly, is flexible and
harsh weather and UV tolerant: -

- https://www.rapidonline.com/lappkabel-7027062-lflex-truck-170-flryy-black-data-cable-3-x-1mm-63-6742

For the solenoid, which draws about 3A we'll need two-core 0.5mm2 cable and **LappKabel**
come to our rescue again with an extremely robust cable for outdoor environments: -

- https://www.rapidonline.com/lappkabel-0021880-robust-210-black-data-cable-2-x-0-5mm-no-earth-63-4752

## Creating a rain history 'helper'
The **Garage Roof Rain and Snow Sensor** is a binary sensor that's **On** when it's
raining. On its own the sensor can only provide the current state - i'ts raining (now)
or it is not. Because I want to automate a watering system I actually need an *estimate*
of how "wet" it's been. I can get a crude estimate of how *wet* it's been by knowing how
*long* it's been raining (over the past few days). To do this we employ the standard
**Home Assistant** [History Stats] integration.

The History Stats integration can providing a rolling window of "on" (or "off') duration
for binary sensors over a period of time. It can provide the total **time** our binary
sensor was "on" or the **ratio** (a percentage of time) the sensor was "on" for a
given duration.

To report the total time in hours it's been raining over the last 3 days ending *today*
at 23:59 we can configure the history stats helper like this: -

- **entity** `"Garage Roof Rain and Snow Sensor"`
- **state** `"On"`
- **type** `"time"`
- **end** `"{{ today_at('23:59') }}"`
- **duration** `"3d"`

This gives us a sensor history with a unit of measurement that's hours with a
default display precision of 0.00 (I alter this to '0.0').

>   I don't like times that are `00:00` because it always causes confusion
    about whether that's the start of the day or the end of the day.
    It's why airplanes or trains don't depart at `00:00` - it's always
    `00:01` or `23:59`.

---

[mini]: https://shop.pimoroni.com/products/automation-2040-w-mini?variant=40336518086739
[pimoroni]: https://shop.pimoroni.com
[rain sensor]: https://www.h-tronic.de/en/Rain-Sensor-RS-12/1115275
[reylax]: https://www.amazon.co.uk/dp/B0C9P4QXBF?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1
[relay module]: https://www.amazon.co.uk/dp/B0CM2Y1WMZ?ref=ppx_yo2ov_dt_b_fed_asin_title
[solenoid valve]: https://www.amazon.co.uk/dp/B0C5ZR4148?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1
[the hosemaster]: https://www.thehosemaster.co.uk
[history stats]: https://www.home-assistant.io/integrations/history_stats/
