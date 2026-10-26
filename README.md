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

## Sensitivity to the objective weighting

`weight_sensitivity/` contains the results obtained when the weighting of the two sub-objectives
is changed. The folder is organised by method and by weighting,

```
weight_sensitivity/{ppo,mpc,ga}/{w1-2,w2-1}/
```

where `w1-2` denotes ω₁:ω₂ = 1:2 and `w2-1` denotes ω₁:ω₂ = 2:1, both with ω₁ + ω₂ = 2 held
constant so that the weighted objectives are directly comparable with the 1:1 case. The
corresponding 1:1 results are already in `ablation/paper_hgnn_1hop_seed42/` for PPO, in `mpc/`
for the predictive controller and in `heuristic/GA/` for GA.

* `ppo/` — the proposed method retrained from scratch at each weighting (10<sup>6</sup> steps,
  seed 42) and evaluated at the same weighting.
* `mpc/` — the predictive controller re-optimised at each weighting; the weights enter the
  objective of the optimiser directly.
* `ga/` — GA re-optimised at each weighting.

For the proposed method, the cost of not retraining can be reproduced by scoring the 1:1 policy
in `ablation/paper_hgnn_1hop_seed42/` with the other two weightings, i.e. by computing
0.5·Obj1 + 0.5·Obj2 for the 1:1 case and ω₁·Obj1 + ω₂·Obj2 for the other two.
