# Energy-26-18828_input_output_data

Input and output data for the manuscript *Optimization of Building Valve Control in District
Heating Secondary Networks Based on Heterogeneous Graph Neural Network via Reinforcement
Learning*.

## Structure

| Directory | Content |
|---|---|
| `instances/Training_instances/` | 24 training instances, 8 for each network scale |
| `instances/Testing_instances/` | 6 test instances, 2 for each network scale |
| `temperature_data.xlsx` | Outdoor temperature and setpoint profiles for the 80 training days and the 7 test days |
| `heuristic/` | Results of Fix_100, DE, GA and PSO |
| `ablation/` | Results of the ablation variants and of the network-depth study |
| `mpc/` | Results of the model predictive controller |
| `td3/` | Results of the TD3 controller |

## Result files

Every result workbook contains two sheets.

* `Daily_Summary` — one row per test day, with `temp_target` (Obj1, °C²), `Pump_Energy` (kWh),
  `Heat_Loss` (kWh, overheating), `energy_target` (Obj2, kWh) and the computation time per
  control step.
* `Hourly_Building_Detail` — one row per building and per hour, with the actual and setpoint
  indoor temperature, the valve opening (%), the building flow rate and the energy components.

## Baseline configurations

* `mpc/` — prediction horizon 3 h, decision variables 3 × _n_<sub>b</sub>, Powell optimiser with
  250 function evaluations, valve openings bounded to 50–100 %, objective weights 1:1. The
  controller is provided with the exact model of the plant; the only uncertainty is the outdoor
  temperature forecast, for which a 5 % relative error is assumed with an AR(1) correlation of
  0.8 and a standard deviation growing as the square root of the lead time.
* `td3/` — deterministic actor sharing the heterogeneous graph encoder of the proposed method,
  10<sup>6</sup> environment steps, replay buffer of 5 × 10<sup>4</sup> samples per network scale,
  Gaussian exploration noise of standard deviation 0.15, learning rate 3 × 10<sup>-4</sup>,
  target update rate τ = 0.005, target policy smoothing σ = 0.2 clipped to ±0.5, policy update
  delay 2. Evaluation is deterministic.
