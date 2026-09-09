# RET — Timing Evidence Report

**Team:** William Camilo Obando Cardenas · **Boards:** ESP32 DevKit V1 · native_sim · **Living** document: updated every week; handed in at the workshop (week 8) and at the close (week 16). House rule: "show me the trace" — every timing claim cites a measurement.

## 1. The system and its task set

Requirements first — one sentence each, EARS style (*when/while <condition>, the system shall <response> within <deadline>*).

| ID          | Requirement                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------- |
| REQ-CTRL-01 | While the system is irrigating, the control loop shall run every 10 ms (deadline = period). |

Initial Control task set from the SoilSense Control scenario:

| Task                        | Req.        | Type (H/F/S) | Period | Deadline | Measured C_i | How it was measured       |
| --------------------------- | ----------- | ------------ | ------ | -------- | ------------ | ------------------------- |
| Flow/pressure control loop  | REQ-CTRL-01 | Hard         | 10 ms  | = T      | ____         | <GPIO + analyzer / trace> |
| Overpressure emergency stop | REQ-CTRL-02 | Hard         | ____   | < 5 ms   | ____         | <GPIO + analyzer / trace> |
| Sensor sampling             | REQ-SENS-01 | Hard         | 1 ms   | ____     | ____         | <GPIO + analyzer / trace> |
| Telemetry to the Hub        | REQ-TEL-01  | Soft         | ____   | ____     | ____         | <trace>                   |
| Command console             | REQ-CMD-01  | Firm         | ____   | ____     | ____         | <trace>                   |

> Note: Only the control-loop timing values are specified for Week 1. The remaining task parameters and measured execution times will be completed in the corresponding weeks/modules.

## 2. ADRs

### ADR-001 — Week 1 development platform

**Context:** The Week 1 environment must be verified using Zephyr, a physical development board, and `native_sim`.

**Decision:** Use the ESP32 DevKit V1 as the available physical development platform and `native_sim` for simulation.

**Justification (with numbers):** The ESP32 DevKit V1 was successfully built and flashed with Zephyr. The physical LED executed the Blinky application. The serial console was verified at 115200 baud, and the modified application message was observed on the ESP32. The `native_sim` target successfully executed the `hello_world` application.

**Status:** Accepted for Week 1 bring-up.

## 3. Evidence by week

Each entry cites the REQ(s) it verifies.

### Week 1 — toolchain and environment bring-up

Evidence collected:

* Zephyr `blinky` successfully compiled and flashed to the ESP32 DevKit V1.
* Physical LED on the ESP32 DevKit V1 was observed blinking.
* Zephyr `hello_world` successfully executed on `native_sim`.
* Zephyr `hello_world` was successfully compiled and flashed to the ESP32 DevKit V1.
* Serial communication was verified at 115200 baud.
* The modified serial message was successfully observed on the ESP32.

### Week 2 — superloop baseline (C0116-DK)

<jitter/latency table + a one-sentence reading>

### Week 3 — S3 baseline and silicon comparison

…

## 4. Schedulability analysis

U = ΣC_i/T_i with measured C_i; test used (RM / hyperbolic / EDF); RTA as a script with its output; blocking B_i if there are mutexes.

(Formulas: READINGS.md, "The math that does get used".)

## 5. Functional safety (final project)

Declared safe state (failure ⇒ valve closed), watchdog, and the evidence of the fail-safe firing.
