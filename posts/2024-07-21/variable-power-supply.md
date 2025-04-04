---
title: "Variable Power Supply"
description: "Create a variable power supply using an old laptop power supply"
date: 2024-07-21T22:01:24+09:30
tags: ["maker", "electronics", "diy"]
featured_image: "/images/power-supply/finished.jpg"
---

## Introduction

In today's world of electronics and DIY projects, having a reliable power supply is essential. However, purchasing a variable power supply can be quite expensive. In this blog post, we'll guide you through creating your own variable power supply using an old laptop charger. This project not only saves you money but also gives new life to old electronic components that might otherwise end up in landfill.

## Parts Required

To embark on this project, you'll need the following items:

- A 3D printer
- An old laptop power supply with an output DC voltage greater than 6 volts
- A Programmable Constant Voltage Current Step-down Power Supply Module [WZ3605E](https://vi.aliexpress.com/w/wholesale-WZ3605E.html?spm=a2g0o.home.search.0)
- Banana plug socket [Link](https://vi.aliexpress.com/w/wholesale-banana-socket.html?spm=a2g0o.productlist.search.0)
- Barrel connector [Link] https://vi.aliexpress.com/item/1005006512605602.html?spm=a2g0o.productlist.main.1.154d72a0O2bLhC&algo_pvid=a48a2e5b-c98f-46a3-9efb-e96b140d6b7e&algo_exp_id=a48a2e5b-c98f-46a3-9efb-e96b140d6b7e-0&pdp_npi=4%40dis%21AUD%213.02%213.02%21%21%2114.53%2114.53%21%402101c80017215653450678713e0b25%2112000037482647997%21sea%21AU%212747051205%21&curPageLogUid=9XwmU1UcmZtW&utparam-url=scene%3Asearch%7Cquery_from%3A
- Soldering iron
- Quality wires rated for ~5amps
- Autodesk Fusion 360 design files 

## Steps with Assembly Pics

### Step 1: Gather All Components

Ensure all the required components are at hand before starting the assembly process. This includes checking the output voltage of the old laptop charger to confirm it meets the minimum requirement.

### Step 2: 3D Print the Enclosure

Using the Autodesk Fusion 360 design files provided, print the enclosure for your power supply. This will house all the components securely.

![3D Printed Enclosure](images/power-supply-enclosure.jpg)

### Step 3: Prepare the Laptop Charger

If the laptop charger does not have the same barrel connector as the socket you purchased, you can splice the cable and only connect the power cables to the barrel jack. I used a 2.1mm barrel jack and socket, purchased from [Jaycar](https://www.jaycar.com.au)


### Step 4: Assemble the Power Supply Module

Connect the Programmable Constant Voltage Current Step-down Power Supply Module WZ3605E to the barrel connector attached to the back of the lid (see the STL files linked above). Ensure proper connections for input and output voltages to the module. The module I have linked has simple screw in connectors.

Inputs (6V-36V) - Connected from the barrel connector to the modules input terminals.

Outputs (6V-36V) - Connected to the banana socket terminals (which we will use as our outputs)

![Assembled Power Supply Module](link_to_your_image_here)

### Step 5: Install Banana Plug Socket and Barrel Connector

Solder the banana plug socket and barrel connector to the output terminals of the power supply module. These will serve as the connection points for your devices.

![Installed Connectors](link_to_your_image_here)

### Step 6: Final Assembly and Testing

Place all assembled components inside the 3D printed enclosure. Connect the wires appropriately, ensuring safety and functionality. Test the power supply with a multimeter to confirm the output voltage range.

![Final Assembly](link_to_your_image_here)

## Conclusion

Creating a variable power supply from an old laptop charger is not only cost-effective but also environmentally friendly. By following the steps outlined in this guide, you've successfully repurposed an old electronic device into a useful tool for your projects. Remember, safety first when dealing with electronics. Enjoy experimenting with your new variable power supply!

---
