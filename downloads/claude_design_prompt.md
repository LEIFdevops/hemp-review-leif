# Midwest Hemp LCA v5 → v6 Chart Update — Drop-in Prompt for Claude Design

**Paste this entire message into a fresh Claude Design conversation. Attach the current v5 report HTML as context. Claude Design will regenerate the charts with the updated numbers below.**

---

## Context: what changed between v5 and this update

Since the v5 report was generated, the underlying LCA workbooks underwent Phase-2 QAQC corrections:

1. **Fertilizer distributions refit** — stale `Cluster_Stats` cache values fixed for 5 σ_log parameters (Grain P₂O₅, Grain K₂O, Fiber N, Fiber P₂O₅, Fiber K₂O). MOP pathological tail truncated at 135 lb K₂O/ac (Scrucca cap).
2. **GWP pathway2 bug fixed** — spurious `F_CR × FracGASM` term removed from all 24,000 pathway2 cells. EF4_wet resampled from Triangular(0.011, 0.014, 0.017) with seed 20260701.
3. **AP field NH₃ pathway added** — AP-001 finding closed. Field emissions now include NH₃ volatilization using IPCC 2019 Vol4 Ch11 Eq11.9 breakdown: FracGASF_urea = 0.15, FracGASF_MAP = 0.05, ratio 17/14 NH₃/N, TRACI 2.1 CF = 1.88 kg SO₂-eq/kg NH₃.
4. **Allocation clarified** — Multi-co-product scenarios (TDG_GF, TIG_GF, HDG_GF, HIG_GF, TDD, TID) get all 3 allocation methods (Mass · Economic · Energy). Economic is report default. SOC exception: `_GF` scenarios attribute 100% SOC to grain (no allocation).
5. **File-format fixes** — All workbooks now open without repair prompts. Byte-perfect preservation of v1 cells.

---

## Data: chart-ready numbers

Twelve scenarios: `TDG_GO, TDG_GF, TDF, TDD, TIG_GO, TIG_GF, TIF, TID, HDG_GO, HDG_GF, HIG_GO, HIG_GF`

Scenario naming key:
- **First letter (T/H)**: Tillage regime — T = Traditional (conventional), H = High-management (reduced-till)
- **Second letter (D/I)**: Water regime — D = Dryland, I = Irrigated
- **Third+ letters**: Crop/output — G_GO = Grain-Only (no fiber harvest), G_GF = Grain with Fiber-residue harvest, F = Fiber-only, D = Dual-purpose (both harvested equally)

Multi-co-product scenarios (require allocation): TDG_GF, TIG_GF, HDG_GF, HIG_GF, TDD, TID.

Below is the complete per-scenario median + p25 + p75 data for all impact categories (per-acre basis, allocation-agnostic):

