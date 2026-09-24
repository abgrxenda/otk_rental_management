# Rental Management System for Odoo 18

A comprehensive rental management module for Odoo 18 with advanced QR code integration, mobile scanning capabilities, and complete rental lifecycle tracking.

![Odoo Version](https://img.shields.io/badge/Odoo-18.0-blue)
![Python](https://img.shields.io/badge/Python-3-yellow?logo=python&logoColor=white)
![License](https://img.shields.io/badge/license-LGPL--3-green)
![Status](https://img.shields.io/badge/status-production%20ready-brightgreen)

## 🎯 Overview

This module transforms Odoo 18 into a powerful rental management system, perfect for businesses that rent out equipment, tools, vehicles, electronics, or any physical assets. From reservation to return, every step is tracked with precision.

## ✨ Key Features

### 📦 Equipment Management
- **Hierarchical Categories** - Organize equipment with unlimited category depth
- **Serial Number Tracking** - Track individual items with unique serial numbers
- **Auto-generation** - Bulk generate serial numbers with customizable prefixes
- **Stock Management** - Real-time availability tracking (available, reserved, rented, damaged)
- **Multi-rate Pricing** - Daily, weekly, and monthly rental rates

### 📋 Project Management
- **Complete Lifecycle** - Draft → Reserved → Ongoing → Returned → Invoiced
- **Visual Kanban Board** - Drag-and-drop projects between status columns
- **Smart Serial Assignment** - Auto-assign or manually select specific serials
- **Date Tracking** - Start date, end date, actual return date
- **Late Fee Calculation** - Automatic calculation based on overdue days
- **Damage Assessment** - Built-in return wizard with condition tracking

### 🔍 QR Code Integration
- **Auto-generation** - QR codes generated automatically for each serial number
- **Custom Design** - Professional QR codes (300x300px, high error correction)
- **Printable Labels** - Single 4x6" labels or batch printing (9 per A4 page)
- **Download & Print** - Export QR codes as PNG or print formatted labels

### 📊 Tracking & Reporting
- **Status History** - Complete audit trail for every serial number
- **Damage Tracking** - Record damage severity, description, and repair costs
- **Activity Logging** - Full chatter integration for communication
- **Financial Reports** - Track rental revenue, damage fees, late fees

### 💰 Financial Features
- **Automatic Invoicing** - Generate invoices from completed rentals
- **Late Fees** - Configurable daily rate or percentage-based
- **Damage Fees** - Track repair costs with auto-classification (minor/moderate/severe)
- **Discounts** - Apply discounts to projects
- **Payment Status** - Track unpaid, partially paid, and paid rentals

## 🚀 Installation

### Prerequisites
```bash
# Install Python dependencies
pip install qrcode[pil] Pillow
```

### Install Module
1. Download or clone this repository to your Odoo addons directory:
```bash
cd /path/to/odoo/addons
git clone https://github.com/abgrxenda/otk_rental_management.git
```

2. Update Odoo apps list:
   - Go to **Apps** menu
   - Click **Update Apps List**
   - Search for "Rental Management"

3. Install the module:
   - Click **Install**

## 📖 Usage Guide

### Basic Workflow

#### 1. Setup Equipment
```
Equipment Menu → Create Equipment
- Set name, code, and category
- Enable "Track Serial Numbers"
- Set rental rates (daily/weekly/monthly)
- Click "Generate Serials" to bulk create serial numbers
```

#### 2. Create Rental Project
```
Projects Menu → Create Project
- Select customer
- Set start and end dates
- Add equipment items
- Set quantity → Serials auto-assign
- Save as Draft
```

#### 3. Reserve Equipment
```
Project Form → Click "Reserve Equipment"
- Serials are reserved and marked as unavailable
- Status changes to "Reserved"
```

#### 4. Start Rental
```
Project Form → Click "Start Rental"
- Equipment is now rented out
- Status changes to "Ongoing"
- Late fees start calculating after end date
```

#### 5. Return Equipment
```
Project Form → Click "Return Equipment"
- Assess condition for each serial (Good/Minor Damage/Damaged/Lost)
- Damage fees auto-populate based on condition
- Add photos and customer signature
- Click "Complete Return"
- Serials return to available status or marked for repair
```

#### 6. Create Invoice
```
Project Form → Click "Create Invoice"
- Invoice includes rental fees, late fees, and damage fees
- Status changes to "Invoiced"
```

### Advanced Features

#### Kanban Board Management
- **Drag & Drop** - Move projects between status columns
- **Visual Indicators** - Overdue warnings, damage alerts
- **Quick Actions** - Click cards for details

#### Serial Number Management
- **Smart Delete** - Protects serials with history from deletion
- **Bulk Operations** - Select multiple serials for batch actions
- **QR Code Regeneration** - Regenerate QR codes anytime

#### Damage Assessment
- **Good Condition** → Serial returns to available ($0 fee)
- **Minor Damage** → Serial marked as damaged ($100 default fee)
- **Damaged** → Serial sent to repair ($500 default fee)
- **Lost** → Serial disposed (full equipment value fee)

## ⚙️ Configuration

### Settings
Navigate to: **Rental → Configuration → Settings**

#### Late Fee Settings
- Enable/disable late fees by default
- Set daily rate (e.g., $50/day)
- Set percentage (e.g., 5% of rental amount per day)
- Choose calculation method (daily rate, percentage, or maximum of both)

#### Serial Number Settings
- Enable auto-generation
- Set serial number prefix (default: "SN")
- Format: `{PREFIX}-{EQUIPMENT_CODE}-{NUMBER}` (e.g., SN-EQ-0001)

#### Project Settings
- Default rental duration (days)
- Require signature for pickup/return
- Require photos for pickup/return

#### Damage Classification
- Minor damage threshold (default: $100)
- Moderate damage threshold (default: $500)
- Severe damage: anything above moderate threshold

## 🗂️ Module Structure
```
otk_rental_management/
├── models/
│   ├── rental_equipment.py          # Equipment/items
│   ├── rental_equipment_category.py # Categories
│   ├── rental_equipment_serial.py   # Serial numbers + QR
│   ├── rental_project.py            # Projects/bookings
│   ├── rental_project_item.py       # Line items
│   ├── rental_project_item_status.py# Status history
│   ├── qr_generator.py              # QR code generation
│   └── res_config_settings.py       # Configuration
├── views/
│   ├── rental_equipment_views.xml
│   ├── rental_equipment_serial_views.xml
│   ├── rental_project_views.xml
│   └── rental_menus.xml
├── wizards/
│   ├── rental_return_wizard.py      # Return assessment
│   ├── bulk_serial_wizard.py        # Bulk serial generation
│   ├── serial_selection_wizard.py   # Manual serial selection
│   └── serial_delete_confirm_wizard.py
├── reports/
│   └── qr_label_reports.xml         # QR label printing
├── security/
│   ├── rental_security.xml          # Access groups
│   └── ir.model.access.csv          # Access rights
├── data/
│   ├── rental_sequence.xml          # Auto-numbering
│   └── rental_data.xml              # Default data
└── static/
    └── description/
        ├── icon.png
        └── index.html
```

## 🔐 Security

### User Groups
- **Rental User** - Create/edit projects, view equipment
- **Rental Manager** - Full access, delete permissions, settings

### Multi-Company Support
- Equipment and projects respect company boundaries
- Works seamlessly in multi-company Odoo installations

## 🛠️ Technical Details

### Dependencies
- **Odoo Modules**: base, web, sale_management, stock, account, contacts
- **Python Libraries**: qrcode, Pillow (PIL)

### Database Models
- `rental.equipment` - Equipment master data
- `rental.equipment.category` - Categories
- `rental.equipment.serial` - Serial numbers with QR codes
- `rental.project` - Rental projects/bookings
- `rental.project.item` - Project line items
- `rental.project.item.status` - Status history

### Key Technologies
- **QR Generation**: qrcode library with PIL for image processing
- **Odoo 18 Features**: List views, chatter, activities, kanban boards
- **Mail Integration**: Activity tracking, followers, messages

## 📊 Use Cases

### Perfect For:
- 🎬 **Equipment Rental Companies** - Cameras, lighting, sound equipment
- 🏗️ **Construction Equipment** - Tools, machinery, scaffolding
- 💻 **IT Equipment Rental** - Laptops, servers, networking gear
- 🚗 **Vehicle Rental** - Cars, trucks, specialized vehicles
- 🎪 **Event Equipment** - Furniture, decorations, AV equipment
- 🏥 **Medical Equipment** - Hospital equipment, mobility aids
- 🎓 **Educational Institutions** - Lab equipment, projectors, tablets

## 🐛 Known Issues & Limitations

- QR scanning feature requires HTTPS for camera access
- Mobile QR scanner not yet implemented (planned for v2.0)
- Batch operations limited to 1000 items at once

## 🗺️ Roadmap

### Version 2.0 (Planned)
- [ ] Mobile QR scanner with camera integration
- [ ] Barcode support alongside QR codes
- [ ] Advanced reporting and analytics dashboard
- [ ] Customer portal for self-service booking
- [ ] Maintenance scheduling integration
- [ ] API endpoints for third-party integrations
- [ ] WhatsApp/SMS notifications

### Version 2.1 (Future)
- [ ] Multi-warehouse support
- [ ] Delivery/pickup logistics integration
- [ ] Contracts and agreements management
- [ ] Subscription-based rentals

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

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

## 🙏 Acknowledgments

- Odoo Community for the amazing framework
- Contributors and testers who helped improve this module

## 📧 Support

For support, please:
1. Check the [Issues](https://github.com/abgrxenda/otk_rental_management/issues) page
2. Create a new issue with detailed information
3. Contact: [odoo@otek.today]

---

**⭐ If you find this module useful, please star the repository!**
