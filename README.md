<!-- title: QM Air Quality Network Data -->
# QM Air Quality Network Data

Fourteen months (**16 April 2025 to 10 June 2026**) of raw air-quality
measurements from the **QMUL Air Quality Monitoring System (AQMS)**: seven
low-cost IoT nodes deployed at Queen Mary University of London (Mile End
campus) and King Edward Memorial Park, London, together with the co-located
records of two Tower Hamlets reference stations.

The stations themselves (hardware design and firmware) are documented at
**[qm-airquality-network](https://github.com/AndresMercad0/qm-airquality-network)**.

**Source:** raw wide-format CSVs exported from the QMUL IoT Lab InfluxDB
server (low-cost nodes 1 to 7) and the LondonAir API (reference stations TH2
and TH7). No resampling, calibration or gap-filling has been applied; data is
preserved at its original collection frequency.

**Contact:** Andres A. Mercado-Velazquez (a.mercadovelazquez@qmul.ac.uk)

---

## Data files

One CSV per node or station, covering the full period 16 April 2025 to 10 June
2026. The files are stored with **Git LFS** because each node file is large.

| File | Node | Sensor tier | Pollutants and signals |
|---|---|---|---|
| `data/node_1.csv` | node_1 | Low | NO2, CO, O3, PM2.5, PM10, VOC, T, RH |
| `data/node_2.csv` | node_2 | Low | NO2, CO, O3, PM2.5, PM10, VOC, T, RH |
| `data/node_3.csv` | node_3 | Low + mid | the low-cost set, plus mid-cost NO2, O3, CO |
| `data/node_4.csv` | node_4 | Low | NO2, CO, O3, PM2.5, PM10, VOC, T, RH |
| `data/node_5.csv` | node_5 | Low + mid | the low-cost set, plus mid-cost NO2, O3, CO |
| `data/node_6.csv` | node_6 | Low | NO2, CO, O3, PM2.5, PM10, VOC, T, RH |
| `data/node_7.csv` | node_7 | Low | NO2, CO, O3, PM2.5, PM10, VOC, T, RH |
| `data/reference/node_TH2_MER.csv` | node_TH2_MER | Reference | NO2, PM2.5 |
| `data/reference/node_TH7_KEMP.csv` | node_TH7_KEMP | Reference | NO2, PM2.5 |

To work with the files you need Git LFS installed (`git lfs install`) before
cloning; otherwise the CSVs come down as small pointer files.

---

## Study locations

| NodeID | Site | Subsite | Exposure | Environment | Latitude | Longitude |
|---|---|---|---|---|---|---|
| node_1 | Queen Mary University of London | Peter Landin Building office (window) | Outdoor | Pedestrian open space | 51.523158 | -0.042900 |
| node_2 | Queen Mary University of London | Peter Landin Building office (room) | Indoor | Natural ventilation | 51.523176 | -0.042907 |
| node_3 | Queen Mary University of London | Mile End Road | Outdoor | Roadside, urban traffic | 51.522530 | -0.042155 |
| node_4 | Queen Mary University of London | Engineering Building (adjacent room to Mile End Road) | Indoor | Mechanical ventilation | 51.522586 | -0.042202 |
| node_5 | King Edward Memorial Park | Glamis Rd & A1203 | Outdoor | Roadside, urban traffic | 51.509477 | -0.050541 |
| node_6 | Queen Mary University of London | Engineering Building (window) then People's Palace (window) | Outdoor | Pedestrian open space | 51.523036 then 51.523179 | -0.041493 then -0.041315 |
| node_7 | Queen Mary University of London | Engineering Building (room) then People's Palace (room) | Indoor | Mixed then mechanical ventilation | 51.523042 then 51.523192 | -0.041542 then -0.041298 |
| node_TH2_MER | Queen Mary University of London | Mile End Road | Outdoor | Roadside, urban traffic | 51.522530 | -0.042155 |
| node_TH7_KEMP | King Edward Memorial Park | Glamis Rd & A1203 | Outdoor | Roadside, urban traffic | 51.509477 | -0.050541 |

Nodes 6 and 7 were relocated between 16 and 19 June 2025; see
[Node relocation event](#node-relocation-event).

### Indoor-outdoor pairs

| Pair | Outdoor node | Indoor node | Location |
|---|---|---|---|
| 1-2 | node_1 | node_2 | Peter Landin Building |
| 3-4 | node_3 | node_4 | Mile End Road / Engineering Building |
| 6-7 | node_6 | node_7 | Engineering Building then People's Palace |

### Co-location with reference stations

| Low-cost node | Reference station | Location |
|---|---|---|
| node_3 | node_TH2_MER (Tower Hamlets TH2) | QMUL Mile End Road |
| node_5 | node_TH7_KEMP (Tower Hamlets TH7) | King Edward Memorial Park |

The reference-station coordinates are asserted as co-located with node_3 and
node_5 rather than being the exact borough station coordinates.

---

## Sensor categories

### Low-cost sensors (all seven nodes)

| Sensor | Manufacturer | Pollutant / signal | Sampling |
|---|---|---|---|
| SHT45 | Sensirion AG | Temperature, humidity | 30 s |
| SGP40 | Sensirion AG | VOC (raw + index) | 30 s |
| SPS30 | Sensirion AG | PM2.5, PM10 | 30 s |
| GM-702B and GM-102B (on the Grove Multichannel Gas v2 board) | Zhengzhou Winsen Electronics (sensors); Seeed Studio (carrier board) | CO, NO2 (raw) | 30 s |
| MQ131, low concentration (on an FC-22 board, via ADS1115) | Zhengzhou Winsen Electronics | O3 (raw + voltage) | 30 s |

### Mid-cost sensors (nodes 3 and 5 only)

Alphasense Ltd B-series electrochemical cells on Individual Sensor Boards
(ISB), read through Texas Instruments ADS1115 ADCs: NO2-B43F, OX-B431
(oxidising, O3 + NO2) and CO-B4. Each cell reports its working and auxiliary
electrodes (OP1, OP2) as raw ADC counts and volts.

### Reference stations (TH2, TH7)

Tower Hamlets borough stations of the London Air Quality Network (LAQN),
reporting NO2 and PM2.5 in ug/m3 at 15-minute resolution. PM2.5 is measured
by a Met One BAM 1020 (beta attenuation); NO2 by a chemiluminescence
analyser (LAQN/AURN specification). Note: the `_met_one_` fragment in the
NO2 column name is historical naming from the internal pipeline; the NO2
instrument is not a Met One device.

---

## Column descriptions

### Metadata columns (all files)

| Column | Description |
|---|---|
| `timestamp_utc` | Timestamp in UTC, ISO 8601 (YYYY-MM-DDTHH:MM:SSZ) |
| `node_id` | Node identifier (e.g. node_3) |
| `site` | Deployment site |
| `subsite` | Specific location within the site |
| `exposure` | Indoor or Outdoor |
| `environment` | Microenvironment description |
| `latitude`, `longitude` | Coordinates (WGS84) |

### Measurement columns, low-cost nodes

`no2_low_mgsv2_raw`, `co_low_mgsv2_raw` (MGSv2 raw); `o3_low_mq131_raw`,
`o3_low_mq131_voltage` (MQ131); `temperature_low_sht45_celsius`,
`humidity_low_sht45_percentage` (SHT45); `voc_low_sgp40_comp_raw`,
`voc_low_sgp40_index` (SGP40); `pm2_5_low_sps30_ug_m3`,
`pm10_low_sps30_ug_m3` (SPS30).

### Measurement columns, mid-cost (nodes 3 and 5, empty elsewhere)

For each of NO2, O3 and CO: `*_mid_alphasense_op1_raw`,
`*_mid_alphasense_op1_voltage`, `*_mid_alphasense_op2_raw`,
`*_mid_alphasense_op2_voltage`.

### Measurement columns, reference stations

`no2_high_met_one_ug_m3`, `pm2_5_high_met_one_ug_m3` (ug/m3).

---

## Node relocation event

Nodes 6 and 7 were relocated between **16 and 19 June 2025** due to
institutional space repurposing at QMUL, moving from the Engineering Building
to the People's Palace. The per-row coordinates reflect the position at the
time of each measurement. For node_6 (outdoor, pedestrian open space in both
locations) the change is minimal and a unified analysis with a relocation
indicator is reasonable; for node_7 (indoor) the microenvironment changed
more substantially, so a segmented analysis is recommended.

---

## Data quality and processing notes

- **Raw and uncalibrated.** Gas readings are in sensor-native units (ADC
  counts, volts, index values), not concentrations. Calibration against the
  co-located reference stations is the subject of a separate study (manuscript
  in preparation).
- **No gap-filling or QC.** Rows are written as measured; gaps exist from
  power and network outages, node relocations and sensor failures.
- The MQ131 ozone signal degrades noticeably in winter conditions.
- The `site` tag is not perfectly uniform across files: the campus nodes
  mostly record it as `QMUL`, node_3 switches from `QMUL` to
  `Queen Mary University of London` partway through the period, and
  node_TH2_MER uses the long form throughout. Treat `QMUL` and
  `Queen Mary University of London` as the same site.
- Timestamps are UTC.

---

## Reference data attribution

Government reference data from stations TH2 (Mile End Road) and TH7 (King
Edward Memorial Park) were obtained from the Environmental Research Group of
Imperial College London, using data from the London Air Quality Network.
This information is licensed under the terms of the **Open Government Licence
v2.0**. Please give the same credit when reusing the files under
`data/reference/`.

- Imperial College London, Environmental Research Group, "Air quality
  networks". https://www.imperial.ac.uk/medicine/departments/school-public-health/environmental-research-group/research/measurement/air-quality-networks/
- Imperial College London, Environmental Research Group, "Environmental
  Research Group Application Programming Interface (ERG API), v1.3", London
  Air Quality Network. http://api.erg.ic.ac.uk/AirQuality/Information/Documentation/pdf

## Licence and citation

The dataset is released under **CC BY 4.0** (see [LICENSE](LICENSE)). If you
use it, please cite it (see [CITATION.cff](CITATION.cff)) together with:

- Mercado-Velazquez, A. A., Poslad, S., & Escamilla-Ambrosio, P. J. (2026).
  *Design and Implementation of a Low-Cost IoT Air-Quality Monitoring Network
  at Queen Mary University of London Mile End Campus*. Smart Cities
  (ICSC-CITIES 2025), CCIS vol. 2742, Springer.
  https://doi.org/10.1007/978-3-032-19019-2_7

## Contact

Andres Aharhel Mercado-Velazquez, IoT2US Lab, School of Electronic
Engineering and Computer Science, Queen Mary University of London.
a.mercadovelazquez@qmul.ac.uk
