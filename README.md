<!-- title: QM Air Quality Network Data -->
# QM Air Quality Network Data

Fourteen months (**16 April 2025 to 10 June 2026**) of raw air-quality
measurements from the **QMUL Air Quality Monitoring System (AQMS)**: seven
low-cost IoT nodes deployed at Queen Mary University of London (Mile End
campus) and King Edward Memorial Park, London, together with the co-located
records of two Tower Hamlets reference stations.

The stations themselves (hardware design and firmware) are documented at
**[qm-airquality-network](https://github.com/AndresMercad0/qm-airquality-network)**.

**Version:** release **v1.0.0** (10 October 2026). The release tag and its commit identify the version of the files; the dataset has no DOI yet. The calibration code that uses these records, with the reference download of that study, is archived at **[qm-airquality-network-calibration](https://github.com/AndresMercad0/qm-airquality-network-calibration)** (Zenodo, https://doi.org/10.5281/zenodo.22850320).

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
| SHT45 | Sensirion AG | Temperature, humidity | one reading per cycle |
| SGP40 | Sensirion AG | VOC (raw + index) | one reading per cycle |
| SPS30 | Sensirion AG | PM2.5, PM10 | one reading per cycle |
| GM-702B and GM-102B (on the Grove Multichannel Gas v2 board) | Zhengzhou Winsen Electronics (sensors); Seeed Studio (carrier board) | CO, NO2 (raw) | one reading per cycle |
| MQ131, low concentration (on an FC-22 board, via ADS1115) | Zhengzhou Winsen Electronics | O3 (raw + voltage) | one reading per cycle |

Each acquisition cycle reads all sensors, stores the row locally, sends unsynchronised rows to the server, and then pauses for 30 s. The interval between rows is therefore not fixed: its median is 32 to 39 s depending on the node (see [Coverage and gaps](#coverage-and-gaps)).

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

## Coverage and gaps

Computed from the files of release v1.0.0. "Bins with a record" is the share of the 15-minute intervals of the period that hold at least one row.

| File | Rows | Median interval (s) | Bins with a record (%) | Gaps > 24 h (number, total days) | Longest gap (days) |
|---|---|---|---|---|---|
| node_1 | 856,093 | 38 | 98.9 | 1, 4.7 | 4.7 |
| node_2 | 1,063,563 | 32 | 97.1 | 2, 11.4 | 9.6 |
| node_3 | 713,300 | 39 | 82.0 | 5, 75.6 | 33.0 |
| node_4 | 700,257 | 38 | 80.9 | 4, 79.7 | 48.8 |
| node_5 | 660,664 | 34 | 86.4 | 7, 54.7 | 24.5 |
| node_6 | 1,020,218 | 32 | 98.2 | 2, 7.5 | 4.5 |
| node_7 | 977,201 | 32 | 99.2 | 1, 3.1 | 3.1 |
| node_TH2_MER | 40,277 | 900 | 99.8 | 0 | 0.5 |
| node_TH7_KEMP | 40,315 | 900 | 99.8 | 0 | 0.1 |

The reference files hold a row for almost every 15-minute interval, but some rows have empty values; the longest stretches without a value are about 10.7 days of NO2 at TH2 (22 January to 2 February 2026) and 15.7 days of PM2.5 at TH7 (10 to 26 January 2026).

---

## Data quality and processing notes

- **Raw and uncalibrated.** Gas readings are in sensor-native units (ADC counts, volts, index values), not concentrations. Calibration against the co-located reference stations is the subject of a study accepted for publication in *Sensors* (MDPI) in October 2026; its code is in [qm-airquality-network-calibration](https://github.com/AndresMercad0/qm-airquality-network-calibration).
- **No gap-filling or QC.** Rows are written as measured; gaps exist from
  power and network outages, node relocations and sensor failures.
- The MQ131 ozone signal degrades noticeably in winter conditions.
- The `site` tag is not perfectly uniform across files: the campus nodes
  mostly record it as `QMUL`, node_3 switches from `QMUL` to
  `Queen Mary University of London` partway through the period, and
  node_TH2_MER uses the long form throughout. Treat `QMUL` and
  `Queen Mary University of London` as the same site.
- **Timestamps.** All timestamps are UTC. A node row carries the time at which its acquisition cycle started. Reference rows carry the timestamps of the ERG API, which reports in GMT (equal to UTC).
- **Clock of node_3.** Compared with node_1 and node_6, whose PM2.5 series agree within 2 minutes in 14 of the 15 months, the series of node_3 is shifted by more than 2 minutes in 8 of 13 comparable months, and by 5 to 23 minutes from January to March 2026. The cause has not been established; analyses at 15-minute resolution should be aware of it.
- **Revised reference values.** The reference files are the values that the ERG API served on 10 June 2026. LAQN revised earlier NO2 values after the download used by the calibration study (window ending 15 February 2026): 23,994 of 27,498 NO2 values changed at TH2 and 23,011 of 29,290 at TH7; no PM2.5 value changed. To reproduce that study, use the earlier download archived in the calibration repository.
- **Privacy.** Subsite labels were generalised before publication so that no room occupant can be identified from the metadata.

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
