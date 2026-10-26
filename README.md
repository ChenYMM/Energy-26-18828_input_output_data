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
| `weight_sensitivity/` | Results obtained with the alternative weightings of the two sub-objectives |

## Result files

Every result workbook contains two sheets.

* `Daily_Summary` — one row per test day, with `temp_target` (Obj1, °C²), `Pump_Energy` (kWh),
  `Heat_Loss` (kWh, overheating), `energy_target` (Obj2, kWh) and the computation time per
  control step.
* `Hourly_Building_Detail` — one row per building and per hour, with the actual and setpoint
  indoor temperature, the valve opening (%), the building flow rate and the energy components.

## Folder names in `weight_sensitivity/`

The results are grouped by method and by weighting, `weight_sensitivity/{ppo,mpc,ga}/{w1-2,w2-1}/`,
where `w1-2` and `w2-1` denote ω₁:ω₂ = 1:2 and ω₁:ω₂ = 2:1, respectively. The results obtained
with ω₁:ω₂ = 1:1 are those in `ablation/paper_hgnn_1hop_seed42/` for PPO, in `mpc/` for the
predictive controller and in `heuristic/GA/` for GA.
