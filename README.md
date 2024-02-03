Long range, low power messaging system based on Meshtastic relays suitable for underground communications.

## Design principles
Natural caves are challenging as it comes to communications due to irregular cavity shapes, presence of water and general difficulty of installing any kind of equipment in an environment where everything has to be carried on human's back over hundreds of meters vertically. There are numerous existing solutions, each with its own challenges:

* Wired telephone is a reliable solution, however poses a significant investment due to the above challenges, plus it is easily damaged in confined or gravel passages.
* [Through-the-earth radio communications](https://en.wikipedia.org/wiki/Through-the-earth_mine_communications) using very low radio frequencies (HeyPhone, Nikola, CaveLink) are the most popular _ad-hoc_ communications solution during cave rescue or expeditions . Obtaining a reliable link at depths below hundreds of meters however is a lottery as many factors from geology to weather impact attenuation even at  low frequencies.
* Underground communication links based on radio repeaters was discussed at least since 2014[^1]. _Sybet_ came up with industrial solution [SPELLCOM](https://sybet.eu/batnode/)  using radio repeaters to relay voice communications over underground cavities.

The **Vangelis** project expands on all of the above by using autonomous low-power repeaters for relaying text messages rather than voice over low-power radio transmission:

* Radio transmission using [LoRA](https://en.wikipedia.org/wiki/LoRa) modulation using [Meshtastic protocol](https://meshtastic.org/docs/introduction) for routing
* Low-power radio transmission with maximum 200 m line-of-sight range for underground nodes
* Long-term autonomous operations of each relay node
* Low weight and portability of the nodes
* Range of surface relays limited only by LoRa radio reach, practically up to a few kilometers in mountainous areas

<figure>
    <img src="drawing.svg" width="297" height="210"
         alt="Vangelis architecture overview">
    <figcaption>Vangelis architecture overview.</figcaption>
</figure>

## Hardware

The system is composed of two node types: surface and underground. Each node operates in router-client mode, which allows both client device (smartphone) connection over Bluetooth and message relaying to other nodes over LoRa.

* **surface node**  powered from a 18650 cell with [PV charging](https://docs.google.com/document/d/12GIY24vLKLABg2RUTPP6yMzokr44GMzJOE4p7ngaCbI/edit#heading=h.9lmvuqjahqxl), equipped with a 3-5 dBi antenna; typical usage scenario is to be installed at cave entrance and relay messages further to another such node at a base camp;
* **cave node** powered from a LiPo battery and intended to operate without recharging for the whole period of a cave operation

### Surface node
Bill of materials:
* [Clear Hinged Lid IP67 Waterproof ABS Electrical Enclosure Case junction box](https://www.ebay.co.uk/itm/304321459053)
* [WisBlock Meshtastic Starter Kit](https://store.rakwireless.com/products/wisblock-meshtastic-starter-kit)
* OLED Display [Solomon SSD1306 - RAK1921](https://store.rakwireless.com/products/rak1921-oled-display-panel)
* [RAKBox-B5 Transparent Acrylic Enclosure](https://store.rakwireless.com/products/rakbox-b5-transparent-acrylic-enclosure)
* [Outdoor LoRA antenna](https://store.rakwireless.com/collections/outdoor-antennas)
* 3D-printed antenna holder, STL for RAK [3dBi Fiberglass Antenna](https://store.rakwireless.com/products/3dbi-fiber-glass-antenna) or [5dBi Fiberglass Antenna](https://store.rakwireless.com/products/5dbi-fiber-glass-antenna-supports-863-870mhz) are available
* [5V  1.25W  250 mA photovoltaic panel](https://www.ebay.co.uk/itm/113383833070), 110mm x 69mm, 2.4mm

Notes:

* OLED display is optional. It's mostly useful when pairing smartphone with Meshtastic over Bluetooth using a random PIN code, but since these are impractical underground, I suggest using no PIN at all or a fixed PIN, both of which need to be configured using Meshtastic CLI prior to deployment. Since it also displays last packet information etc it's a nice to have.
* Choice of the antenna is of paramount importance for outdoor LoRA range. Poor antenna will limit the range to tens or hundreds of meters at best, even in line of sight. With RAK 3-5 dBi antennas we easily get kilometers long range. It does not matter for underground nodes as their range is limited by rock anyway.
 
### Cave node
Bill of materials:
* [RAK WisBlock Mini Base Board RAK19003](https://store.rakwireless.com/products/wisblock-base-board-rak19003)
* [RAK LoRa Core Module RAK4631](https://store.rakwireless.com/products/nordic-nrf52840-ble-core-module-for-lorawan-with-lora-sx1262-rak4631-rak4631-c)
* [RAK barometric pressure sensor RAK1902](https://store.rakwireless.com/products/rak1902-kps22hb-barometric-pressure-sensor)
* [LoRa 2 dBi SubG antenna](https://store.rakwireless.com/products/lora-antenna)
* [IPEX to RP-SMA connector](https://store.rakwireless.com/products/ipex-to-sma-connector)
* [BLE-PCB Bluetooth antenna](https://store.rakwireless.com/products/ble-pcb-antenna-5-5dbi)
* [TacMesh Waterproof Enclosure](https://www.thingiverse.com/thing:5923930/) 3D printed from PLA, [assembly instructions](https://youtu.be/gpnivx2jVRk)
* [Waterproof on-off switch](https://www.amazon.co.uk/dp/B0BKK4RXYX)
* [3.7V 2000 mAh LiPo battery](https://www.amazon.co.uk/dp/B08HD33ZKB)
* [Rubber washer 16x6x1.5 mm](https://www.amazon.co.uk/dp/B01N7WI68Z)
* [Rubber o-ring 49x45x2 mm](https://www.amazon.co.uk/dp/B0C1524ZMR)
* [Tactile 4x4x1.5 mm push button](https://www.amazon.co.uk/dp/B08F7V2Y66)
* [USB-C male to female coupler](https://www.amazon.co.uk/dp/B0B18SLFD4)
* [Rubber USB-C plugs](https://www.ebay.co.uk/itm/144021649084)

### Barometric pressure
As cave nodes are located through the cave, they have no means of determining their own location as GPS signal is unavailable. Barometric pressure sensor allows to [correlate the pressure](https://en.wikipedia.org/wiki/Barometric_formula) seen by a node with altitude above mean sea level ([AMSL](https://en.wikipedia.org/wiki/Height_above_mean_sea_level)) as long as at least one surface node is equipped with GPS receiver _and_ barometric pressure sensor.
## Footnotes

[^1]: M. D. Bedford; G. A. Kennedy [Modeling Microwave Propagation in Natural Caves Passages](https://ieeexplore.ieee.org/abstract/document/6933914/),  IEEE Transactions on Antennas and Propagation, 2014
