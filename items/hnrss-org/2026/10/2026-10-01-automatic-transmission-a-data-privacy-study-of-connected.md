---
title: Automatic Transmission – a data-privacy study of connected vehicles
link: https://automatictransmission.khoury.northeastern.edu/index.html
source: hnrss-org
published: 2026-10-01T20:23:27Z
updated: 2026-10-01T20:23:27Z
first_seen: 2026-10-01T23:33:46.642706577Z
authors:
- rafaelc
content: extracted
html: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.html
preview:
  file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.preview-a4ce8ef715dd.webp
  width: 256
  height: 180
  alt: A connected vehicle
  color: '#6f6d67'
images:
- source: https://automatictransmission.khoury.northeastern.edu/images/test-facility.png
  original:
    file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-a17d33ace3ad.png
    width: 562
    height: 396
  variants:
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-3dd4a0c29097.webp
    width: 320
    height: 225
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-f11ca955c182.webp
    width: 562
    height: 396
  color: '#171717'
- source: https://automatictransmission.khoury.northeastern.edu/images/car-diagram.png
  original:
    file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-b5c1b768fa45.png
    width: 441
    height: 595
  variants:
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-f4ad95405d5a.webp
    width: 320
    height: 432
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-4c3d18ee91d7.webp
    width: 441
    height: 595
  color: '#fdfdfd'
- source: https://automatictransmission.khoury.northeastern.edu/images/rpi.png
  original:
    file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-7decdbe72971.png
    width: 640
    height: 487
  variants:
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-be670d638f9d.webp
    width: 320
    height: 244
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-e4938cb2dfb0.webp
    width: 640
    height: 487
  color: '#1a1a13'
- source: https://automatictransmission.khoury.northeastern.edu/images/tent-in-use.png
  original:
    file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-4dedace7c9c1.png
    width: 523
    height: 506
  variants:
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-afdf8f391d87.webp
    width: 320
    height: 310
  - file: 2026-10-01-automatic-transmission-a-data-privacy-study-of-connected.image-acd5182b667d.webp
    width: 523
    height: 506
  color: '#e6e6e5'
---

## Your car is a smartphone on wheels. Here's who's listening.

The first large-scale measurement study of the connected-vehicle ecosystem.

A **connected car** is a vehicle with built-in internet access — Wi-Fi, cellular, GPS — that lets it communicate constantly with its manufacturer and outside companies. **Over 75% of vehicles sold globally have this connectity built-in.**

