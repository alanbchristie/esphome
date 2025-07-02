# esphome

![GitHub](https://img.shields.io/github/license/alanbchristie/esphome)
![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/alanbchristie/esphome)

A personal repository capturing my work developing [ESPHome] devices on MacOS
to use with my [Home Assistant] network, where I tend to specialise in [Pico] devices
based on the ARM-based RP2040 wireless micro-controller. You'll find all the device
files in the `device-files` directory.

I've simplified the launch of the ESPHome container by relying on Docker Compose
(part of my standard software development toolset).

## Getting started
As well as Docker, you'll need: -

- Docker Compose

Launching the ESPHome builder container image, from the root of the project: -

    docker compose up -d

With this done the builder will be available at http://localhost:6052

## Devices

**garage.yaml** is my first super-simple device based on their tutorials.
It's for a Pico on a [Pico Explorer] board with a pair of `binary_sensor` definitions
for the explorer **X** and **Y** buttons.

---

[docker compose]: https://docs.docker.com/compose/install
[esphome]: https://esphome.io
[home assistant]: https://www.home-assistant.io
[pico]: https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html#pico-1-family
[pico explorer]: https://shop.pimoroni.com/products/pico-explorer-base?variant=32369514315859
