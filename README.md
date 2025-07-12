# esphome

![GitHub](https://img.shields.io/github/license/alanbchristie/esphome)
![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/alanbchristie/esphome)

A personal repository capturing my work developing [ESPHome] devices on macOS
Sequoia to use with my [Home Assistant] network, where I tend to specialise in [Pico]
devices based on the ARM-based RP2040 wireless micro-controller.
You'll find all the device files in the `device-files` directory.

I've simplified the launch of the ESPHome container by relying on Docker Compose
(part of my standard software development toolset).

>   On the Mac, the first ESPHome firmware install is via a hard-wired USB connection.
    From this point on, with the ESPHome [ota] key set you can update the firmware
    when the device is connected to Home Assistant from the container image over-the-air.

## Getting started with ESPHome
As well as Docker, you'll need: -

- Docker Compose
- A suitable USB Cable

Launching the ESPHome builder container image from the root of the project: -

    docker compose up -d

With this done the builder will be available at http://localhost:6052

When you're done you can stop the container with: -

    docker compose down

## Devices

**rain-water.yaml** is my first super-simple device based on their tutorials.
It's for a [Pico] W on a [mini] board with a **switch** to control a solenoid
water valve and a **binary input** driven by a rain/snow sensor.

---

[docker compose]: https://docs.docker.com/compose/install
[esphome]: https://esphome.io
[home assistant]: https://www.home-assistant.io
[mini]: https://shop.pimoroni.com/products/automation-2040-w-mini?variant=40336518086739
[ota]: https://esphome.io/components/ota
[pico]: https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html#pico-1-family
