# 🚇 Metro Ticket Generation System in ServiceNow

A ServiceNow-based digital metro ticketing system designed to simplify ticket booking, automate fare calculation, and generate digital QR-based tickets through a self-service workflow.

The project is built as a **Custom Scoped Application** using ServiceNow Studio and combines Service Catalog, Catalog Client Scripts, UI Policies, custom tables, and Service Portal components to provide a dynamic metro ticket booking experience.

---

## 📌 Project Overview

The **Metro Ticket Generation System** provides passengers with a self-service method to enter their journey details, calculate the applicable fare automatically, and generate a digital ticket with a QR code.

The system uses a custom Metro Station table to store station information and uses client-side scripting to determine the fare based on the selected source, destination, number of passengers, and type of journey.

The project also includes dynamic form behavior through UI Policies and QR-based digital ticket generation using Service Portal components.

---

## ✨ Key Features

* **Self-Service Metro Booking:** Passengers can submit metro ticket requests through a Service Catalog item.

* **Source & Destination Selection:** Starting and destination stations are selected from the custom Metro Station table.

* **Dynamic Fare Calculation:** Fare is calculated automatically based on the number of stops between the selected stations.

* **Passenger-Based Pricing:** The calculated fare is multiplied according to the selected number of passengers.

* **Single & Return Journey Support:** The system supports both single and return journeys, with return journeys applying the appropriate fare multiplier.

* **Dynamic Form Behaviour:** UI Policies control the visibility of the single-journey and return-journey amount fields based on the selected journey type.

* **Input Validation:** The system prevents passengers from selecting the same station as both the starting and destination station.

* **QR-Based Digital Ticket:** A QR code is generated for the digital ticket using Service Portal components.

* **Custom Metro Station Database:** Station information is maintained in a custom scoped application table.

* **Service Portal Compatibility:** Catalog Client Scripts are configured with UI Type **All** to support the required interfaces.

---

## 🔄 System Workflow

```text
Passenger
    │
    ▼
Service Portal / Service Catalog
    │
    ▼
Enter Journey Details
    │
    ├── Starting Station
    ├── Destination Station
    ├── Number of Passengers
    ├── Type of Journey
    └── Payment Mode
    │
    ▼
Catalog Client Scripts
    │
    ├── Validate Source & Destination
    ├── Determine Number of Stops
    ├── Calculate Fare
    └── Apply Passenger / Return Fare
    │
    ▼
UI Policies
    │
    ├── Single Journey Amount
    └── Return Journey Amount
    │
    ▼
Ticket Generation
    │
    ▼
QR Code Generation
    │
    ▼
Digital Metro Ticket
```

---

## 🗄️ Data Model

The project uses a custom Metro Station table within the scoped application.

**Custom Table:**

`x_2169755_metro_0_metro_station_s_details`

**Table Purpose:**

Stores the metro station information used by the ticket booking form.

**Custom Field:**

`station_name`

**Sample Station Records:**

* Vyttila
* MG Road
* Kaloor
* Edapally
* Aluva

---

## 📝 Service Catalog Variables

The Metro Ticket Booking catalog item uses the following variables:

| Variable                    | Type                      | Purpose                         |
| --------------------------- | ------------------------- | ------------------------------- |
| `starting_from`             | Choice / Record Reference | Select starting station         |
| `going_to`                  | Choice / Record Reference | Select destination station      |
| `enter_payment_mode`        | Single-line Text          | Enter payment information       |
| `no_of_passengers`          | Choice / Dropdown         | Select number of passengers     |
| `type_of_journey`           | Choice / Radio            | Select Single or Return journey |
| `amount_for_single_journey` | Single-line Text          | Displays single journey fare    |
| `amount_including_return`   | Single-line Text          | Displays return journey fare    |
| `mode_of_paymnet`           | Choice / Radio            | Select payment mode             |

---

## ⚙️ Fare Calculation Logic

The fare calculation is handled using **Catalog Client Scripts**.

Instead of repeatedly querying the database, the script uses:

```javascript
g_form.getDisplayValue()
```

to obtain the selected station values directly from the form.

The five stations are mapped to sequential index values:

```text
Vyttila   → 0
MG Road   → 1
Kaloor    → 2
Edapally  → 3
Aluva     → 4
```

The system calculates the distance between the selected stations by finding the difference between their index positions.

The fare is then assigned according to the number of stops:

```text
Fare Tier → Fare

Tier 1 → ₹20
Tier 2 → ₹30
Tier 3 → ₹40
Tier 4 → ₹50
```

The calculated fare is then multiplied by the selected number of passengers.

For a return journey, the applicable fare is multiplied by **2**.

The resulting amount is automatically populated into the appropriate fare field.

---

## 🧩 Client Scripts & UI Policies

The project uses four `onChange` Catalog Client Scripts.

They respond to changes in:

* Starting station
* Destination station
* Number of passengers
* Type of journey

The scripts are configured with **UI Type: All** to maintain compatibility with the required ServiceNow interfaces.

### UI Policies

UI Policies dynamically control the fare fields according to the selected journey type.

* `amount_for_single_journey` is displayed for a single journey.
* `amount_including_return` is displayed for a return journey.

This prevents unnecessary fields from being displayed to the passenger.

---

## 🎫 QR Code Digital Ticket

The system also supports digital ticket generation using a QR-based ticket interface.

After the booking information is processed, the ticket information can be presented digitally along with a generated QR code.

Service Portal components / widgets are used to support the QR ticket experience and provide a more convenient alternative to a paper-based ticket.

The QR-based approach also provides a foundation for future ticket verification functionality.

---

## 🛠️ Technology Stack

**Platform:** ServiceNow

**Application Type:** Custom Scoped Application

**Development Environment:** ServiceNow Studio

**Interface:** Service Catalog / Service Portal

**Automation & Form Logic:**

* Catalog Client Scripts
* UI Policies

**Frontend / Portal Components:**

* Service Portal Widgets
* VA Widgets
* Client-side scripting

**Database:**

* Custom ServiceNow Table
* Metro Station records

**Languages / Scripting:**

* JavaScript

---

## 🔮 Future Scope

The current system can be extended with additional metro transportation features, including:

* UPI and online payment gateway integration
* Automated email/SMS/WhatsApp ticket delivery
* Ticket cancellation and refund workflow
* Advanced ticket verification for station staff
* Daily/monthly metro pass management
* Dynamic fare configuration instead of hard-coded fare tiers
* Passenger travel analytics
* Peak-hour and route analysis
* Mobile-friendly digital ticket verification

---

## 👨‍💻 Author

**Amal Krishna J**

BTech Artificial Intelligence & Data Science

ServiceNow Developer | CAD & CSA Certified

---

## 📌 Project Summary

The **Metro Ticket Generation System in ServiceNow** demonstrates how ServiceNow can be used to build a self-service digital ticketing solution using low-code configuration combined with JavaScript-based client-side logic.

The project integrates **custom data storage, Service Catalog, dynamic form behaviour, automated fare calculation, Service Portal components, and QR-based digital ticket generation** into a single metro ticketing workflow.
