<div align="center">

# 🚁 Investigating DJI Mavic Air Flight Evidence

### SBT-DF204 · Case Study 5 · Digital Forensics Lab Report

![Domain](https://img.shields.io/badge/Domain-Mobile%20%26%20IoT%20Forensics-blue)
![Platform](https://img.shields.io/badge/Source-Android%20Logical%20Extraction-green)
![App](https://img.shields.io/badge/Artefacts-DJI%20GO%204-orange)
![Telemetry](https://img.shields.io/badge/Telemetry-DatCon%204.3.0-lightgrey)
![Status](https://img.shields.io/badge/Report-Completed-brightgreen)
![Limitation](https://img.shields.io/badge/microSD%20%2F%20EWF-Not%20Analysed-red)

*A forensic examination of DJI GO 4 artefacts, flight-controller telemetry, and cached video evidence recovered from an Android logical extraction.*

</div>

---

## 📑 Table of Contents

1. [Case Information](#1-case-information)
2. [Executive Summary](#2-executive-summary)
3. [Scope and Investigative Questions](#3-scope-and-investigative-questions)
4. [Tools and Environment](#4-tools-and-environment)
5. [Methodology and Evidence Integrity](#5-methodology-and-evidence-integrity)
6. [Evidence Inventory and Integrity Log](#6-evidence-inventory-and-integrity-log)
7. [DJI GO 4 Artefacts](#7-dji-go-4-artefacts)
8. [Flight-Record and Telemetry Findings](#8-flight-record-and-telemetry-findings)
   - [8.1 FLY057](#81-fly057)
   - [8.2 FLY058](#82-fly058)
   - [8.3 Main TXT Record](#83-main-txt-record)
9. [Cached Video and Media Comparison](#9-cached-video-and-media-comparison)
10. [Raw microSD and EWF Image Examination](#10-raw-microsd-and-ewf-image-examination)
11. [UTC Timeline and Correlation](#11-utc-timeline-and-correlation)
12. [Key Findings](#12-key-findings)
13. [Limitations and Further Work](#13-limitations-and-further-work)
14. [Appendices](#14-appendices)
    - [Appendix A: Representative Frame Record](#appendix-a--representative-frame-record)
    - [Appendix B: Recovered MP4 Metadata](#appendix-b--recovered-mp4-metadata)
15. [Figures Referenced in the Report](#15-figures-referenced-in-the-report)
16. [Author](#16-author)
17. [Disclaimer](#17-disclaimer)

---

## 1. Case Information

| Field | Details |
| :--- | :--- |
| **Case Title** | Investigating DJI Mavic Air Flight Evidence |
| **Case Reference** | SBT-DF204 – Case Study 5 |
| **Course** | SBT-DF205 – Mobile and IoT Forensics Case Study |
| **Programme** | Fellowship in Web Application Security & Digital Forensics |
| **Batch** | BATCH-B2025 · L1/S2 |
| **Student Name** | Adekunle Ogunyemi |
| **Student ID** | 2025/FWSD/11479 |
| **Instructor** | Aminu Idris, AMCPN |
| **Report Date** | 2 October 2026 |

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 2. Executive Summary

This report documents an examination of an **Android logical extraction** containing DJI GO 4-related artefacts. The evidence set comprised:

- Two decoded flight-controller **DAT** records (FLY057 and FLY058)
- A cached **2018 MP4** with associated sidecar and analytics files
- Six cached **thumbnail images**
- Five recovered **MP4** files in the case working directory

The working Android ZIP and the source Android ZIP returned **matching SHA-256 values** in the recorded comparison.

The available evidence supports a telemetry sequence on **19 June 2018** and a cached video record whose sidecar time and GPS coordinates are close to the beginning of one telemetry sequence (FLY057). This is a **correlation, not proof** that the files represent the same event. No exact operator identity or precise real-world location is established.

> ⚠️ **Major limitation:** The purported raw microSD image files examined earlier were small HTML pages stating that a link was temporarily disabled, rather than valid disk images. No valid EWF image was located. Raw microSD image analysis and EWF verification/comparison therefore **could not be completed**. These gaps are disclosed rather than inferred.

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 3. Scope and Investigative Questions

The examination addresses:

- Preservation and integrity of evidence
- DJI GO 4 artefacts
- Main flight-record and detailed telemetry findings
- Cached video and image evidence
- Raw microSD and EWF evidence status
- UTC chronology
- Cautious conclusions and limitations

**Constraints:** Work was limited to offline examination of available case files and working copies. No live drone was connected, and no evidence was uploaded to an external analysis service.

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 4. Tools and Environment

| Tool | Purpose |
| :--- | :--- |
| **SHA-256 hashing** | Integrity verification of evidence, working copies, extracted artefacts and recovered videos |
| **ffprobe** | Video codec, resolution, frame rate, duration and file size |
| **FFmpeg** | Extraction of representative frames at approximately 10 seconds |
| **DatCon 4.3.0** | Decoding of the two supplied DAT files into CSV for telemetry review |
| **ExifTool** | Metadata inspection (returned a file-format error on the 2018 TXT record) |
| **strings** | Readable-content check on the main TXT record (no reliable content produced) |

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 5. Methodology and Evidence Integrity

1. Used the provided case directory and Android logical extraction; analysis outputs were written under the case reports folders.
2. Compared `evidence/Android_Logical.zip` with `working/working_Android_Logical.zip`. Both recorded SHA-256 values were:

   ```text
   9a352b282d832556eb7cb73010ea281f9946d889438ce07d9bbee3b64229285d
   ```

3. Recorded SHA-256 hashes for extracted artefacts and recovered videos. Hash equality supports byte-for-byte identity of compared files at hashing time; it does **not** independently establish provenance or acquisition correctness.
4. Used `ffprobe` for video codec, resolution, frame rate, duration and size; `FFmpeg` extracted representative frames at approximately 10 seconds.
5. Used **DatCon 4.3.0** to decode two supplied DAT files into CSV for telemetry review.
6. The available TXT flight records were **not** decoded with a validated parser. ExifTool returned a file-format error for the 2018 TXT record; no unsupported interpretation was assigned to its raw integer values.
7. The sidecar header says **EDT**. If its `14:50:39` time is EDT (UTC−4), it converts to `18:50:39 UTC`. This is a **conditional interpretation**, not an independently verified clock configuration.

*Figure01_Case_Directory.*  
*Figure02_Android_ZIP_Hashes.*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 6. Evidence Inventory and Integrity Log

| Item | Observed Details | SHA-256 / Status |
| :--- | :--- | :--- |
| Android logical ZIP | `evidence/Android_Logical.zip` and working copy | Both: `9a352b282d832556eb7cb73010ea281f9946d889438ce07d9bbee3b64229285d` |
| Cached 2018 MP4 | 57,791,765 bytes; H.264; 1280×720; 30 fps; 240.9724 s | `2ae2a4f186803ec592b0af6284634e592c5079395e99f7df3edd583f8e72ab00` |
| 2018 sidecar `.info` | CaptureDate 2018/06/19 14:50:39; GPS 39.9611953271, −106.2164633258; EndTimeMsec 234833 | `a42bfeb57c5fff850bc059e10eb9a7b4f7b84887feb71e65464099d1e0fd0b4a` |
| 2018 analytics `.map` | 143,358 bytes; associated with cached 2018 record | `db58496a49d6f8ae82af46338cfcd397f5b7b741b1f726718a69caf5d466dd6a` |
| 2018 analytics `.tlv` | 444,914 bytes; not parsed | `6cf36ce052645c6a4df9ecca10499347919048cefa503f0631a1d6b9dc00792c` |
| Main TXT record | 555,776 bytes; not decoded | `0a9f1816d52a98f840f6e67c08a14473ae5a0a2e75e9484d5de66368c4fd4c4a` |
| FLY057 DAT | 4,002,791 bytes; decoded with DatCon | `06130820a4a813bf21f7286150bd3c83084105fecc899bb4d8534895fa2df3a1` |
| FLY058 DAT | 1,104,150 bytes; decoded with DatCon | `fdfccf0c5b1d90d189c19032bb34e151914f42fba42c7994d0492cf327ade07a` |
| `SD_DJI_0001.MP4` | 785,356,701 bytes; H.264 3840×2160; 29.97 fps; 60.794 s | `61a290fa102ce3e2913242b83aae8073af3751cd6b8f1ed0e14eab7625f77815` |
| `SD_DJI_0002.MP4` | 681,239,977 bytes; H.264 3840×2160; 29.97 fps; 52.719 s | `9db5019a6c22e8ce2c2d726df4f8c1c0b52fc3d5806b38058bf1896f80e6ba7b` |
| `SD_DJI_0003.MP4` | 3,759,523,376 bytes; H.264 3840×2160; 29.97 fps; 291.024 s | `aad026be3d81c7dbf251c37d7be4776d239dd44cd7a9988190e6a0f04b87796a` |
| `SD_DJI_0004.MP4` | 1,714,275,603 bytes; H.264 3840×2160; 29.97 fps; 132.699 s | `6aa7dc8ae4f82d9238d99673a3b4c30ab87f3dfe46627de30b2d6f200beaacb3` |
| `SD_DJI_0005.MP4` | 1,473,715,936 bytes; H.264 3840×2160; 29.97 fps; 114.081 s | `890e7c07f2d747cdbdffe8939636fc1c4f1230ce592db8d91fff8742551f8192` |
| Raw microSD / EWF | Candidate `.001` files were ~201 KB HTML error pages, not valid images; no valid EWF found | ❌ Unavailable / invalid |

*Figure03_Recovered_Video_Inventory*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 7. DJI GO 4 Artefacts

The Android extraction contains DJI-related data under:

```text
Android_Logical/sdcard/DJI/dji.go.v4/
```

Artefacts identified:

- Cached 2018 MP4 and `.info` sidecar
- Analytics `.map` and `.tlv` files
- Six `CACHE_IMAGE` JPEG thumbnails
- Main TXT flight-record file
- Two `MCDatFlightRecords` DAT files

Their presence supports that DJI-related application data existed in the logical extraction; it **does not identify** the person who used the app or controlled a drone.

The six thumbnails include indoor/workbench or equipment imagery and outdoor landscape imagery. Filename number strings were not independently validated as capture timestamps, and the examined JPEGs did not provide verified EXIF GPS/capture-time data. They are treated as **cached visual artefacts**, not standalone proof of location or chronology.

*Figure04_DJI_GO4_Artefacts*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 8. Flight-Record and Telemetry Findings

### 8.1 FLY057

| Attribute | Observation |
| :--- | :--- |
| **Records** | 5,260 telemetry records (excluding two initial lines) |
| **Time span** | 2018-06-19 18:50:34 UTC to 18:55:39 UTC (about 5 min 5 s) |
| **Flight modes** | `ASST_TAKEOFF`, `AutoTakeoff`, `GPS_Atti`, `AutoLanding` |
| **Latitude range** | ≈ 39.960316 – 39.961795 |
| **Longitude range** | ≈ −106.217334 – −106.216451 |
| **Bounding-box diagonal** | ≈ 181 m (not total flight distance) |
| **GPS-height range** | 2483.623 – 2516.930 |
| **Relative-height range** | −12.696 – 22.529 |

**Analyst notes:**

- Mode boundaries overlap, so they are not exact physical liftoff/touchdown times.
- The motor-state field is `0` at the first record and `1` thereafter through the last record; the final record does **not** establish motor shutdown or power-off.
- Field units/semantics for height values were not independently confirmed and should not be called metres without qualification.

### 8.2 FLY058

| Attribute | Observation |
| :--- | :--- |
| **Records** | 3,785 decoded data records |
| **Valid GPS timestamps** | 2018-06-19 19:00:23 UTC to 19:02:08 UTC (many rows lack timestamps) |
| **Flight modes** | Mostly `GPS_Atti`, with a smaller number of `Atti` |
| **Motor-state** | `0` in rows where present |

**Analyst notes:**

- Without validated field semantics, motor-state is **not** used to conclude whether the aircraft was powered, airborne or landed.
- Around elapsed time **167.7–168.9 s**, coordinates jump to a latitude of about 39.976 and then return toward 39.961 while timestamps are sparse and relative height is near zero. The cause is unknown; this is treated as a **telemetry/GPS anomaly**, not a verified route segment.

### 8.3 Main TXT Record

The 2018 main TXT record was not decoded with a validated parser. ExifTool reported a file-format error, and `strings` output did not provide reliable, readable content. **No substantive flight events are attributed to it.** File-system modification time is not proof of actual flight time.

*Figure05_FLY057_Telemetry*  
*Figure06_FLY058_Coordinate_Anomaly*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 9. Cached Video and Media Comparison

**Cached 2018 Android video**

| Attribute | Value |
| :--- | :--- |
| Codec / resolution / frame rate | H.264 · 1280×720 · 30 fps |
| Duration (ffprobe) | 240.9724 s |
| Sidecar CaptureDate | 2018/06/19 14:50:39 |
| Sidecar GPS | 39.9611953271, −106.2164633258 |
| Sidecar EndTimeMsec | 234833 (234.833 s) |

EndTimeMsec is about **6.14 seconds shorter** than the ffprobe duration; the reason is unknown.

**Time and location correlation**

- If the sidecar EDT label is correct, `14:50:39 EDT` converts to `18:50:39 UTC`, which is five seconds after the first FLY057 timestamp.
- The sidecar coordinates are close to FLY057's initial coordinates.
- This supports a **possible correlation**, not proof that the cached video and FLY057 are the same flight.

**Recovered MP4 files**

- Five recovered MP4 files are H.264, 3840×2160, 30000/1001 fps.
- All five SHA-256 hashes differ from the cached 2018 MP4, so none is byte-for-byte identical. Different hashes do not rule out related footage if a cached copy was transcoded, cropped or shortened.

**Representative frames (≈ 10 s)**

| Source | Visible Content |
| :--- | :--- |
| `SD_DJI_0001` / `SD_DJI_0002` | Indoor room with tables, computer equipment, people and drone-related equipment |
| `SD_DJI_0003` / `0004` / `0005` | Outdoor rural terrain: grassland, hills, vegetation; cattle visible in one sampled frame |
| Cached 2018 MP4 | Large white event tent, vehicles, people, blue canopy and hills |

The outdoor views share broad visual characteristics, but sampled frames alone **cannot establish** the same location or event.

*Figure07_Cached_Video_Metadata*  
*Figure08_Recovered_Video_Metadata*  
*Figure09_Cached_Video_Frame*  
*Figure10_Recovered_Outdoor_Frame*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 10. Raw microSD and EWF Image Examination

Candidate files named `df048_internal_microSD.001` and `.001.1` were approximately **201 KB** and contained HTML text indicating a link was temporarily disabled. They were **not valid disk images**. No valid EWF image or segment set was located.

Although five large MP4 files are in the working directory, their presence does not prove that a valid image was acquired or that the files were recovered from a verified image.

**This report therefore cannot state:**

- That a raw microSD filesystem was mounted or parsed
- Partition or sector details, or deleted-file recovery from a valid image
- EWF hash validation or raw-versus-EWF agreement

These tasks remain **outstanding** pending correct image evidence and acquisition records.

*Figure11_Invalid_MicroSD_Candidates*  
*Figure12_EWF_Search*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 11. UTC Timeline and Correlation

| UTC Time / Date | Event or Observation | Qualification |
| :--- | :--- | :--- |
| 2018-06-19 18:50:34 | First FLY057 telemetry timestamp; initial GPS near 39.961198, −106.216468 | Telemetry observation; exact liftoff not inferred |
| 2018-06-19 18:50:35–18:50:44 | FLY057 includes `AutoTakeoff` mode | Mode evidence, not exact physical event boundary |
| 2018-06-19 18:50:39 *(conditional)* | Sidecar 14:50:39 EDT converted to UTC | Conditional on timezone label; correlation not proof |
| 2018-06-19 18:50:34–18:55:39 | FLY057 telemetry interval | 5m05s timestamp span; final motor-state field is 1 |
| 2018-06-19 18:55:31–18:55:39 | FLY057 includes `AutoLanding` mode | Does not prove exact touchdown or shutdown |
| 2018-06-19 19:00:23–19:02:08 | FLY058 valid GPS timestamp interval | Many rows lack timestamps; coordinate anomaly present |
| 2018-06-19 14:50:39 *(local)* | Cached MP4 CaptureDate | Original local time retained; UTC conversion conditional |

*Figure13_UTC_Timeline*  


[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 12. Key Findings

| # | Finding |
| :-: | :--- |
| 1 | The Android logical extraction contains multiple DJI GO 4-related artefacts, including a cached MP4, sidecar metadata, analytics files, cached thumbnails, a main TXT record and two DAT flight records. |
| 2 | FLY057 records telemetry from 18:50:34 to 18:55:39 UTC on 19 June 2018 and includes takeoff/flight/landing mode labels. Exact liftoff, touchdown, motor shutdown and power-off are **not** independently established. |
| 3 | FLY058 contains a shorter valid GPS timestamp interval and a suspicious coordinate jump treated as an anomaly rather than a verified route. |
| 4 | The cached 2018 video sidecar's time and coordinates are close to FLY057's beginning, making a relationship **plausible but unproven**. |
| 5 | Five recovered 4K MP4 files are present and hashed. Sample frames show indoor equipment/person imagery and outdoor rural/cattle imagery. None is byte-for-byte identical to the cached Android MP4. |
| 6 | No exact location, aircraft operator, owner or identity of any person is established. |
| 7 | Raw microSD and EWF analysis remains **incomplete** because valid image evidence was unavailable; this is a material limitation. |

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 13. Limitations and Further Work

- [ ] Obtain the correct raw microSD image and EWF image/segments from the authorised evidence source, plus acquisition logs and original hashes; verify before analysis.
- [ ] Use a validated DJI GO 4 TXT parser and document version, assumptions and output validation.
- [ ] Parse `.map` and `.tlv` analytics files with a validated tool if available, retaining originals and recording tool versions.
- [ ] Examine more than one frame per video and compare scenes systematically; do not use a single frame to identify a precise place or event.
- [ ] Confirm timezone/clock settings and source documentation before relying on the EDT-to-UTC conversion.
- [ ] Preserve working copies and generated reports; hash analysis outputs and screenshots in the final evidence log.

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 14. Appendices

### Appendix A – Representative Frame Record

| Frame File | Sampling Point | Visible Content |
| :--- | :--- | :--- |
| `SD_DJI_0001_10s.jpg` | Approx. 00:00:10 | Indoor room; tables, computer equipment, people and drone-related equipment |
| `SD_DJI_0002_10s.jpg` | Approx. 00:00:10 | Indoor work area; monitors and person close to camera |
| `SD_DJI_0003_10s.jpg` | Approx. 00:00:10 | Open grassland, shrubs and trees/brush |
| `SD_DJI_0004_10s.jpg` | Approx. 00:00:10 | Wide rural valley/grassland, track, hills and distant cattle |
| `SD_DJI_0005_10s.jpg` | Approx. 00:00:10 | Dry grass/soil with a cow at edge of frame |
| `android_cached_2018_10s.jpg` | Approx. 00:00:10 | Large white event tent, vehicles, people, blue canopy and hills |

### Appendix B – Recovered MP4 Metadata

| File | Codec / Resolution / Frame Rate | Duration | Size |
| :--- | :--- | :--- | :--- |
| `SD_DJI_0001.MP4` | H.264; 3840×2160; 30000/1001 fps | 60.794 s | 785,356,701 B |
| `SD_DJI_0002.MP4` | H.264; 3840×2160; 30000/1001 fps | 52.719 s | 681,239,977 B |
| `SD_DJI_0003.MP4` | H.264; 3840×2160; 30000/1001 fps | 291.024 s | 3,759,523,376 B |
| `SD_DJI_0004.MP4` | H.264; 3840×2160; 30000/1001 fps | 132.699 s | 1,714,275,603 B |
| `SD_DJI_0005.MP4` | H.264; 3840×2160; 30000/1001 fps | 114.081 s | 1,473,715,936 B |

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 15. Figures Referenced in the Report

Figure captions appear in the relevant sections above, as in the main report.

| Figure | Description |
| :--- | :--- |
| Figure 01 | Case directory |
| Figure 02 | Android ZIP hashes |
| Figure 03 | Recovered video inventory |
| Figure 04 | DJI GO 4 artefacts |
| Figure 05 | FLY057 telemetry |
| Figure 06 | FLY058 coordinate anomaly |
| Figure 07 | Cached video metadata |
| Figure 08 | Recovered video metadata |
| Figure 09 | Cached video frame |
| Figure 10 | Recovered outdoor frame |
| Figure 11 | Invalid microSD candidates |
| Figure 12 | EWF search |
| Figure 13 | UTC timeline |

[⬆ Back to top](#-investigating-dji-mavic-air-flight-evidence)

---

## 16. Author

**Adekunle Ogunyemi**
Registration No.: 2025/FWSD/11479
Fellowship in Web Application Security & Digital Forensics · BATCH-B2025 · L1/S2

**Instructor:** Aminu Idris, AMCPN

---

## 17. Disclaimer

This report records work performed and evidence available. It does **not** claim completion of raw microSD/EWF analysis because valid image evidence was unavailable. All findings are limited to offline examination of the supplied case files and working copies, and are presented for **educational purposes** as part of a digital forensics programme.

<div align="center">

**⬆ [Back to top](#-investigating-dji-mavic-air-flight-evidence)**

</div>