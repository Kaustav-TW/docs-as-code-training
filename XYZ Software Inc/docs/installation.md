---
sidebar_position: 2
---
# Product XYZ Installation Guide

> This guide explains how to install Product XYZ.
> Gauri - Please review this file

## Table of Contents

- #Overview
  - #Key_Features
  - #System_Requirements
- #Pre-Installation_Checklist
- #Installation_Architecture
  - #Step1: Download
  - #Step2: Run Installer
  - #Step3: Validate Installation
- #Notes
- #Footnote

---

## Overview

Product XYZ enables organizations to manage employee data efficiently.

### Key Features

- Employee Management
- Payroll Integration
- Reporting Dashboard

### System Requirements

| Component | Requirement |
|------------|:------------:|
| OS | Windows 11 |
| RAM | 8 GB |
| Storage | 20 GB |

---

## Pre-Installation Checklist

- [x] Download installer
- [x] Verify permissions
- [x] Schedule maintenance window

---

## Installation Architecture

```mermaid
flowchart LR
A[Download Installer] --> B[Run Setup]
B --> C[Install Components]
C --> D[Launch Product]
```

---

## Step 1: Download

Click the following link:

[Download Package](https://www.XYZProduct.com/download)

---

## Step 2: Run Installer

Execute:

```powershell
ProductXYZ_Setup.exe
```

### Installation Wizard

![Installation Wizard800x450/png?text=Installation+Wizard](./Images/Installation%20Wizard.png)

Click `Next` and follow the instructions on screen.

---

## Step 3: Validate Installation

Run:

```bash
xyz --version
```

Expected output:

```text
Version 5.2.1
```

---

## Notes

> Administrator rights are required.

~~Windows Server 2012~~ is no longer supported.

---

## Footnote

Installation logs are stored in the application folder.[^1]

[^1]: Default location: C:\ProgramData\ProductXYZ\Logs
