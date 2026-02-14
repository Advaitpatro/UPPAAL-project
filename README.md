
# UPPAAL Model Project (Traffic Intersection + Emergency Priority)

This repository contains an **UPPAAL** timed-automata model for a two-direction intersection (North–South and East–West) with:
- Traffic light phases (green/yellow/red with all-red clearance)
- Vehicle + pedestrian queues per direction
- A controller that switches phases with timing constraints
- Emergency vehicle arrival/clear handling (priority/preemption logic)
- An environment process that generates arrivals/crossing events

## Files

- `Final g (1).xml` — Main UPPAAL model (`nta`) you can open directly in UPPAAL.
- `tracetest (2).xtr` — Saved trace file (optional; useful for reproducing/inspecting a simulation run depending on your UPPAAL setup).

> If you want cleaner names, you can rename them to:
> - `uppaal_model.xml`
> - `uppaal_trace.xtr`

## Requirements

- **UPPAAL 4.x** (GUI or CLI)

## How to Open (UPPAAL GUI)

1. Launch UPPAAL.
2. **File → Open…**
3. Select your model XML (e.g., `Final g (1).xml` or `uppaal_model.xml`).
4. Use:
   - **Simulator** tab to run simulations.
   - **Verifier** tab to check properties/queries.

## Model Structure (High Level)

The model instantiates templates similar to:
- `TrafficLight(NS)`, `TrafficLight(EW)`
- `VehicleQueue(NS)`, `VehicleQueue(EW)`
- `PedestrianQueue(NS)`, `PedestrianQueue(EW)`
- `EmergencyVehicle(NS)`, `EmergencyVehicle(EW)`
- `Controller()`
- `Environment()`

## Key Timing/Capacity Parameters

Typical parameters declared in the model:
- `T_MAX_GREEN` — max green duration
- `T_MIN_GREEN` — min green duration
- `T_YELLOW` — yellow time
- `T_ALL_RED` — all-red clearance time
- `T_CROSS` — crossing time
- `MAX_QUEUE` — queue capacity bound

Edit these in the **Global Declarations** section in the UPPAAL XML.

## Channels / Events

The model uses broadcast channels for coordination, typically including:
- Light switching: `switch_green[id]`, `switch_yellow[id]`, `switch_red[id]`
- Vehicles/pedestrians: `arrive_car[id]`, `cross_car[id]`, `arrive_ped[id]`, `cross_ped[id]`
- Emergency: `emg_arrive[id]`, `emg_clear[id]`

## Suggested Verification Queries (Examples)

Add these in UPPAAL’s **Verifier** tab (adjust names if your model uses different variables):

- No deadlock:
  - `A[] not deadlock`

- Queue bounds:
  - `A[] (q_car[0] <= MAX_QUEUE and q_car[1] <= MAX_QUEUE)`
  - `A[] (q_ped[0] <= MAX_QUEUE and q_ped[1] <= MAX_QUEUE)`

- Mutual exclusion for green:
  - `A[] not (ns_is_green and ew_is_green)`

## License

Add a license file (MIT/Apache-2.0/etc.) if you plan to publish publicly.
