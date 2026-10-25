# BLEHCIForge Pro (v1.0.0)

### Bluetooth Core Specification v5.4 Host Controller Interface (HCI) Protocol Studio, H4 UART Packet Dissector, ACL/ISO Frame Builder & 10-Point Normative Zero-Trust Compliance Auditor

[![Bluetooth Core Spec v5.4](https://img.shields.io/badge/Bluetooth%20Core%20Spec-v5.4%20Compliant-blue.svg)](https://www.bluetooth.com/specifications/specs/) ![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-brightgreen.svg) ![License](https://img.shields.io/badge/License-Commercial%20Developer%20Tool-orange.svg) ![Automated Tests](https://img.shields.io/badge/Tests-10%2F10%20Passing-success.svg)

---

## 1\. Executive Summary

**BLEHCIForge Pro** is a high-precision, developer-grade protocol studio and zero-trust verification suite for the **Bluetooth Host Controller Interface (HCI)** and **H4 UART Transport Protocol** (*Bluetooth Core Specification v5.4, Vol 4, Part A & Part E*).

Engineered specifically for embedded firmware engineers, wireless IoT system architects, and hardware cybersecurity auditors, BLEHCIForge Pro eliminates the friction of crafting, debugging, and fuzzing low-level Bluetooth controller communications across Nordic Semiconductor (nRF52/nRF53), Espressif (ESP32-C3/S3), Silicon Labs (EFR32), STMicroelectronics (STM32WB), and Linux BlueZ environments.

---

## 2\. Key Architecture & Protocol Capabilities

### 2.1 H4 UART Transport Framing Engine

* Full support for 1-byte H4 UART packet type indicators:  
  - `0x01`: HCI Command Packet (`HCI_COMMAND_PKT`)  
  - `0x02`: HCI ACL Data Packet (`HCI_ACL_DATA_PKT`)  
  - `0x03`: HCI SCO Data Packet (`HCI_SCO_DATA_PKT`)  
  - `0x04`: HCI Event Packet (`HCI_EVENT_PKT`)  
  - `0x05`: HCI ISO Data Packet (`HCI_ISO_DATA_PKT` \- LE Audio / Auracast)

### 2.2 16-Bit OpCode Bitfield Codec

* Deterministic encoding and decoding between 16-bit OpCodes and its underlying sub-components: OpCode \= (OGF \<\< 10\) \| OCF  
  - **OGF (OpCode Group Field, 6 bits):** Link Control (`0x01`), Link Policy (`0x02`), Controller & Baseband (`0x03`), Informational (`0x04`), Status (`0x05`), Testing (`0x06`), LE Controller (`0x08`), Vendor-Specific (`0x3F`).  
  - **OCF (OpCode Command Field, 10 bits):** Specific command indices (0 .. 1023).

### 2.3 HCI ACL Data Packet Assembler

* Frames 16-bit connection handle and control words:  
  - **Connection Handle (12 bits):** Range 0x0000 .. 0x0EFF.  
  - **PB Flag (Packet Boundary, 2 bits):** `0b00` (First Non-Flushable), `0b01` (Continuing Fragment), `0b10` (First Flushable).  
  - **BC Flag (Broadcast, 2 bits):** `0b00` (Point-to-Point).  
* Automatic 16-bit little-endian payload length computation and L2CAP data frame injection.

### 2.4 10-Point Normative Zero-Trust Compliance Auditor

Automatically validates wire frames against strict Bluetooth Core Spec v5.4 normative constraints:

1. **RULE-01 (Transport Indicator Boundary):** Indicator strictly within `{0x01..0x05}`.  
2. **RULE-02 (OpCode Bitfield Sanity):** Validates 0 \<= OGF \<= 0x3F and 0 \<= OCF \<= 0x03FF.  
3. **RULE-03 (Payload Length Field Match):** Exact octet verification between header length and byte stream.  
4. **RULE-04 (LE Advertising Interval Range Limits):** Validates intervals within 0x0020 (20 ms) and 0x4000 (10.24 s).  
5. **RULE-05 (Advertising Interval Coherence Order):** Enforces Min \<= Max.  
6. **RULE-06 (LE Advertising Channel Map Bitmask):** Validates channel mask in \[0x01..0x07\] (37, 38, 39).  
7. **RULE-07 (Connection Handle Normative Envelope):** Disallows handles in reserved controller range \> 0x0EFF.  
8. **RULE-08 (ACL Packet Boundary Flag Conformance):** Enforces valid PB state bit transitions.  
9. **RULE-09 (Command Complete Return Status Code Taxonomy):** Validates against 76 standardized Bluetooth HCI error statuses.  
10. **RULE-10 (High-Risk Command Safety Warning):** Flags disruptive runtime commands (`HCI_Reset`, `HCI_Disconnect`).

---

## 3\. Directory Layout

product\_305/

├── README.md                                \# This document

├── test\_blehci.py                           \# Automated 10-test verification suite

├── source-code/

│   ├── index.html                           \# Multi-tab responsive Web Studio

│   ├── hci.css                              \# Modern cybersecurity dark theme

│   ├── hci-engine.js                        \# Core protocol encoder & dissector

│   ├── hci-auditor.js                       \# 10-point normative compliance ruleset

│   └── app.js                               \# Interactive UI controller

├── build/

│   ├── blehci-cli.py                        \# Zero-dependency Python 3 CLI runner

│   └── blehciforge-pro-v1.0.0.zip           \# Complete production release bundle

├── documentation/

│   ├── ARCHITECTURE.md                      \# Low-level protocol architecture deep dive

│   ├── USER\_GUIDE.md                        \# Operator guide for web studio and CLI

│   └── MONETIZATION\_AND\_MARKETING\_PLAYBOOK.md \# B2B commercialization and GTM plan

└── screenshots/

    └── blehci-studio-preview.svg            \# Vector UI & architecture preview diagram

---

## 4\. Quickstart & Verification

### Run Automated Unit Tests (10/10 Verification)

python3 test\_blehci.py

### CLI Command Synthesis

\# Synthesize HCI Reset command

python3 build/blehci-cli.py cmd \--name reset

\# Synthesize custom command (OGF 0x08, OCF 0x000A)

python3 build/blehci-cli.py cmd \--ogf 0x08 \--ocf 0x000a \--params "01"

### CLI ACL Data Frame Assembly

python3 build/blehci-cli.py acl \--handle 0x0040 \--pb 0 \--bc 0 \--payload "06000400020100"

### CLI Protocol Dissection & Zero-Trust Audit

python3 build/blehci-cli.py dissect "01 06 20 0f 20 00 40 00 00 00 00 00 00 00 00 07 00"

python3 build/blehci-cli.py audit "01 06 20 0f 20 00 40 00 00 00 00 00 00 00 00 07 00"

---

## 5\. Commercial Licensing & Pricing

* **Community Edition (Open Source):** Core Web Synthesizer for individual makers.  
* **Professional Single-Seat (\$149/year or \$299 perpetual):** Full Web Studio, complete CLI toolchain, C header export, Wireshark clipboard dissector, 10-point compliance audit engine.  
* **Hardware Team License (\$999/year, up to 10 seats):** Automated CI/CD test harness, fuzzing generators, priority support.  
* **Enterprise / Silicon Vendor (\$4,999/year):** Source code escrow, vendor-specific profiles, air-gapped on-premise deployment.