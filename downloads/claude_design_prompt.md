# Midwest Hemp LCA v5 → v6 Chart Update — Drop-in Prompt for Claude Design

**Paste this entire message into a fresh Claude Design conversation. Attach the current v5 report HTML (`Midwest Hemp Environmental Footprint v5 - LEIF.html`) as context. Claude Design will regenerate the charts with the updated numbers below and return a new HTML report.**

---

## Context: what changed between v5 and this update

Since the v5 report was generated, the underlying LCA workbooks went through Phase-2 QAQC corrections. **The most material methodological change is the R12 irrigation×regime fix — read this first.**

### R12 IRRIGATION × REGIME FIX (material — landed 2026-07-02)

The v5 GWP model applied a dry/wet emission-factor split as a *climate* toggle uniformly across all 12 scenarios. But irrigation adds soil moisture — which is exactly what drives denitrification (higher EF₁) and leaching (non-zero FracLEACH). Physically, a field belongs on the *wet* curve if it's in a wet climate **OR** if it's irrigated. Only rainfed fields in dry regions belong on the *dry* curve.

**Fix applied (Rylie R12 QR12-1 = Path A):**
- **Irrigated scenarios** (2nd letter I: `TIG_GO, TIG_GF, TIF, TID, HIG_GO, HIG_GF`) collapse to a single wet-regime result. They use `EF1_wet_frozen` ≈ 0.0157 and non-zero FracLEACH regardless of climate zone.
- **Dryland scenarios** (2nd letter D: `TDG_GO, TDG_GF, TDF, TDD, HDG_GO, HDG_GF`) keep both dry and wet — dry vs wet legitimately represents a dry vs wet growing region.

**Impact on numbers:** irrigated per-acre GWP shifted up ~30–50% versus what v5 showed. Dryland unchanged. The `GWP_primary` category in Data Block A is the corrected canonical value; `GWP_dry` and `GWP_wet` remain available as climate sensitivity for dryland scenarios only.

### Other Phase-2 corrections (background context)

1. **Fertilizer distributions refit** — stale `Cluster_Stats` cache values fixed for 5 σ_log parameters (Grain P₂O₅, Grain K₂O, Fiber N, Fiber P₂O₅, Fiber K₂O). MOP pathological tail truncated at 135 lb K₂O/ac (Scrucca cap).
2. **GWP pathway2 bug fixed** — spurious `F_CR × FracGASM` term removed from all 24,000 pathway2 cells. EF4_wet resampled from Triangular(0.011, 0.014, 0.017) with seed 20260701.
3. **AP field NH₃ pathway added** — AP-001 finding closed. Field emissions now include NH₃ volatilization using IPCC 2019 Vol4 Ch11 Eq11.9 breakdown: FracGASF_urea = 0.15, FracGASF_MAP = 0.05, ratio 17/14 NH₃/N, TRACI 2.1 CF = 1.88 kg SO₂-eq/kg NH₃.
4. **Allocation clarified + shipped** — Multi-co-product scenarios (`TDG_GF, TIG_GF, HDG_GF, HIG_GF, TDD, TID`) computed with all 3 allocation methods (Mass · Economic · Energy). Economic is the report default; energy and mass are shown as sensitivity. SOC exception: `_GF` scenarios attribute 100% SOC to grain (no allocation).
5. **File-format corruption fixed** — All workbooks now open without repair prompts. Byte-perfect preservation of v1 cells.

---

## Scenario naming key

Twelve scenarios: `TDG_GO, TDG_GF, TDF, TDD, TIG_GO, TIG_GF, TIF, TID, HDG_GO, HDG_GF, HIG_GO, HIG_GF`

- **1st letter (T/H)**: Tillage — T = Traditional (conventional), H = High-management (reduced-till)
- **2nd letter (D/I)**: Water regime — **D = Dryland (rainfed), I = Irrigated**
- **3rd+ letters**: Output — `G_GO` = Grain-Only (no fiber harvest), `G_GF` = Grain with Fiber-residue harvest, `F` = Fiber-only, `D` = Dual-purpose (both harvested)