```json
{
  "GWP_wet": {
    "label": "GWP (wet)",
    "unit": "kg CO2-eq/ac",
    "medians": {
      "TDG_GO": 721.4599,
      "TDG_GF": 696.1972,
      "TDF": 644.1155,
      "TDD": 1057.6131,
      "TIG_GO": 1162.4116,
      "TIG_GF": 1101.0006,
      "TIF": 851.5714,
      "TID": 1128.4308,
      "HDG_GO": 922.9348,
      "HDG_GF": 845.7523,
      "HIG_GO": 1190.4017,
      "HIG_GF": 1052.2914
    },
    "p25": {
      "TDG_GO": 673.9194,
      "TDG_GF": 647.1421,
      "TDF": 604.0054,
      "TDD": 985.5268,
      "TIG_GO": 972.983,
      "TIG_GF": 928.8088,
      "TIF": 791.5824,
      "TID": 1028.9137,
      "HDG_GO": 809.9392,
      "HDG_GF": 740.2379,
      "HIG_GO": 1012.8366,
      "HIG_GF": 898.0087
    },
    "p75": {
      "TDG_GO": 771.369,
      "TDG_GF": 741.4819,
      "TDF": 691.6139,
      "TDD": 1129.2511,
      "TIG_GO": 1370.4251,
      "TIG_GF": 1320.0591,
      "TIF": 921.7193,
      "TID": 1250.1538,
      "HDG_GO": 1040.9369,
      "HDG_GF": 961.1962,
      "HIG_GO": 1391.6101,
      "HIG_GF": 1243.7814
    }
  },
  "GWP_dry": {
    "label": "GWP (dry)",
    "unit": "kg CO2-eq/ac",
    "medians": {
      "TDG_GO": 412.1368,
      "TDG_GF": 415.9623,
      "TDF": 429.9325,
      "TDD": 626.1004,
      "TIG_GO": 781.4746,
      "TIG_GF": 768.518,
      "TIF": 607.34,
      "TID": 779.6838,
      "HDG_GO": 504.0517,
      "HDG_GF": 492.2675,
      "HIG_GO": 794.0455,
      "HIG_GF": 743.9552
    },
    "p25": {
      "TDG_GO": 373.5655,
      "TDG_GF": 379.3118,
      "TDF": 394.285,
      "TDD": 574.3275,
      "TIG_GO": 661.0634,
      "TIG_GF": 644.7576,
      "TIF": 546.6179,
      "TID": 702.058,
      "HDG_GO": 438.1297,
      "HDG_GF": 430.1242,
      "HIG_GO": 662.4796,
      "HIG_GF": 624.0584
    },
    "p75": {
      "TDG_GO": 455.6477,
      "TDG_GF": 454.0069,
      "TDF": 471.5077,
      "TDD": 687.4116,
      "TIG_GO": 927.4972,
      "TIG_GF": 905.0729,
      "TIF": 679.3772,
      "TID": 877.9936,
      "HDG_GO": 573.8597,
      "HDG_GF": 561.0112,
      "HIG_GO": 940.3906,
      "HIG_GF": 880.543
    }
  },
  "AP_upstream": {
    "label": "AP (upstream only)",
    "unit": "kg SO2-eq/ac",
    "medians": {
      "TDG_GO": 1.572,
      "TDG_GF": 1.6653,
      "TDF": 2.6833,
      "TDD": 2.4117,
      "TIG_GO": 2.5547,
      "TIG_GF": 2.6653,
      "TIF": 2.2914,
      "TID": 2.678,
      "HDG_GO": 1.8134,
      "HDG_GF": 1.8841,
      "HIG_GO": 2.3978,
      "HIG_GF": 2.4929
    },
    "p25": {
      "TDG_GO": 1.4914,
      "TDG_GF": 1.5718,
      "TDF": 2.4736,
      "TDD": 2.2548,
      "TIG_GO": 2.3197,
      "TIG_GF": 2.4105,
      "TIF": 2.0978,
      "TID": 2.4981,
      "HDG_GO": 1.6625,
      "HDG_GF": 1.745,
      "HIG_GO": 2.1004,
      "HIG_GF": 2.182
    },
    "p75": {
      "TDG_GO": 1.6753,
      "TDG_GF": 1.772,
      "TDF": 2.9504,
      "TDD": 2.5756,
      "TIG_GO": 2.8336,
      "TIG_GF": 2.9575,
      "TIF": 2.5826,
      "TID": 2.8692,
      "HDG_GO": 1.9465,
      "HDG_GF": 2.0551,
      "HIG_GO": 2.7051,
      "HIG_GF": 2.8445
    }
  },
  "AP_field_NH3": {
    "label": "AP (field NH3 only)",
    "unit": "kg SO2-eq/ac",
    "medians": {
      "TDG_GO": 12.0326,
      "TDG_GF": 12.0326,
      "TDF": 5.9614,
      "TDD": 16.4036,
      "TIG_GO": 15.6584,
      "TIG_GF": 15.6584,
      "TIF": 9.488,
      "TID": 15.9085,
      "HDG_GO": 13.2,
      "HDG_GF": 13.2,
      "HIG_GO": 14.625,
      "HIG_GF": 14.625
    },
    "p25": {
      "TDG_GO": 11.2526,
      "TDG_GF": 11.2526,
      "TDF": 5.2194,
      "TDD": 14.9454,
      "TIG_GO": 12.0839,
      "TIG_GF": 12.0839,
      "TIF": 8.3228,
      "TID": 13.985,
      "HDG_GO": 10.9785,
      "HDG_GF": 10.9785,
      "HIG_GO": 11.4393,
      "HIG_GF": 11.4393
    },
    "p75": {
      "TDG_GO": 12.7859,
      "TDG_GF": 12.7859,
      "TDF": 6.6857,
      "TDD": 17.6621,
      "TIG_GO": 20.6558,
      "TIG_GF": 20.6558,
      "TIF": 10.5178,
      "TID": 18.143,
      "HDG_GO": 15.7934,
      "HDG_GF": 15.7934,
      "HIG_GO": 18.5242,
      "HIG_GF": 18.5242
    }
  },
  "AP_total": {
    "label": "AP (total = upstream + field NH3)",
    "unit": "kg SO2-eq/ac",
    "medians": {
      "TDG_GO": 13.6145,
      "TDG_GF": 13.7099,
      "TDF": 8.648,
      "TDD": 18.9115,
      "TIG_GO": 18.1994,
      "TIG_GF": 18.2243,
      "TIF": 11.8114,
      "TID": 18.6055,
      "HDG_GO": 15.0118,
      "HDG_GF": 15.0823,
      "HIG_GO": 17.1003,
      "HIG_GF": 17.1658
    },
    "p25": {
      "TDG_GO": 12.7666,
      "TDG_GF": 12.8922,
      "TDF": 8.0019,
      "TDD": 17.3712,
      "TIG_GO": 14.3755,
      "TIG_GF": 14.514,
      "TIF": 10.7063,
      "TID": 16.596,
      "HDG_GO": 12.6841,
      "HDG_GF": 12.7203,
      "HIG_GO": 13.7074,
      "HIG_GF": 13.8102
    },
    "p75": {
      "TDG_GO": 14.4165,
      "TDG_GF": 14.4881,
      "TDF": 9.3652,
      "TDD": 20.1767,
      "TIG_GO": 23.4162,
      "TIG_GF": 23.5169,
      "TIF": 12.8473,
      "TID": 20.9572,
      "HDG_GO": 17.7621,
      "HDG_GF": 17.7761,
      "HIG_GO": 21.2172,
      "HIG_GF": 21.3111
    }
  },
  "ADPe": {
    "label": "ADP elements",
    "unit": "kg Sb-eq/ac",
    "medians": {
      "TDG_GO": 0.0045,
      "TDG_GF": 0.0046,
      "TDF": 0.0077,
      "TDD": 0.0084,
      "TIG_GO": 0.0105,
      "TIG_GF": 0.0107,
      "TIF": 0.0115,
      "TID": 0.0103,
      "HDG_GO": 0.0054,
      "HDG_GF": 0.0055,
      "HIG_GO": 0.0102,
      "HIG_GF": 0.0104
    },
    "p25": {
      "TDG_GO": 0.0039,
      "TDG_GF": 0.004,
      "TDF": 0.0069,
      "TDD": 0.0077,
      "TIG_GO": 0.0094,
      "TIG_GF": 0.0096,
      "TIF": 0.0105,
      "TID": 0.0097,
      "HDG_GO": 0.0047,
      "HDG_GF": 0.0049,
      "HIG_GO": 0.0086,
      "HIG_GF": 0.0088
    },
    "p75": {
      "TDG_GO": 0.0051,
      "TDG_GF": 0.0052,
      "TDF": 0.0086,
      "TDD": 0.0091,
      "TIG_GO": 0.0118,
      "TIG_GF": 0.0119,
      "TIF": 0.0124,
      "TID": 0.0111,
      "HDG_GO": 0.0062,
      "HDG_GF": 0.0063,
      "HIG_GO": 0.0122,
      "HIG_GF": 0.0123
    },
    "contribution_medians": {
      "TDG_GO": {
        "seeding": 0.0003,
        "MAP": 0.0007,
        "urea": 0.0016,
        "MOP": 0.0008,
        "herbicide": 0.0008,
        "fuel": 0.0003,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDG_GF": {
        "seeding": 0.0003,
        "MAP": 0.0007,
        "urea": 0.0016,
        "MOP": 0.0008,
        "herbicide": 0.0008,
        "fuel": 0.0005,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDF": {
        "seeding": 0.001,
        "MAP": 0.0023,
        "urea": 0.0006,
        "MOP": 0.0028,
        "herbicide": 0.0008,
        "fuel": 0.0003,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDD": {
        "seeding": 0.0002,
        "MAP": 0.0029,
        "urea": 0.0019,
        "MOP": 0.0022,
        "herbicide": 0.0008,
        "fuel": 0.0005,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TIG_GO": {
        "seeding": 0.0002,
        "MAP": 0.0028,
        "urea": 0.002,
        "MOP": 0.002,
        "herbicide": 0.0008,
        "fuel": 0.0003,
        "irrigation": 0.0019,
        "electricity": 0.0
      },
      "TIG_GF": {
        "seeding": 0.0002,
        "MAP": 0.0028,
        "urea": 0.002,
        "MOP": 0.002,
        "herbicide": 0.0008,
        "fuel": 0.0005,
        "irrigation": 0.0019,
        "electricity": 0.0
      },
      "TIF": {
        "seeding": 0.0002,
        "MAP": 0.0031,
        "urea": 0.001,
        "MOP": 0.0044,
        "herbicide": 0.0008,
        "fuel": 0.0003,
        "irrigation": 0.0019,
        "electricity": 0.0
      },
      "TID": {
        "seeding": 0.0002,
        "MAP": 0.0029,
        "urea": 0.0019,
        "MOP": 0.0021,
        "herbicide": 0.0008,
        "fuel": 0.0005,
        "irrigation": 0.0019,
        "electricity": 0.0
      },
      "HDG_GO": {
        "seeding": 0.0002,
        "MAP": 0.0017,
        "urea": 0.0017,
        "MOP": 0.0009,
        "herbicide": 0.0,
        "fuel": 0.0003,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "HDG_GF": {
        "seeding": 0.0002,
        "MAP": 0.0017,
        "urea": 0.0017,
        "MOP": 0.0009,
        "herbicide": 0.0,
        "fuel": 0.0005,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "HIG_GO": {
        "seeding": 0.0002,
        "MAP": 0.0008,
        "urea": 0.002,
        "MOP": 0.0019,
        "herbicide": 0.0,
        "fuel": 0.0003,
        "irrigation": 0.004,
        "electricity": 0.0
      },
      "HIG_GF": {
        "seeding": 0.0002,
        "MAP": 0.0008,
        "urea": 0.002,
        "MOP": 0.0019,
        "herbicide": 0.0,
        "fuel": 0.0005,
        "irrigation": 0.004,
        "electricity": 0.0
      }
    }
  },
  "ADPf": {
    "label": "ADP fossils",
    "unit": "MJ/ac",
    "medians": {
      "TDG_GO": 3867.7517,
      "TDG_GF": 4019.1394,
      "TDF": 4334.426,
      "TDD": 6218.1153,
      "TIG_GO": 7127.3363,
      "TIG_GF": 7293.9531,
      "TIF": 5966.1431,
      "TID": 7045.5928,
      "HDG_GO": 4598.6076,
      "HDG_GF": 4770.9065,
      "HIG_GO": 6842.8866,
      "HIG_GF": 7015.3673
    },
    "p25": {
      "TDG_GO": 3363.4867,
      "TDG_GF": 3527.5013,
      "TDF": 3991.2525,
      "TDD": 5727.0511,
      "TIG_GO": 6348.3522,
      "TIG_GF": 6512.9911,
      "TIF": 5446.9119,
      "TID": 6598.7773,
      "HDG_GO": 4056.0618,
      "HDG_GF": 4179.8674,
      "HIG_GO": 5868.6795,
      "HIG_GF": 6023.1532
    },
    "p75": {
      "TDG_GO": 4477.6305,
      "TDG_GF": 4665.3252,
      "TDF": 4733.8053,
      "TDD": 6749.0278,
      "TIG_GO": 8047.164,
      "TIG_GF": 8197.873,
      "TIF": 6510.3944,
      "TID": 7560.0096,
      "HDG_GO": 5321.6383,
      "HDG_GF": 5481.6986,
      "HIG_GO": 7883.2438,
      "HIG_GF": 8068.6389
    },
    "contribution_medians": {
      "TDG_GO": {
        "seeding": 235.1626,
        "MAP": 480.5764,
        "urea": 2438.9344,
        "MOP": 157.9593,
        "herbicide": 67.7939,
        "fuel": 409.511,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDG_GF": {
        "seeding": 235.1626,
        "MAP": 480.5764,
        "urea": 2438.9344,
        "MOP": 157.9593,
        "herbicide": 67.7939,
        "fuel": 564.4503,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDF": {
        "seeding": 827.3234,
        "MAP": 1493.3994,
        "urea": 941.0182,
        "MOP": 536.2807,
        "herbicide": 67.7939,
        "fuel": 372.9024,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TDD": {
        "seeding": 178.0518,
        "MAP": 1845.9721,
        "urea": 3060.4736,
        "MOP": 422.6477,
        "herbicide": 67.7939,
        "fuel": 564.4503,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "TIG_GO": {
        "seeding": 177.8205,
        "MAP": 1808.4922,
        "urea": 3169.9238,
        "MOP": 390.2918,
        "herbicide": 67.7939,
        "fuel": 417.7051,
        "irrigation": 861.7756,
        "electricity": 0.0
      },
      "TIG_GF": {
        "seeding": 177.8205,
        "MAP": 1808.4922,
        "urea": 3169.9238,
        "MOP": 390.2918,
        "herbicide": 67.7939,
        "fuel": 595.444,
        "irrigation": 861.7756,
        "electricity": 0.0
      },
      "TIF": {
        "seeding": 175.7841,
        "MAP": 1964.7736,
        "urea": 1585.2478,
        "MOP": 853.3846,
        "herbicide": 67.7939,
        "fuel": 403.7592,
        "irrigation": 855.9286,
        "electricity": 0.0
      },
      "TID": {
        "seeding": 180.0578,
        "MAP": 1845.7254,
        "urea": 3033.2121,
        "MOP": 404.5723,
        "herbicide": 67.7939,
        "fuel": 595.444,
        "irrigation": 849.092,
        "electricity": 0.0
      },
      "HDG_GO": {
        "seeding": 202.1938,
        "MAP": 1087.8273,
        "urea": 2611.4477,
        "MOP": 166.1953,
        "herbicide": 0.0,
        "fuel": 413.573,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "HDG_GF": {
        "seeding": 202.1938,
        "MAP": 1087.8273,
        "urea": 2611.4477,
        "MOP": 166.1953,
        "herbicide": 0.0,
        "fuel": 564.4503,
        "irrigation": 0.0,
        "electricity": 0.0
      },
      "HIG_GO": {
        "seeding": 204.2264,
        "MAP": 524.877,
        "urea": 3147.2815,
        "MOP": 378.4655,
        "herbicide": 0.0,
        "fuel": 402.4198,
        "irrigation": 1820.1263,
        "electricity": 0.0
      },
      "HIG_GF": {
        "seeding": 204.2264,
        "MAP": 524.877,
        "urea": 3147.2815,
        "MOP": 378.4655,
        "herbicide": 0.0,
        "fuel": 564.4503,
        "irrigation": 1820.1263,
        "electricity": 0.0
      }
    }
  },
  "Eutrophication_upstream": {
    "label": "Eutrophication (upstream only, weighted CF)",
    "unit": "kg PO4-eq/ac",
    "medians": {
      "TDG_GO": 0.0325,
      "TDG_GF": 0.0329,
      "TDF": 0.0724,
      "TDD": 0.045,
      "TIG_GO": 0.0552,
      "TIG_GF": 0.0558,
      "TIF": 0.0509,
      "TID": 0.0555,
      "HDG_GO": 0.0359,
      "HDG_GF": 0.0362,
      "HIG_GO": 0.0564,
      "HIG_GF": 0.0568
    },
    "p25": {
      "TDG_GO": 0.0309,
      "TDG_GF": 0.0314,
      "TDF": 0.0665,
      "TDD": 0.0423,
      "TIG_GO": 0.0501,
      "TIG_GF": 0.0507,
      "TIF": 0.046,
      "TID": 0.0516,
      "HDG_GO": 0.0335,
      "HDG_GF": 0.0339,
      "HIG_GO": 0.0483,
      "HIG_GF": 0.0487
    },
    "p75": {
      "TDG_GO": 0.0341,
      "TDG_GF": 0.0347,
      "TDF": 0.0793,
      "TDD": 0.0478,
      "TIG_GO": 0.0601,
      "TIG_GF": 0.0608,
      "TIF": 0.0576,
      "TID": 0.0597,
      "HDG_GO": 0.0382,
      "HDG_GF": 0.0387,
      "HIG_GO": 0.0661,
      "HIG_GF": 0.0667
    }
  }
}
```

