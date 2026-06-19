# Houras Marble & Granite ERP

A premium, cross-platform ERP for a marble & granite workshop, built with **Flutter**.
Runs on **Windows Desktop, Android, and iOS** (and web/macOS/linux). Offline-first with
local Hive storage, JWT-style auth, role-based permissions, Arabic (RTL) / English,
light & dark themes, charts, PDF invoices and Excel export.

> Built with a feature-first Clean Architecture + Repository pattern + Riverpod DI.

---

## Features

- **Dashboard** — KPI cards (orders, revenue, expenses, profit), revenue line chart,
  orders bar chart, inventory pie chart, top-customers, low-stock alerts, recent orders.
- **Orders** — create/edit, status workflow (New → Cutting → Polishing → Production →
  Ready → Installed → Delivered), animated progress timeline, PDF invoice.
- **Marble Cost Calculator** — `Area = L × W × Qty`, material + cutting + polishing +
  transport + installation + profit margin.
- **Customers / Suppliers** — full CRUD, balances/statements.
- **Inventory** — materials, stock value, incoming/outgoing/adjustment movements,
  low-stock detection, Excel export.
- **Employees** — directory, salaries, attendance check-in/out, worked hours.
- **Accounting** — cashbox, income, expenses, receivables, payables, payments.
- **Reports** — profit trend, sales summary, Excel export.
- **Settings** — theme switcher, language switcher, reseed/clear database.

## Architecture

```
lib/
  main.dart                 # entry point, Hive init + seeding
  app.dart                  # MaterialApp.router, theme, l10n, RTL
  core/
    constants/              # app constants, box names, keys
    localization/           # AR/EN strings + delegate
    providers/              # Riverpod: repos, theme, locale, refresh
    router/                 # go_router config + auth redirect
    services/               # Hive, PDF, Excel
    theme/                  # colors + light/dark themes
    utils/                  # responsive, formatters, security (hash/JWT)
    widgets/                # GlassCard, KpiCard, shimmer, states, panels
  data/
    models/                 # 12 entities (User, Role, Customer, Supplier,
                            #   Material, InventoryMovement, Order, OrderItem,
                            #   Employee, Attendance, Expense, Payment)
    repositories/           # BaseRepository<T> + concrete repos
    sample_data.dart        # realistic demo seeding
  features/
    auth/ dashboard/ customers/ suppliers/ inventory/ orders/
    employees/ accounting/ reports/ calculator/ settings/
  shared/
    layout/                 # AppShell, Sidebar, nav items
    widgets/                # confirm dialog, snackbars
```

### Tech / Packages
Riverpod, go_router, fl_chart, flutter_animate, flex_color_scheme,
responsive_framework, flutter_screenutil, google_fonts, shimmer,
flutter_staggered_grid_view, data_table_2, font_awesome_flutter,
pdf + printing, excel, hive + hive_flutter, shared_preferences,
dio + connectivity_plus (sync layer), intl, uuid, crypto.

> Entities are persisted as JSON strings in Hive, so **no `build_runner`/codegen** is required.

## Setup

1. **Install Flutter** (3.22+): https://docs.flutter.dev/get-started/install
   Verify with:
   ```bash
   flutter --version
   ```
2. From the project root, generate the platform folders (Windows/Android/iOS):
   ```bash
   flutter create .
   ```
3. Get dependencies:
   ```bash
   flutter pub get
   ```
4. Run:
   ```bash
   flutter run -d windows      # Windows desktop
   flutter run -d chrome       # Web
   flutter run                 # connected Android/iOS device
   ```

### Demo login
```
Email:    admin@houras.com
Password: admin123
```
Other seeded accounts: `accountant@houras.com`, `store@houras.com` (same password).

## Roles & Permissions
`Admin`, `Accountant`, `Store Manager`, `Technician`, `Installer`.
The sidebar and routes adapt to the signed-in role (see `data/models/role.dart`).

## Notes
- **Offline-first:** all data lives locally in Hive. A `dio`/`connectivity_plus` based
  REST sync layer can flush local changes to `AppConstants.apiBaseUrl` when online.
- **JWT:** `core/utils/security.dart` issues/verifies a mock signed token locally; swap
  for real server-issued tokens when wiring a backend.
- Use **Settings → Reseed** to reset demo data.
