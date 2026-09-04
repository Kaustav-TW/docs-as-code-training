# User Guide

## Introduction

This guide helps end users perform common tasks.

---

## Dashboard Overview

![Dashboard](/docs/Images/Dashboard.png)

### Main Areas

| Area | Description |
|--------|-------------|
| Home | Landing Page |
| Reports | Analytics |
| Profile | User Settings |

---

## Navigation Diagram

```mermaid
flowchart LR
A[Home] --> B[Employees]
A --> C[Payroll]
A --> D[Reports]
A --> E[Time & Attendance]
A --> F[Benefits]
A --> G[Integrations]
A --> H[Administration]
A --> I[System Settings]
A --> J[Audit Logs]
A --> K[Help & Support]
```

---

## Creating a new Employee Record

1. Click **Employees**.
2. Click **New Record**.
3. Enter employee details.
4. Click **Save**.

### Example Data

```json
{
"employeeId": 1001,
"firstName": "John",
"lastName": "Smith",
"dateOfBirth": "08/16/2000",
"department": "Admin",
"hireDate": "08/17/2026",
"payPeriod": "Weekly"
}
```

---

## Search Tips

Use:

`Employee ID`

or

`Employee Name`

---

## Best Practices

> Save work frequently.

### Recommended

- Verify data before saving
- Review reports monthly

### Not Recommended

- ~~Sharing credentials~~
- ~~Using generic accounts~~

---

## Frequently Used Features

| Feature | Benefit |
|-----------|---------|
| Search | Faster access |
| Favorites | Quick navigation |
| Export | Data sharing |