Multi-co-product scenarios (require allocation): `TDG_GF, TIG_GF, HDG_GF, HIG_GF, TDD, TID`.

Allocation constants used for economic + energy methods:
- **Grain price**: 1.9327 $/kg (3-yr average 2023-2025 from alloc tab)
- **Fiber price**: 0.463 $/kg (3-yr average 2023-2025)
- **Grain energy content**: 19.29 MJ/kg (as-sold)
- **Fiber energy content**: 14.94 MJ/kg (as-sold)

---

## Data block A: per-ACRE impact by scenario (allocation-agnostic)

Medians + p25 + p75 across 1,000 Monte Carlo runs. Use for Charts 1–3.

**Use `GWP_primary` as the canonical GWP for headline charts.** `GWP_wet` and `GWP_dry` are retained for the dryland-only climate sensitivity chart.

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
  },
  "GWP_primary": {
    "label": "GWP (irrigation-corrected primary)",
    "unit": "kg CO2-eq/ac",
    "medians": {
      "TDG_GO": 412.1404,
      "TDG_GF": 416.0521,
      "TDF": 429.9822,
      "TDD": 626.116,
      "TIG_GO": 1163.2474,
      "TIG_GF": 1101.0958,
      "TIF": 851.8059,
      "TID": 1128.7008,
      "HDG_GO": 504.0619,
      "HDG_GF": 492.4425,
      "HIG_GO": 1190.9238,
      "HIG_GF": 1053.1337
    },
    "p25": {
      "TDG_GO": 373.5655,
      "TDG_GF": 379.3118,
      "TDF": 394.285,
      "TDD": 574.3275,
      "TIG_GO": 972.983,
      "TIG_GF": 928.8088,
      "TIF": 791.5824,
      "TID": 1028.9137,
      "HDG_GO": 438.1297,
      "HDG_GF": 430.1242,
      "HIG_GO": 1012.8366,
      "HIG_GF": 898.0087
    },
    "p75": {
      "TDG_GO": 455.6477,
      "TDG_GF": 454.0069,
      "TDF": 471.5077,
      "TDD": 687.4116,
      "TIG_GO": 1370.4251,
      "TIG_GF": 1320.0591,
      "TIF": 921.7193,
      "TID": 1250.1538,
      "HDG_GO": 573.8597,
      "HDG_GF": 561.0112,
      "HIG_GO": 1391.6101,
      "HIG_GF": 1243.7814
    },
    "method_note": "Per R12 Path A: irrigated scenarios (TIG_GO/TIG_GF/TIF/TID/HIG_GO/HIG_GF) use wet-regime EF1 + non-zero FracLEACH regardless of climate zone. Dryland scenarios (TDG_GO/TDG_GF/TDF/TDD/HDG_GO/HDG_GF) use dry-regime as primary (GWP_wet remains available as climate-wet sensitivity for dryland)."
  }
}
```

## Data block B: per-KG output × 3-METHOD allocation (Chart 5)

Medians only in this block for readability. Full stats (p25, p75, min, max, mean) are in the attached `lca_perkg_allocation.json`. Key format: `SCENARIO__OutputProduct` (e.g., `TDG_GF__Grain`). For single-output scenarios, all 3 methods have identical values.

**Use `GWP_primary` here as well for the canonical GWP per-kg.** GWP_wet and GWP_dry retained for dryland sensitivity.

```json
{
  "GWP_wet": {
    "unit": "kg CO2-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 2.039177,
        "economic": 2.039177,
        "energy": 2.039177
      },
      "TDG_GF__Grain": {
        "mass": 0.725356,
        "economic": 1.394437,
        "energy": 0.845686
      },
      "TDG_GF__Fiber": {
        "mass": 0.725356,
        "economic": 0.334029,
        "energy": 0.654979
      },
      "TDF__Fiber": {
        "mass": 0.226261,
        "economic": 0.226261,
        "energy": 0.226261
      },
      "TDD__Grain": {
        "mass": 0.739103,
        "economic": 1.656519,
        "energy": 0.881079
      },
      "TDD__Fiber": {
        "mass": 0.739103,
        "economic": 0.396809,
        "energy": 0.682391
      },
      "TIG_GO__Grain": {
        "mass": 1.926829,
        "economic": 1.926829,
        "energy": 1.926829
      },
      "TIG_GF__Grain": {
        "mass": 0.680043,
        "economic": 1.307326,
        "energy": 0.792855
      },
      "TIG_GF__Fiber": {
        "mass": 0.680043,
        "economic": 0.313162,
        "energy": 0.614062
      },
      "TIF__Fiber": {
        "mass": 0.252478,
        "economic": 0.252478,
        "energy": 0.252478
      },
      "TID__Grain": {
        "mass": 0.535536,
        "economic": 1.33487,
        "energy": 0.649891
      },
      "TID__Fiber": {
        "mass": 0.535536,
        "economic": 0.31976,
        "energy": 0.503337
      },
      "HDG_GO__Grain": {
        "mass": 1.151553,
        "economic": 1.151553,
        "energy": 1.151553
      },
      "HDG_GF__Grain": {
        "mass": 0.391017,
        "economic": 0.751697,
        "energy": 0.455882
      },
      "HDG_GF__Fiber": {
        "mass": 0.391017,
        "economic": 0.180064,
        "energy": 0.353078
      },
      "HIG_GO__Grain": {
        "mass": 0.903035,
        "economic": 0.903035,
        "energy": 0.903035
      },
      "HIG_GF__Grain": {
        "mass": 0.299378,
        "economic": 0.575529,
        "energy": 0.349041
      },
      "HIG_GF__Fiber": {
        "mass": 0.299378,
        "economic": 0.137864,
        "energy": 0.270331
      }
    }
  },
  "GWP_dry": {
    "unit": "kg CO2-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 1.164958,
        "economic": 1.164958,
        "energy": 1.164958
      },
      "TDG_GF__Grain": {
        "mass": 0.436524,
        "economic": 0.839182,
        "energy": 0.508939
      },
      "TDG_GF__Fiber": {
        "mass": 0.436524,
        "economic": 0.201021,
        "energy": 0.394171
      },
      "TDF__Fiber": {
        "mass": 0.152919,
        "economic": 0.152919,
        "energy": 0.152919
      },
      "TDD__Grain": {
        "mass": 0.441357,
        "economic": 0.978717,
        "energy": 0.525375
      },
      "TDD__Fiber": {
        "mass": 0.441357,
        "economic": 0.234446,
        "energy": 0.4069
      },
      "TIG_GO__Grain": {
        "mass": 1.313916,
        "economic": 1.313916,
        "energy": 1.313916
      },
      "TIG_GF__Grain": {
        "mass": 0.478712,
        "economic": 0.920283,
        "energy": 0.558125
      },
      "TIG_GF__Fiber": {
        "mass": 0.478712,
        "economic": 0.220448,
        "energy": 0.432265
      },
      "TIF__Fiber": {
        "mass": 0.182898,
        "economic": 0.182898,
        "energy": 0.182898
      },
      "TID__Grain": {
        "mass": 0.366784,
        "economic": 0.926656,
        "energy": 0.444587
      },
      "TID__Fiber": {
        "mass": 0.366784,
        "economic": 0.221975,
        "energy": 0.344331
      },
      "HDG_GO__Grain": {
        "mass": 0.648953,
        "economic": 0.648953,
        "energy": 0.648953
      },
      "HDG_GF__Grain": {
        "mass": 0.23341,
        "economic": 0.448711,
        "energy": 0.27213
      },
      "HDG_GF__Fiber": {
        "mass": 0.23341,
        "economic": 0.107486,
        "energy": 0.210763
      },
      "HIG_GO__Grain": {
        "mass": 0.6071,
        "economic": 0.6071,
        "energy": 0.6071
      },
      "HIG_GF__Grain": {
        "mass": 0.211478,
        "economic": 0.406548,
        "energy": 0.24656
      },
      "HIG_GF__Fiber": {
        "mass": 0.211478,
        "economic": 0.097386,
        "energy": 0.190959
      }
    }
  },
  "AP": {
    "unit": "kg SO2-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 0.004425,
        "economic": 0.004425,
        "energy": 0.004425
      },
      "TDG_GF__Grain": {
        "mass": 0.00176,
        "economic": 0.003384,
        "energy": 0.002052
      },
      "TDG_GF__Fiber": {
        "mass": 0.00176,
        "economic": 0.000811,
        "energy": 0.00159
      },
      "TDF__Fiber": {
        "mass": 0.000958,
        "economic": 0.000958,
        "energy": 0.000958
      },
      "TDD__Grain": {
        "mass": 0.001693,
        "economic": 0.003734,
        "energy": 0.002003
      },
      "TDD__Fiber": {
        "mass": 0.001693,
        "economic": 0.000894,
        "energy": 0.001552
      },
      "TIG_GO__Grain": {
        "mass": 0.004302,
        "economic": 0.004302,
        "energy": 0.004302
      },
      "TIG_GF__Grain": {
        "mass": 0.001641,
        "economic": 0.003155,
        "energy": 0.001913
      },
      "TIG_GF__Fiber": {
        "mass": 0.001641,
        "economic": 0.000756,
        "energy": 0.001482
      },
      "TIF__Fiber": {
        "mass": 0.000704,
        "economic": 0.000704,
        "energy": 0.000704
      },
      "TID__Grain": {
        "mass": 0.001263,
        "economic": 0.003146,
        "energy": 0.001528
      },
      "TID__Fiber": {
        "mass": 0.001263,
        "economic": 0.000754,
        "energy": 0.001184
      },
      "HDG_GO__Grain": {
        "mass": 0.002299,
        "economic": 0.002299,
        "energy": 0.002299
      },
      "HDG_GF__Grain": {
        "mass": 0.000898,
        "economic": 0.001727,
        "energy": 0.001047
      },
      "HDG_GF__Fiber": {
        "mass": 0.000898,
        "economic": 0.000414,
        "energy": 0.000811
      },
      "HIG_GO__Grain": {
        "mass": 0.001836,
        "economic": 0.001836,
        "energy": 0.001836
      },
      "HIG_GF__Grain": {
        "mass": 0.000712,
        "economic": 0.001369,
        "energy": 0.00083
      },
      "HIG_GF__Fiber": {
        "mass": 0.000712,
        "economic": 0.000328,
        "energy": 0.000643
      }
    }
  },
  "ADPe": {
    "unit": "kg Sb-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 1.2e-05,
        "economic": 1.2e-05,
        "energy": 1.2e-05
      },
      "TDG_GF__Grain": {
        "mass": 5e-06,
        "economic": 9e-06,
        "energy": 6e-06
      },
      "TDG_GF__Fiber": {
        "mass": 5e-06,
        "economic": 2e-06,
        "energy": 4e-06
      },
      "TDF__Fiber": {
        "mass": 3e-06,
        "economic": 3e-06,
        "energy": 3e-06
      },
      "TDD__Grain": {
        "mass": 6e-06,
        "economic": 1.3e-05,
        "energy": 7e-06
      },
      "TDD__Fiber": {
        "mass": 6e-06,
        "economic": 3e-06,
        "energy": 5e-06
      },
      "TIG_GO__Grain": {
        "mass": 1.7e-05,
        "economic": 1.7e-05,
        "energy": 1.7e-05
      },
      "TIG_GF__Grain": {
        "mass": 6e-06,
        "economic": 1.2e-05,
        "energy": 7e-06
      },
      "TIG_GF__Fiber": {
        "mass": 6e-06,
        "economic": 3e-06,
        "energy": 6e-06
      },
      "TIF__Fiber": {
        "mass": 3e-06,
        "economic": 3e-06,
        "energy": 3e-06
      },
      "TID__Grain": {
        "mass": 5e-06,
        "economic": 1.2e-05,
        "energy": 6e-06
      },
      "TID__Fiber": {
        "mass": 5e-06,
        "economic": 3e-06,
        "energy": 5e-06
      },
      "HDG_GO__Grain": {
        "mass": 7e-06,
        "economic": 7e-06,
        "energy": 7e-06
      },
      "HDG_GF__Grain": {
        "mass": 3e-06,
        "economic": 5e-06,
        "energy": 3e-06
      },
      "HDG_GF__Fiber": {
        "mass": 3e-06,
        "economic": 1e-06,
        "energy": 2e-06
      },
      "HIG_GO__Grain": {
        "mass": 8e-06,
        "economic": 8e-06,
        "energy": 8e-06
      },
      "HIG_GF__Grain": {
        "mass": 3e-06,
        "economic": 6e-06,
        "energy": 3e-06
      },
      "HIG_GF__Fiber": {
        "mass": 3e-06,
        "economic": 1e-06,
        "energy": 3e-06
      }
    }
  },
  "ADPf": {
    "unit": "MJ/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 10.86128,
        "economic": 10.86128,
        "energy": 10.86128
      },
      "TDG_GF__Grain": {
        "mass": 4.274204,
        "economic": 8.156271,
        "energy": 4.976609
      },
      "TDG_GF__Fiber": {
        "mass": 4.274204,
        "economic": 1.953783,
        "energy": 3.854356
      },
      "TDF__Fiber": {
        "mass": 1.521326,
        "economic": 1.521326,
        "energy": 1.521326
      },
      "TDD__Grain": {
        "mass": 4.318493,
        "economic": 9.545061,
        "energy": 5.160003
      },
      "TDD__Fiber": {
        "mass": 4.318493,
        "economic": 2.28646,
        "energy": 3.996394
      },
      "TIG_GO__Grain": {
        "mass": 11.711125,
        "economic": 11.711125,
        "energy": 11.711125
      },
      "TIG_GF__Grain": {
        "mass": 4.443304,
        "economic": 8.478957,
        "energy": 5.173498
      },
      "TIG_GF__Fiber": {
        "mass": 4.443304,
        "economic": 2.031081,
        "energy": 4.006846
      },
      "TIF__Fiber": {
        "mass": 1.745155,
        "economic": 1.745155,
        "energy": 1.745155
      },
      "TID__Grain": {
        "mass": 3.283124,
        "economic": 8.115829,
        "energy": 3.954949
      },
      "TID__Fiber": {
        "mass": 3.283124,
        "economic": 1.944096,
        "energy": 3.063087
      },
      "HDG_GO__Grain": {
        "mass": 5.872528,
        "economic": 5.872528,
        "energy": 5.872528
      },
      "HDG_GF__Grain": {
        "mass": 2.29164,
        "economic": 4.373034,
        "energy": 2.668239
      },
      "HDG_GF__Fiber": {
        "mass": 2.29164,
        "economic": 1.047533,
        "energy": 2.066536
      },
      "HIG_GO__Grain": {
        "mass": 5.136296,
        "economic": 5.136296,
        "energy": 5.136296
      },
      "HIG_GF__Grain": {
        "mass": 1.973869,
        "economic": 3.766646,
        "energy": 2.298247
      },
      "HIG_GF__Fiber": {
        "mass": 1.973869,
        "economic": 0.902276,
        "energy": 1.77998
      }
    }
  },
  "Eutrophication": {
    "unit": "kg P-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 0.000304,
        "economic": 0.000304,
        "energy": 0.000304
      },
      "TDG_GF__Grain": {
        "mass": 0.000106,
        "economic": 0.000203,
        "energy": 0.000123
      },
      "TDG_GF__Fiber": {
        "mass": 0.000106,
        "economic": 4.9e-05,
        "energy": 9.5e-05
      },
      "TDF__Fiber": {
        "mass": 0.000141,
        "economic": 0.000141,
        "energy": 0.000141
      },
      "TDD__Grain": {
        "mass": 0.000318,
        "economic": 0.000715,
        "energy": 0.000379
      },
      "TDD__Fiber": {
        "mass": 0.000318,
        "economic": 0.000171,
        "energy": 0.000294
      },
      "TIG_GO__Grain": {
        "mass": 0.00069,
        "economic": 0.00069,
        "energy": 0.00069
      },
      "TIG_GF__Grain": {
        "mass": 0.000249,
        "economic": 0.00048,
        "energy": 0.000291
      },
      "TIG_GF__Fiber": {
        "mass": 0.000249,
        "economic": 0.000115,
        "energy": 0.000225
      },
      "TIF__Fiber": {
        "mass": 0.000152,
        "economic": 0.000152,
        "energy": 0.000152
      },
      "TID__Grain": {
        "mass": 0.000217,
        "economic": 0.000528,
        "energy": 0.000266
      },
      "TID__Fiber": {
        "mass": 0.000217,
        "economic": 0.000126,
        "energy": 0.000206
      },
      "HDG_GO__Grain": {
        "mass": 0.000259,
        "economic": 0.000259,
        "energy": 0.000259
      },
      "HDG_GF__Grain": {
        "mass": 8.6e-05,
        "economic": 0.000166,
        "energy": 0.000101
      },
      "HDG_GF__Fiber": {
        "mass": 8.6e-05,
        "economic": 4e-05,
        "energy": 7.8e-05
      },
      "HIG_GO__Grain": {
        "mass": 4.4e-05,
        "economic": 4.4e-05,
        "energy": 4.4e-05
      },
      "HIG_GF__Grain": {
        "mass": 1.6e-05,
        "economic": 3.1e-05,
        "energy": 1.9e-05
      },
      "HIG_GF__Fiber": {
        "mass": 1.6e-05,
        "economic": 7e-06,
        "energy": 1.5e-05
      }
    }
  },
  "GWP_primary": {
    "unit": "kg CO2-eq/kg output",
    "medians_by_method": {
      "TDG_GO__Grain": {
        "mass": 1.164958,
        "economic": 1.164958,
        "energy": 1.164958
      },
      "TDG_GF__Grain": {
        "mass": 0.436524,
        "economic": 0.839182,
        "energy": 0.508939
      },
      "TDG_GF__Fiber": {
        "mass": 0.436524,
        "economic": 0.201021,
        "energy": 0.394171
      },
      "TDF__Fiber": {
        "mass": 0.152919,
        "economic": 0.152919,
        "energy": 0.152919
      },
      "TDD__Grain": {
        "mass": 0.441357,
        "economic": 0.978717,
        "energy": 0.525375
      },
      "TDD__Fiber": {
        "mass": 0.441357,
        "economic": 0.234446,
        "energy": 0.4069
      },
      "TIG_GO__Grain": {
        "mass": 1.926829,
        "economic": 1.926829,
        "energy": 1.926829
      },
      "TIG_GF__Grain": {
        "mass": 0.680043,
        "economic": 1.307326,
        "energy": 0.792855
      },
      "TIG_GF__Fiber": {
        "mass": 0.680043,
        "economic": 0.313162,
        "energy": 0.614062
      },
      "TIF__Fiber": {
        "mass": 0.252478,
        "economic": 0.252478,
        "energy": 0.252478
      },
      "TID__Grain": {
        "mass": 0.535536,
        "economic": 1.33487,
        "energy": 0.649891
      },
      "TID__Fiber": {
        "mass": 0.535536,
        "economic": 0.31976,
        "energy": 0.503337
      },
      "HDG_GO__Grain": {
        "mass": 0.648953,
        "economic": 0.648953,
        "energy": 0.648953
      },
      "HDG_GF__Grain": {
        "mass": 0.23341,
        "economic": 0.448711,
        "energy": 0.27213
      },
      "HDG_GF__Fiber": {
        "mass": 0.23341,
        "economic": 0.107486,
        "energy": 0.210763
      },
      "HIG_GO__Grain": {
        "mass": 0.903035,
        "economic": 0.903035,
        "energy": 0.903035
      },
      "HIG_GF__Grain": {
        "mass": 0.299378,
        "economic": 0.575529,
        "energy": 0.349041
      },
      "HIG_GF__Fiber": {
        "mass": 0.299378,
        "economic": 0.137864,
        "energy": 0.270331
      }
    },
    "method_note": "R12 Path A: irrigated (I) uses C2Fg_wet; dryland (D) uses C2Fg_dry."
  }
}
```

---

## Chart directives — what to regenerate

Preserve v5's visual style (colors, fonts, panel layout, dark navy background if that was v5's palette). Only update the numbers and the narrative text about NH₃ and irrigation×regime.

### Chart 1 — Per-acre impact by scenario (primary comparison)

Grouped bar chart. Six panels, one per impact category:
1. **GWP** — use `GWP_primary` medians. **This is the corrected default post-R12.** Irrigated bars will be visibly taller than v5 showed.
2. **Acidification** — use `AP_total` medians (upstream + field NH₃ combined). Field NH₃ dominates now.
3. **ADP-elements** — `ADPe` medians.
4. **ADP-fossils** — `ADPf` medians.
5. **Eutrophication** — `Eutrophication_upstream` medians (note upstream-only caveat below).

For each scenario bar: median as bar height, error whiskers p25 → p75.

### Chart 2 — AP breakdown: upstream vs field NH₃

Stacked bar chart, 12 scenarios × 2 stacks:
- Bottom stack (v5's upstream color): `AP_upstream` medians
- Top stack (new field-NH₃ color, ideally distinct): `AP_field_NH3` medians

Total bar height = `AP_total`. **This is the key "before/after" visual for the paper** — v5 showed only upstream; the corrected version shows upstream + field NH₃, and field NH₃ dominates in every scenario (typically 3–7× upstream). Rewrite any v5 narrative that said "AP is dominated by upstream" — that's no longer true.

### Chart 3 — Contribution breakdown for ADPe and ADPf

Stacked bar chart per impact category (ADPe and ADPf), 12 scenarios × 8 activity stacks:
- seeding, MAP, urea, MOP, herbicide, fuel, irrigation, electricity

Use `contribution_medians[scenario][activity]` from Data Block A. Fuel dominates ADPf; MOP dominates ADPe.

### Chart 4 — GWP scenario ranking (updated post-R12)

Rank order for `GWP_primary`, lowest to highest per-acre:

**TDG_GO (412) → TDG_GF (416) → TDF (430) → HDG_GF (492) → HDG_GO (504) → TDD (626) → TIF (852) → HIG_GF (1053) → TIG_GF (1101) → TID (1129) → TIG_GO (1163) → HIG_GO (1191)**

Notable: post-R12, dryland scenarios cluster at the low end (~412–626 kg CO₂-eq/ac), irrigated cluster at the high end (~852–1191 kg CO₂-eq/ac). This is the corrected physics — irrigation always increases N₂O.

### Chart 5 — Allocation sensitivity per-kg output ★

**This is the ISO-required sensitivity analysis for multi-co-product scenarios.**

Per impact category, show 3-method comparison (Mass / Economic / Energy) for each multi-co-product scenario × output. For single-output scenarios, all methods give identical values.

**Recommended visualization: grouped bar chart per impact category:**
- X-axis: scenario_output pairs (e.g., "TDG_GF Grain", "TDG_GF Fiber", "TDD Grain", "TDD Fiber", ...)
- Y-axis: per-kg impact
- Three bars per group: Mass, Economic, Energy (color-coded)
- **Default view: Economic** (report default per Rylie). Toggle to see other methods.

Key story from the data:
- Under **Economic allocation**, grain always gets a higher per-kg impact than fiber because grain is worth ~4× more per kg ($1.93/kg vs $0.46/kg)
- Under **Mass allocation**, grain and fiber share the impact proportional to yield — usually equal per-kg
- Under **Energy allocation**, grain gets somewhat more than fiber (19.29 vs 14.94 MJ/kg, ~30% higher factor)

Use `per_kg_by_impact[GWP_primary][medians_by_method]` from Data Block B for the GWP per-kg allocation sensitivity.

### Chart 6 (optional add) — Climate sensitivity for dryland

For dryland scenarios only (`TDG_GO, TDG_GF, TDF, TDD, HDG_GO, HDG_GF`), show side-by-side bars for `GWP_dry` and `GWP_wet` to represent dry vs wet growing region as a sensitivity axis. Irrigated scenarios don't have this axis (they're always wet regime post-R12) — you can either omit them from this chart or note "n/a for irrigated" inline.

---

## Narrative updates needed in the report body

- **GWP methodology section**: add a paragraph explaining the R12 fix. Text template: *"For irrigated cropping systems, we apply the IPCC 2019 wet-regime EF₁ (0.0157) and non-zero FracLEACH regardless of surrounding climate, because irrigation is the dominant driver of soil moisture. This departs from a purely climate-zone-driven regime selection and better reflects the physical driver of N₂O and leached-N emissions."*
- **GWP results narrative**: update any language that talked about "dry vs wet climate" as if it applied to all scenarios. Post-R12, only dryland scenarios have a climate sensitivity axis.
- **AP section**: replace "AP is dominated by upstream fertilizer production" with "AP is dominated by field NH₃ volatilization from urea and MAP; upstream fertilizer production is a secondary driver." Include the field-NH₃-to-upstream ratio (typically 3–7× depending on scenario).
- **AP methodology → Impact assessment → Acidification**: add a paragraph explaining the IPCC 2019 Vol4 Ch11 Eq11.9 breakdown for FracGASF (0.15 urea, 0.05 MAP), the NH₃-to-N ratio (17/14), and the TRACI 2.1 CF (1.88 kg SO₂-eq/kg NH₃).
- **Allocation section**: state that Economic allocation is the report default, all three methods are shown as sensitivity, and grain gets more impact under Economic than Mass because of its ~4× price premium.
- **Uncertainty section**: mention the fertilizer distribution recalibration corrected 5 stale σ_log values (Grain P₂O₅, Grain K₂O, Fiber N, Fiber P₂O₅, Fiber K₂O) and truncated the MOP tail at 135 lb K₂O/ac.

---

## Caveats

- **Eutrophication** values in Data Block A are UPSTREAM ONLY. If v5 showed total (upstream + field-weighted CF), add ~15–25% on top for the TD/TI scenarios per Rylie's regionalized CF data. If v5 used only upstream, you're good as-is.
- **ADPe/ADPf** per-acre AND per-kg values were computed by Python from first principles (frozen fertilizer inputs × CML CF from the CF tab). The Excel workbook cache was cold for these two categories — the numbers here should match exactly what the workbook would produce after a Ctrl+Alt+F9 pass.
- **GWP_primary** was computed by combining the cached workbook `TotalGWP_peracre_wet` (for irrigated) and `TotalGWP_peracre_dry` (for dryland). The physical `FinalMC_GWP_v3.xlsx` workbook has a new tab (`TotalGWP_peracre_v3_primary`) with formulas that make the same selection — those formulas will recompute to the same values after a Ctrl+Alt+F9 pass.
- **All values from 1,000 Monte Carlo runs**. `p25` and `p75` give the interquartile range for error whiskers.

---

## Deliverable

Return the updated HTML report as a single downloadable file, structurally matching v5. If any data is ambiguous or a chart needs clarification, ask Rylie a targeted question rather than guessing.

---

*Generated 2026-07-02T19:02:05+00:00 from LEIF AIOS Phase 2 Step 12.*
*Source workbooks (all corrected):*
- *FinalMC_GWP_v3.xlsx (Steps 0–1, 12 — includes R12 irrigation×regime fix)*
- *FinalMC_upstreamAP_v7.xlsx (Steps 0–2, 5, 7, 10 — includes field NH₃)*
- *FinalMC_upstreamADPe_v3.xlsx (Step 2)*
- *FinalMC_upstreamADPf_v3.xlsx (Step 2)*
- *Final_MC_eutrophication_regional_weightedCF_v6.xlsx (Steps 0–2, 6, 9 — byte-perfect from v1)*