![A connected vehicle](https://automatictransmission.khoury.northeastern.edu/images/test-facility.png)

Photo is not loading`images/connected-car.jpg`

It knows where you drive.

It knows who you are.

It can share this data and more with insurance companies, advertisers, etc...

Report Summary

Partnering with Consumer Reports, we tested 21 late model vehicles and 30 companion mobile apps to understand the privacy implications of the connected vehicle ecosystem.

- 01We find that both vehicles and companion apps contact numerous third-party domains, including advertisers and trackers.
- 0219/21 vehicles tested send traffic to at least one third party
- 03Seven of 30 apps transmit sensitive identifiers to third-party companies

We go through a lengthy discolure process and provide insight into how manufacturer's perceive this data sharing issue. \
**Our findings underscore the need for continued measurement and scrutiny of the connected vehicle ecosystem.**

#### Partnership with Consumer Reports

Consumer Reports gave the team access to its purchased fleet of test vehicles — a sample that would have cost over $1.2M to assemble independently.

#### The Paper

This work is peer reviewed and will be published at IMC '26. [Read the paper →](https://automatictransmission.khoury.northeastern.edu/paper.html)

#### The Research Team

We are a team of privacy, security, and networking-systems researchers at Northeastern University. [See the full team →](https://automatictransmission.khoury.northeastern.edu/authors.html)

21

vehicles tested,\
19 brands

30

companion \
 mobile apps

19 / 21

vehicles contacted a\
third party over Wi-Fi

7 / 30

apps sent PII to trackers

5 / 30

apps sent VIN + other\
PII to trackers

## Research Questions

![Diagram of how the connected vehcile ecosystem works](https://automatictransmission.khoury.northeastern.edu/images/car-diagram.png)

Photo is not loading `images/car-diagram.png`

#### Connected Vehicle Ecosystem

This diagram shows the data flows to and from a vehicle and its companion mobile app. Both devices send data, including private consumer data, to different 1st and 3rd party servers using Wi-Fi and cellular service. Solid arrows represent flows that were intercepted through our experiments.

#### The Problem

Once the data gets sent to these servers, it is up to the companies that receive the consumer information to make decisions on what they do with it. Unfortunately, in many cases, this includes sharing or selling consumer data to other undisclosed 3rd parties. \
 **Consumers have no control over their data once it has left their device**

In this paper, we take the first steps to address the limited visibility into the privacy implications of the connected vehicle ecosystem. We identify two vantage points in the ecosystem where we can gain insight into the data that connected vehicles are sharing with both manufacturers and third parties: the vehicles themselves and the mobile apps provided by manufacturers. \
 We ask the following guiding questions:

- 01What personal consumer data do connected vehicles and their companion mobile apps transmit?
- 02Who receives that personal consumer data?
- 03What is the manufacturer response to these findings?

## Methods

We investigated 21 vehicles from the U.S. market in a controlled environment along with 30 companion mobile apps instrumented with on-site vehicles between October 2024 and August 2025. Below is a description of the experiments we ran and our setup.

### Vehicle Testing

![Testing Setup](https://automatictransmission.khoury.northeastern.edu/images/rpi.png)

Photo is not loading `images/rpi.png`

#### Wi-Fi Testing Setup

To collect Wi-Fi traffic from vehicles, we configured a custom access point (AP) on a Raspberry Pi and used tcpdump to log all packets that were sent or received via this AP. \
 This allowed us to see all the destinations the vehicles were sending data to but not the information within the packets as it was encrypted.

![Vehicles at the testing facility](https://automatictransmission.khoury.northeastern.edu/images/test-facility.png)

Photo is not loading `images/test-facility.jpg`

#### Stationary Tests

Idle Baseline\
vehicle on — no activity

Active Test\
perform all possible actions

#### Driving Test

drive 5–45 mph with acceleration & hard braking

#### Isolating Cellular Traffic

![Faraday tent used to block cellular signals](https://automatictransmission.khoury.northeastern.edu/images/tent-in-use.png)

Photo is not loading `images/faraday-tent.jpg`

One hypothesis we tested was whether blocking a vehicle’s ability to communicate over its cellular network would force more Wi-Fi communication. To block external cellular signals, we drove 11 EVs in the sample into a car-sized Faraday tent providing ≈93 dB of attenuation, blocking their cellular connection entirely. Stationary Idle and Active tests were repeated inside the tent to see whether traffic that normally goes out over cellular rerouted to Wi-Fi instead.

### App Testing

In total, we experimented with 30 connected vehicle companion apps that were paired with the vehicles at Consumer Reports’s testing facility.

#### Device Setup

- 01We used a combination of test phones and iOS versions for our experiments: an iPhone 8/iOS 16.6, an iPhone X/iOS 16.7.11, and an iPhone 13/iOS 18.5.
- 02To minimize background traffic, we deleted all non-essential apps on the test phones and tested apps one-by-one, including deleting each vehicle app and restarting the phone before downloading the next app.
- 03We used the iOS native screen recording feature to record our interactions with each app for later review.
- 04To capture and decrypt network traffic from the companion mobile apps, we used iPhones with custom root certificates connected to mitmproxy.

#### Process for Testing Each App

- 01During app installation and login we accepted all permission requests (e.g., tracking, location, calendar access, Bluetooth, notifications) that the application requested
- 02We had a Consumer Reports employee log into the app using their existing credentials associated with a vehicle on the lot.
- 03Once we were logged-in, we manually exercised all available functionality, such as looking for nearby charging staions, geolocating the vehicle, viewing vehicle data and service history (e.g., tire pressure), viewing notifications (e.g., “doors are unlocked”), and viewing in-app privacy policies.
- 04Some apps allowed us to perform physical interactions on the vehicle, such as remotely opening the trunk. We performed all such actions and verified that the vehicle completed each request.

## Vehicle Dataset

$1.2M+

estimated cost to independently procure this fleet, only possible due to Consumer Reports partnership

| Manufacturer                       | Brand      | Year                | Model                | Vehicle Tests |   |   |   |
| ---------------------------------- | ---------- | ------------------- | -------------------- | ------------- | - | - | - |
| General Motors (GM)                | Buick      | 2024                | Envista              | ✓             | ✓ | – | – |
| Cadillac                           | 2024       | Lyriq               | ✓                    | ✓             | ✓ | – |   |
| Chevrolet                          | 2024       | Blazer              | ✓                    | ✓             | ✓ | – |   |
| Stellantis                         | Dodge      | 2023                | Hornet               | -             | ✓ | – | – |
| Fiat                               | 2024       | 500e                | ✓                    | ✓             | ✓ | – |   |
| RAM                                | 2025       | 1500 Bighorn        | ✓                    | ✓             | – | – |   |
| Fisker                             | Fisker     | 2023                | Ocean                | ✓             | ✓ | - | – |
| Ford Motor Co.                     | Ford       | 2022                | F150 Lightning       | ✓             | ✓ | - | – |
| Ford                               | 2024       | Mustang GT Fastback | ✓                    | ✓             | – | – |   |
| Honda Motor Co.                    | Honda      | 2024                | Prologue Touring AWD | ✓             | ✓ | ✓ | – |
| Tata Motors                        | Land Rover | 2023                | Range Rover Sport    | ✓             | ✓ | – | – |
| Toyota Motor Corp.                 | Lexus      | 2024                | NX450H+ PHEV         | ✓             | ✓ | – | – |
| Toyota                             | 2023       | Corolla Cross       | ✓                    | ✓             | – | – |   |
| Subaru                             | 2023       | Solterra            | ✓                    | ✓             | ✓ | – |   |
| Lucid                              | Lucid      | 2023                | Air Touring          | ✓             | ✓ | ✓ | – |
| Mercedes-Benz Group AG             | Mercedes   | 2023                | EQS450 4Matic        | ✓             | ✓ | - | – |
| Renault-Nissan-Mitsubishi Alliance | Nissan     | 2023                | Ariya Platinum       | ✓             | ✓ | ✓ | – |
| Rivian                             | Rivian     | 2022                | R1S                  | ✓             | ✓ | ✓ | – |
| Tesla                              | Tesla      | 2024                | Cybertruck           | ✓             | ✓ | ✓ | – |
| Tesla                              | 2024       | Model 3             | ✓                    | ✓             | ✓ | ✓ |   |
| Zhejiang Geely Holding Group       | Volvo      | 2024                | C40                  | ✓             | ✓ | ✓ | – |

**\***While we tested a wide range of vehicles, we did not cover every manufacturer in the U.S. market. As with all empirical studies, our results should be interpreted as a snapshot in time, and may not generalize to vehicles outside our sample or outside the U.S.

## Findings

- **19 of 21 vehicles** contacted at least one third party over Wi-Fi, including known advertising and tracking domains
- **7 of 30 companion apps** transmitted sensitive identifiers (VINs, emails, phone numbers, precise location) to third parties associated with advertising and tracking.\
   Transmiting multiple forms of PII to the same third party allows advertisers to build in-depth profiles on consumers

| App | VIN | Location | Phone | Email |
| --- | --- | -------- | ----- | ----- |

Click any company chip for what it received and from which app.

-
- Pairing a companion app **roughly doubled** a vehicle's exposure to advertising/tracking companies on average, and in some cases added 20+ new ones.

 vehicle-only ATA companies added by the companion mobile app

## Manufacturer Disclosures

- The team disclosed these findings to the different manufacturers featured in the study, below is an interactve summary of the different explanations received.

Click any box above for more info.

- The ongoing theme of all these responses was **shifting the blame to the consumer**.
- **The current system does not give owners the ability to choose.** \
   Assuming owners can find the particular agreements, if an owner decides they are uncomfortable with the data sharing described within them, they are faced with (arguably) unfair choices: \
   01accept the agreements regardless of their concerns\
   02stop using their car's connected-vehicle features - which includes remote start, the app, and other very useful features\
   03stop using the vehicle entirely

- As one notable exception, **Honda** improved its data collection practices to prevent sending precise geolocation to a third party associated with user tracking.

## Conclusion

- We found that **vehicles, under a variety of real-world settings, contact** not only a wide range of first parties (i.e., manufacturer domains) and car-specific support parties, but also **third parties that are known to provide advertising and tracking services.**
- Vehicles from the same manufacturer exhibit different network behaviors - this makes it very difficult and expensive to study these vehicles.
- When adding vehicle companion apps to the analysis, we found that vehicle owners are exposed to even more privacy-sensitive ATA communication—in some cases more than two dozen additional trackers.
- Our study revealed a **large gap between what vehicle manufacturers publicly disclosed and how the connected vehicle ecosystem actually shares data over the Internet**
- **Based on the opaque nature of vehicular systems, we argue that there is a need for better transparency to ensure increased visibility into the entire ecosystem to identify and address corresponding harms.**