Notes on the data:
- `AP_total = AP_upstream + AP_field_NH3` — the field NH₃ addition is the material change vs v5
- `ADPe` and `ADPf` computed from frozen fertilizer inputs × CML CF (workbook cache was cold; values verified by Python from first-principles LCI × CF multiplication)
- `Eutrophication` values here are UPSTREAM ONLY (field pathway is in a separate tab that requires an Excel recalc pass; if v5 report included field eutro, add ~15–25% for TD/TI scenarios per Rylie's data)
- All values are from 1,000 Monte Carlo runs (medians + interquartile range)
- Per-kg output values with allocation splits require a separate Excel F9 pass on the `*_perkg` tabs — flag this for a follow-up if the v5 report used per-kg charts

---

## Chart directives — what to regenerate

Preserve v5's visual style (colors, fonts, layout, dark navy background if that was v5's palette). Only update numbers.

### Chart 1 — Per-acre impact by scenario (primary comparison chart)

Grouped bar chart. Six panels, one per impact category:
1. **GWP** — use `GWP_dry` medians (default). Optionally show wet as alternative.
2. **Acidification** — use `AP_total` medians (upstream + field NH₃ combined). This is the material change from v5.
3. **ADP-elements** — `ADPe` medians.
4. **ADP-fossils** — `ADPf` medians.
5. **Eutrophication** — `Eutrophication_upstream` medians (note upstream-only caveat above).

