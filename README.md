# ⚙️ Role-Based Access Configuration

This repository hosts the **central configuration file** (`plans_matrix.yml`) for defining **feature accessibility** across different **Plans** and **User Roles** in a role-based feature access system.

It is intended to be publicly available (via **GitHub Pages**) so that client applications can load and interpret access rules dynamically — without recompiling or redeploying code.

---

## 🧩 Overview

The configuration defines:
- **Plans:** Free, Basic, Premium  
- **Roles:** Parent, Child, Member  
- **Features:** Calls, ScreenTime, Location  
- **Access Matrix:** Specifies which role can access which feature depending on plan and target user

Example use case:
| Acting Role | Target Role | Plan | Visible Features |
|--------------|--------------|------|------------------|
| Parent | Child | Free | Calls |
| Parent | Child | Premium | Calls, ScreenTime, Location |
| Member | Self | Basic | Calls |
| Member | Parent | Any | *(None)* |

---

## 🧱 Config File

The configuration file is written in **YAML** for human readability and scalability.

### 📄 [`plans_matrix.yml`](./plans_matrix.yml)
```yaml
version: 1
generated_at: 2025-10-04
notes:
  - "R = allowed/accessible; N = not allowed."
  - "Matrix is explicit per acting_role → target_role → plan → feature."

features:
  - Calls
  - ScreenTime
  - Location

plans:
  Free:
    features: [Calls]
  Basic:
    features: [Calls, ScreenTime]
  Premium:
    features: [Calls, ScreenTime, Location]

roles:
  - Parent
  - Child
  - Member

access:
  Parent:
    Child:
      Free:     { Calls: R, ScreenTime: N, Location: N }
      Basic:    { Calls: R, ScreenTime: R, Location: N }
      Premium:  { Calls: R, ScreenTime: R, Location: R }
  Member:
    self:
      Free:     { Calls: R, ScreenTime: N, Location: N }
      Basic:    { Calls: R, ScreenTime: N, Location: N }
      Premium:  { Calls: R, ScreenTime: N, Location: N }
