# 🚌 Verilog Bus Ticketing System

This repository contains the Verilog Hardware Description Language (HDL) implementation and a comprehensive testbench for a digital **Bus Ticketing System**.

The system is designed to simulate two primary functions:
1.  **Fare Calculation:** Determining the ticket cost based on the boarding and exit points.
2.  **Seat Management:** Tracking booked seats and indicating seat availability based on a fixed capacity.

---

## 📂 Repository Contents

| File Name | Description |
| :--- | :--- |
| `BusTicketingSystem.v` | The main Verilog module (DUT - Design Under Test). Implements fare calculation and seat booking logic. |
| `tb_bus_ticketing_system.v` | The testbench for simulating and verifying the `BusTicketingSystem` module. |
| `README.md` | This file. |

---

## ⚙️ System Design and Parameters

The system is defined by the following key parameters and logic:

### 1. Fare Calculation Logic

The fare is calculated in a continuous assignment block (`always @(*)`) and is based on the distance traveled (difference between exit and boarding points).

* **Fare Rate:** Defined by the `FARE_RATE` parameter.
    $$
    \text{Fare} = (\text{Exit Point} - \text{Boarding Point}) \times \text{FARE\_RATE}
    $$
* **Invalid Trip:** If `exit_point <= boarding_point`, the fare is set to **0**.
* **`FARE_RATE` Parameter:** Set to **10** (currency units per stop).

### 2. Seat Management Logic

Seat booking is handled by a sequential block (`always @(posedge seat_request)`) and is triggered by the `seat_request` signal.

* **Total Capacity:** Defined by the `SEAT_CAPACITY` parameter.
* **Booking:** If the number of `booked_seats` is less than `SEAT_CAPACITY`, a seat is booked (`booked_seats` increments by 1).
* **Availability Output:** The `seat_available` output indicates if there is **still capacity remaining** after the booking attempt.
* **`SEAT_CAPACITY` Parameter:** Set to **6** (total seats).

---

## ▶️ Simulation and Verification

The `tb_bus_ticketing_system.v` file includes several test cases to verify both the fare calculation and the seat management logic.

### Test Cases Included:

1.  **Valid Fare Calculations:** (e.g., Boarding 2 to Exit 5).
2.  **Invalid Fare Calculations:** (e.g., Boarding = Exit, or Boarding > Exit).
3.  **Maximum Range Test:** (Boarding 0 to Exit 15).
4.  **Booking Success/Failure:** Tests booking attempts up to and beyond the `SEAT_CAPACITY` limit of 6.

### Running the Simulation

This project can be simulated using any standard Verilog simulator (e.g., Icarus Verilog, ModelSim, Riviera-PRO).

**Example using a command-line flow (assuming files are named `BusTicketingSystem.v` and `tb_bus_ticketing_system.v`):**

```bash
# Compile the design and the testbench
iverilog -o bus_sim BusTicketingSystem.v tb_bus_ticketing_system.v

# Run the simulation
vvp bus_sim
