# OTEK Rental Management for Odoo 19

A comprehensive rental management module for Odoo 19 with QR code generation, digital signatures, and complete rental lifecycle tracking. Mobile web-based QR scanning is in progress (see Known Issues).

![Odoo Version](https://img.shields.io/badge/Odoo-19.0-blue)
![Python](https://img.shields.io/badge/Python-3-yellow?logo=python&logoColor=white)
![License](https://img.shields.io/badge/license-LGPL--3-green)
![Status](https://img.shields.io/badge/status-in%20development-yellow)

## 🎯 Overview

This module turns Odoo 19 into a full rental management system, suitable for businesses renting out equipment, tools, vehicles, electronics, or any physical assets. Every step - from reservation to return and invoicing - is tracked with serial-level precision, QR codes, and photo/signature capture.

## ✨ Key Features

### 📦 Equipment Management
- **Hierarchical Categories** - Organize equipment with unlimited category depth
- **Serial Number Tracking** - Track individual units with unique, auto-generated serial numbers
- **Stock Management** - Real-time availability (available, reserved, rented, damaged, disposed)
- **Multi-rate Pricing** - Daily, weekly, and monthly rental rates

### 📋 Project Management
- **Complete Lifecycle** - Draft → Reserved → Ongoing → Returned → Invoiced (with Cancelled at any stage)
- **Kanban Board** - Visual overview with overdue and damage indicators
- **Serial Assignment** - Auto-assign or manually select specific serials per line item
- **Partial Pickup/Return** - Hand over or return a subset of a project's items without closing the whole project
- **Late Fee Calculation** - Automatic, based on a fixed daily rate, a percentage of the rental amount, or whichever is greater
- **Damage Assessment** - Built-in return wizard with severity classification and repair-cost estimates
- **Digital Signatures** - Capture customer signatures for pickup, return, partial pickup/return, and damage acknowledgment

### 🔍 QR Code Integration
- **Auto-generation** - QR codes generated automatically for each serial number, regenerated if the serial number changes
- **Company-branded Design** - Circular dots, rounded position markers, optional logo overlay, high error correction, 1080x1080px output
- **Printable Labels** - Single label or batch label sheets for printing
- **Mobile Web Scanner** *(in progress, not yet functional)* - Camera-based scanning directly in the browser (jsQR), no native app required; scaffolding and context-aware routing (add to project, handover, return, report damage, send to repair, verify) are in place but the feature is not complete

### 📊 Tracking & Reporting
- **Status History** - Full audit trail per serial number, including who changed what and when
- **Scan Log** - Every QR scan is recorded with type, user, and outcome
- **Chatter Integration** - Activity tracking, followers, and messages on equipment and projects

### 💰 Financial Features
- **Automatic Invoicing** - Generate an invoice directly from a completed rental project
- **Late Fees** - Configurable daily rate and/or percentage-based, whichever yields the higher fee
- **Damage Fees** - Configurable minor/moderate severity thresholds, tracked per status-history entry
- **Discounts** - Apply a discount to the project total
- **Payment Status** - Unpaid, partially paid, or paid

### 🌐 REST API
Public, API-key-authenticated endpoints for integrating external systems (kiosks, self-service terminals, third-party apps):
- `GET /api/rental/serial/<serial_number>` - look up a serial's current status
- `POST /api/rental/serial/rent` / `POST /api/rental/serial/return` - quick rent/return
- `GET /api/rental/equipment/list`, `GET /api/rental/equipment/<id>` - equipment catalog
- `GET /api/rental/project/list`, `POST /api/rental/project/create` - project listing and creation

## 🚀 Installation

### Prerequisites
```bash
# Python dependencies (installed automatically on first module install if missing)
pip install qrcode[pil] Pillow
```

### Install
1. Copy this module into your Odoo 19 addons path (or install directly from the Odoo Apps Store once published).
2. Go to **Apps** → **Update Apps List**.
3. Search for "OTEK Rental Management" and click **Install**.

## 📖 Usage Guide

### Basic Workflow

#### 1. Setup Equipment
```
Equipment Menu → Create Equipment
- Set name, category, and rental rates (daily/weekly/monthly)
- Enable "Track Serial Numbers" and "Auto-generate Serials" if desired
- Use the "Generate Serials" wizard to bulk-create serial numbers
```

#### 2. Create Rental Project
```
Projects Menu → Create Project
- Select customer, start and end dates
- Add equipment items; serials auto-assign or select manually
- Save as Draft
```

#### 3. Reserve → Start → Return
```
Project Form → "Reserve Equipment" → "Start Rental" → "Complete Return"
- Or use "Partial Pickup" / "Partial Return" for split handovers
- Assess condition on return; damage fees auto-populate based on severity
- Capture customer signature and photos at pickup/return
```

#### 4. Invoice
```
Project Form → "Create Invoice"
- Invoice includes rental fees, late fees, and damage fees
- Status changes to "Invoiced"
```

### Mobile QR Scanning *(in progress)*
The web-based scanner is under active development and not yet functional end-to-end.

## ⚙️ Configuration

Navigate to: **Rental → Configuration → Settings**

- **Late Fees** - Enable by default, set daily rate and/or percentage, choose calculation method
- **Serial Numbers** - Enable auto-generation, format is `{EQUIPMENT_CODE}-{NUMBER}` (e.g., `EQ-0001`)
- **Reminders & Overdue** - Send reminder emails before due date, send overdue notifications
- **Invoicing** - Auto-create invoice on return, optionally include late fees
- **Stock Warnings** - Low-stock threshold alerts
- **Damage Classification** - Minor/moderate fee thresholds; anything above moderate is treated as severe
- **QR Branding** - Upload a company logo to overlay on generated QR codes

## 🗂️ Module Structure
```
otk_rental_management/
├── controllers/
│   └── main.py                          # Public REST API (API-key auth)
├── models/
│   ├── otk_rental_equipment.py          # Equipment/items
│   ├── otk_rental_equipment_category.py # Categories
│   ├── otk_rental_equipment_serial.py   # Serial numbers + QR generation
│   ├── otk_rental_project.py            # Projects/bookings
│   ├── otk_rental_project_item.py       # Line items
│   ├── otk_rental_project_item_status.py# Status/damage history
│   ├── otk_rental_project_signature.py  # Digital signatures
│   ├── otk_rental_scan_log.py           # QR scan audit log
│   ├── qr_generator.py                  # QR code image generation
│   ├── company_qr_extension.py          # Company logo overlay
│   ├── serial_qr_model.py               # QR mixin helpers
│   └── res_config_settings.py           # Configuration
├── views/
│   ├── otk_rental_equipment_views.xml
│   ├── otk_rental_equipment_category_views.xml
│   ├── otk_rental_equipment_serial_views.xml
│   ├── otk_rental_project_views.xml
│   ├── otk_rental_project_item_views.xml
│   ├── otk_rental_project_signature_views.xml
│   ├── qr_scanner_views.xml             # Mobile web scanner
│   ├── res_config_settings_views.xml
│   └── otk_rental_menus.xml
├── wizards/
│   ├── otk_rental_return_wizard.py      # Full return assessment
│   ├── otk_rental_pickup_wizard.py      # Partial pickup
│   ├── otk_rental_partial_return_wizard.py
│   ├── bulk_serial_wizard.py            # Bulk serial generation
│   ├── serial_selection_wizard.py       # Manual serial selection
│   ├── serial_delete_confirm_wizard.py
│   └── add_signature_wizard.py
├── reports/
│   └── qr_label_report.xml              # QR label printing
├── security/
│   ├── otk_rental_security.xml          # Access groups
│   └── ir.model.access.csv              # Access rights
├── data/
│   ├── otk_rental_sequence.xml          # Auto-numbering
│   └── otk_rental_data.xml              # Default data
└── static/
    ├── lib/jsQR/                        # Camera QR decoding
    └── description/
```

## 🔐 Security

### User Groups
- **Rental User** (`group_otk_rental_user`) - Create/edit projects, view equipment
- **Rental Manager** (`group_otk_rental_manager`) - Full access, delete permissions, settings

### Multi-Company Support
Equipment and projects respect company boundaries and work in multi-company installations.

## 🛠️ Technical Details

### Dependencies
- **Odoo Modules**: base, web, sale_management, stock, account, contacts
- **Python Libraries**: qrcode, Pillow (PIL)
- **JavaScript**: jsQR (bundled)

### Database Models
- `otk.rental.equipment` - Equipment master data
- `otk.rental.equipment.category` - Categories
- `otk.rental.equipment.serial` - Serial numbers with QR codes
- `otk.rental.project` - Rental projects/bookings
- `otk.rental.project.item` - Project line items
- `otk.rental.project.item.status` - Status/damage history
- `otk.rental.project.signature` - Captured signatures
- `otk.rental.scan.log` - QR scan audit log

## 📊 Use Cases

Equipment rental companies (cameras, lighting, sound), construction equipment, IT equipment, vehicle rental, event equipment, medical equipment, and educational institutions lending lab/AV equipment.

## 🐛 Known Issues

- **Mobile web QR scanner is not yet functional** - views and JS scaffolding exist but the feature is incomplete; do not advertise this as working until finished.

## 🤝 Contributing

Contributions are welcome via Pull Request:
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the LGPL-3 License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Ömer Kadir | Ömer Teknoloji**
- Website: [omertek.com](https://omertek.com)
- Support: support@omertek.com

---

**⭐ If you find this module useful, please star the repository!**