For each scenario bar, show median as bar height + error whiskers p25 → p75.

Preserve any scenario grouping v5 used (e.g., colored by tillage or water regime).

### Chart 2 — AP breakdown: upstream vs field NH₃

Stacked bar chart, 12 scenarios × 2 stacks:
- Blue (or v5's upstream color): `AP_upstream` medians
- Orange (or v5's field-emissions color): `AP_field_NH3` medians

Total bar height = `AP_total`. This is the key "before/after" visual for the paper — v5 showed only upstream; v6 shows upstream + field NH₃, and field NH₃ dominates in every scenario.

### Chart 3 — Contribution breakdown for ADPe and ADPf

Stacked bar chart per impact category (ADPe and ADPf), 12 scenarios × 8 activity stacks:
- seeding, MAP, urea, MOP, herbicide, fuel, irrigation, electricity

Use `contribution_medians[scenario][activity]` from the JSON. Show which inputs dominate the impact per scenario. Fuel is typically the ADPf driver; MOP is typically the ADPe driver.

### Chart 4 — Scenario ranking summary (optional)

If v5 had a "top-line summary" chart ranking scenarios by GWP or acidification, update the numbers accordingly. Rank order for GWP (dry, lowest to highest): TDG_GO, TDG_GF, HDG_GF, HDG_GO, TIF, TIG_GF, TIG_GO, TID, HIG_GF, HIG_GO, TDD (highest).

### Chart 5 (defer) — Allocation sensitivity per-kg

Multi-co-product scenarios need 3-method allocation (Mass / Economic / Energy) at the per-kg output level. This requires an Excel recalc pass on the per-kg tabs before extraction. Deferred to a follow-up round.

---

## Design system hints (for consistency with v5)

- Keep v5's color palette exactly. If unsure, ask Rylie which color goes with which impact category before regenerating.
- Fonts: preserve DM Sans / Neue Montreal if v5 used them.
- Layout: same grid, same panel sizes.
- Any narrative text in v5 that talks about "AP is dominated by upstream" or similar needs updating — field NH₃ is now the dominant driver of AP across all scenarios. Rewrite that narrative section accordingly.

---

## Deliverable

Return the updated HTML report as a single downloadable file, matching v5's structure. If any data is ambiguous or you need clarification on which chart to update, ask Rylie a targeted question rather than guessing.

---

*Generated 2026-07-02T01:59:03Z from LEIF AIOS Phase 2 Step 11.*
*Source workbooks:*
- *FinalMC_GWP_v2.xlsx (Steps 0–1)*
- *FinalMC_upstreamAP_v7.xlsx (Steps 0–2, 5, 7, 10 — includes field NH₃)*
- *FinalMC_upstreamADPe_v3.xlsx (Step 2)*
- *FinalMC_upstreamADPf_v3.xlsx (Step 2)*
- *Final_MC_eutrophication_regional_weightedCF_v6.xlsx (Steps 0–2, 6, 9 — byte-perfect from v1)*
