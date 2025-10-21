# 🧮 PreSales Calculator v2.5.0 - Open Source Edition

**Multi-Protocol Storage Analysis Tool for Pre-Sales Engineers**

> **Note:** This is the **Open Source Edition** featuring the Cleversafe calculator. For enterprise consulting, custom calculators, or access to additional vendor implementations (MinIO, Scality, Vast, Qumulo), please [contact me](#-contact).

A comprehensive web application for storage architects, pre-sales engineers, and technical teams to understand storage configurations, capacity planning, and failure tolerance analysis across Block, Object, and File storage protocols.

---

## 🎯 Project Overview

### **Three Calculator Types:**

1. **Block (RAID)** - Traditional RAID 0/1/5/6/10/50/60 with storage aggregation
2. **Object (Reed-Solomon EC)** - Erasure coding for object storage systems
3. **File (SMB/NFS)** - File storage calculators (Coming soon)

### **Current Vendor Support (Open Source Edition):**

**Object Storage:**
- ✅ **Cleversafe** - Reed-Solomon Cauchy EC (Fully implemented)

**Additional Vendors:**
- Additional vendor implementations are available in the full version. [Contact me](#-contact) for details.

---

## ✨ Key Features

### **Block (RAID) Calculator**

#### Storage Aggregation
- **Dynamic storage rows** - Add/remove storage units
- **13 storage units supported:**
  - Binary: B, KiB, MiB, GiB, TiB, PiB, EiB (1024 base)
  - Decimal: KB, MB, GB, TB, PB, EB (1000 base)
- **High-precision arithmetic** using Decimal.js
- **Mixed unit aggregation** - Combine different units (e.g., 2 TB + 500 GB + 256 GiB)

#### RAID Levels Supported
- **RAID 0** - Striping (100% usable, no protection)
- **RAID 1** - Mirroring (50% usable, 1 drive failure)
- **RAID 5** - Single parity ((n-1)/n usable, 1 drive failure)
- **RAID 6** - Dual parity ((n-2)/n usable, 2 simultaneous failures)
- **RAID 10** - Mirrored stripes (50% usable, 1 per pair)
- **RAID 50** - Striped RAID 5 groups (nested RAID)
- **RAID 60** - Striped RAID 6 groups (nested RAID)

#### Calculations Provided
- Raw capacity aggregation
- Usable capacity after RAID overhead
- Storage efficiency percentage
- RAID overhead (capacity and percentage)
- Protection level explanations
- Minimum drive requirements

---

### **Object (Reed-Solomon EC) Calculator - Cleversafe**

#### Width-Threshold-Write Threshold Model
- **Width** - Total Storage Nodes in cluster (default: 9)
- **Threshold** - Minimum nodes required to READ data (default: 6)
- **Write Threshold** - Minimum nodes required to WRITE successfully (default: 7)
- **Rebuild Threshold** - Auto-calculated (equals Threshold for Reed-Solomon systems)

**Key Insight:** Width = Storage Node count, NOT slice count. Objects are broken into slices distributed across all Width nodes.

#### Storage Node Configuration
- Drives per Storage Node (1-120)
- Drive capacity in TB (0.5-100 TB)
- Annual Failure Rate (AFR) - 0-10% (default: 0.5%)
- Collapsible AFR section for advanced users

#### Site Topology
- **Single Site** - All nodes in one location (with risk warnings)
- **3-Site Configuration** - Geographic distribution for disaster recovery

#### Comprehensive Calculations

**Capacity Analysis:**
- Raw Capacity = Width × Drives per Node × Drive Capacity
- Storage Efficiency = (Threshold / Width) × 100%
- Usable Capacity = Raw Capacity × (Threshold / Width)
- Expansion Factor = Width / Threshold

**Failure Tolerance:**
- Node-level tolerance (read/write operations)
- Site-level tolerance (3-site configurations)
- Drive-level tolerance (simplified model)

**Availability & Durability:**
- Durability (9's of durability)
- MTTDL (Mean Time To Data Loss) in years
- Site topology impact on resilience

**Performance Indicators:**
- Write Amplification (Width / Threshold)
- Rebuild Time Estimate (hours or days)
- Network Overhead percentage

**Educational Context:**
- Step-by-step explanation of every calculation
- Why Write Threshold > Threshold
- Reed-Solomon Cauchy mathematics overview
- Storage Nodes vs Slices clarification
- Drive density impact on performance

---

## 🏗️ Architecture

### **Technology Stack**
- **Pure HTML/CSS/JavaScript** - No frameworks, zero dependencies
- **Decimal.js** - High-precision arithmetic via CDN
- **Google Fonts** - IBM Plex Sans typography
- **Single-file application** - Portable and fast

### **Design Philosophy**
- **IBM-inspired UI** - Professional color palette (Blue #0062ff, Cyan #00d4ff, Green #24a148)
- **Dark theme** - Reduces eye strain for long sessions
- **Responsive layout** - Works on desktop, tablet, mobile
- **Educational focus** - Every metric includes tooltips and explanations
- **Accessibility** - Semantic HTML, ARIA-friendly, WCAG AA compliant

### **Navigation System**
```
Calculator Type Dropdown
├── Block (RAID)
├── Object (Reed-Solomon EC)
│   └── Vendor Dropdown (Cleversafe, MinIO, Scality RING, Scality Artesca)
└── File (SMB/NFS)
```

---

## 🚀 Getting Started

### **Prerequisites**
- Modern web browser (Chrome, Firefox, Safari, Edge)
- No installation required
- No build process needed

### **Quick Start**
1. Open `index.html` in your browser
2. Select calculator type from dropdown
3. For Object storage: Select vendor (currently Cleversafe)
4. Enter your configuration parameters
5. Results update in real-time

### **For Block (RAID):**
1. Select "Block (RAID)" from Calculator Type
2. Add storage units (click "Add Storage Unit")
3. Enter quantity and select unit (e.g., 4 × 2 TB)
4. Select RAID level
5. For nested RAID (50/60), specify number of groups
6. View results: raw capacity, usable capacity, efficiency, protection

### **For Object (Reed-Solomon EC) - Cleversafe:**
1. Select "Object (Reed-Solomon EC)" from Calculator Type
2. Select "Cleversafe" from Vendor dropdown
3. Configure erasure coding scheme (Width-Threshold-Write Threshold)
   - Default: 9-6-7 (9 nodes, 6 to read, 7 to write)
4. Configure storage nodes (drives per node, drive capacity, AFR)
5. Select site topology (Single or 3-Site)
6. View comprehensive results and educational explanations

---

## 📊 Example Scenarios

### **RAID Example: 8-Drive RAID 6**
```
Storage Aggregation: 8 × 4 TB = 32 TB raw
RAID Level: RAID 6
Result: 24 TB usable (75% efficiency)
Protection: Can lose 2 drives simultaneously
```

### **Cleversafe Example: 9-6-7 Configuration**
```
Erasure Coding: 9 nodes, 6 threshold, 7 write threshold
Storage Nodes: 9 nodes × 12 drives × 8 TB = 864 TB raw
Result: 576 TB usable (66.7% efficiency)
Protection: Can lose 3 nodes for reads, 2 nodes for writes
Durability: 12 nines (99.9999999999%)
MTTDL: ~1.85 Million years
```

---

## 🧪 Testing

### **RAID Calculator Test Suite**
- `test-raid.html` - Comprehensive test suite
- **38 tests total** - All passing
- Tests cover:
  - Unit conversions (13 units)
  - All RAID levels (0/1/5/6/10/50/60)
  - Storage aggregation (mixed units)
  - Edge cases (large numbers, precision)
  - Nested RAID validation

**Run tests:**
```bash
# Open test-raid.html in browser
# Tests run automatically on page load
# Results display with pass/fail status
```

---

## 📂 Project Structure

```
presales-calculator/
├── index.html                  # Main calculator application
├── test-raid.html             # RAID calculator test suite
├── README.md                  # This file
├── QUICK_START_V2.5.md        # Quick start guide
├── V2.5_BUILD_COMPLETE.md     # Build summary
├── PROGRESS_UPDATE.md         # Development progress
├── CHANGELOG.md               # Version history
├── LICENSE                    # MIT License
├── .gitignore                 # Git ignore rules
├── favicon.svg                # Site icon
├── site.webmanifest          # PWA manifest
├── package.json              # NPM metadata
├── lighthouserc.json         # Lighthouse CI config
├── GIT_CHEAT_SHEET_DENNY.md  # Git reference
└── .github/
    └── workflows/
        └── deploy.yml        # GitHub Actions CI/CD
```

---

## 🔧 Technical Details

### **High-Precision Arithmetic**
Uses Decimal.js for accurate calculations:
- Handles exabyte-scale storage
- Preserves precision for small fractions
- Accurate mixed-unit aggregation
- No floating-point errors

### **RAID Formulas**
```javascript
RAID 0: usable = 100% (no overhead)
RAID 1: usable = 50% (mirroring)
RAID 5: usable = (n-1)/n (single parity)
RAID 6: usable = (n-2)/n (dual parity)
RAID 10: usable = 50% (mirrored stripes)
RAID 50: usable = (drives_per_group - 1) / drives_per_group
RAID 60: usable = (drives_per_group - 2) / drives_per_group
```

### **Reed-Solomon EC Formulas**
```javascript
Storage Efficiency = (Threshold / Width) × 100%
Usable Capacity = Raw Capacity × (Threshold / Width)
Node Tolerance (Read) = Width - Threshold
Node Tolerance (Write) = Width - Write Threshold
Durability (9's) = 9 + (Width - Threshold)
```

---

## 🎨 UI/UX Features

### **Interactive Elements**
- Real-time calculation updates
- Dynamic scheme display (9-6-7 visual)
- Collapsible advanced sections
- Hover tooltips on all parameters
- Color-coded warnings and highlights

### **Visual Feedback**
- Green highlights for key metrics
- Red warnings for risks (single-site, RAID 0)
- Blue accents for interactive elements
- Smooth animations and transitions

### **Responsive Design**
- Two-column layout on desktop
- Single-column on mobile
- Touch-friendly controls
- Readable at all screen sizes

---

## 🚀 CI/CD Pipeline

### **GitHub Actions Workflow**
Automated workflow includes:

1. **Lighthouse CI** - Performance testing
   - Performance score
   - Accessibility audit
   - Best practices check
   - SEO analysis

2. **Deploy to GitHub Pages** - Automatic deployment
   - Builds on push to main/master
   - Creates deployment artifacts
   - Generates version metadata

3. **Release Creation** - Version management
   - Creates ZIP/TAR.GZ archives
   - Auto-generates release notes
   - Tags versions (e.g., v2.5.0)

---

## 🛣️ Roadmap

### **v2.6.0 - Scality Integration**
- [ ] Scality RING calculator
- [ ] Scality Artesca calculator
- [ ] HPE-specific configurations
- [ ] Multi-site topology calculator

### **v2.7.0 - MinIO Support**
- [ ] MinIO erasure coding calculator
- [ ] MinIO-specific schemes
- [ ] Performance modeling

### **v2.8.0 - File Storage Calculators**
- [ ] Qumulo scale-out NAS
- [ ] VAST Data Universal Storage
- [ ] Hammerspace Global Data Platform

### **Future Enhancements**
- [ ] TCO (Total Cost of Ownership) calculator
- [ ] Cost per GB/TB calculations
- [ ] PDF report generation
- [ ] Comparison mode (side-by-side configs)
- [ ] API for integrations

---

## 📖 Documentation

- **README.md** - This file (project overview)
- **QUICK_START_V2.5.md** - Quick start guide with examples
- **V2.5_BUILD_COMPLETE.md** - Detailed build summary
- **CHANGELOG.md** - Version history and release notes
- **PROGRESS_UPDATE.md** - Current development status

---

## 🤝 Contributing

This is currently a private project under active development. Contributions are not accepted at this time.

For questions, feedback, or collaboration inquiries, contact:
- **LinkedIn:** [Your LinkedIn Profile]
- **Email:** [Your Email]

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details

Copyright (c) 2025 [Your Name]

---

## 🙏 Acknowledgments

### **Technology**
- **Decimal.js** - Precision arithmetic library
- **Google Fonts** - IBM Plex Sans typography
- **GitHub Actions** - CI/CD automation

### **Inspiration**
- IBM COS architecture principles
- Cleversafe dispersed storage innovation
- 20+ years of storage industry experience

---

## 📊 Project Stats

- **Version:** 2.5.0
- **Lines of Code:** ~1,850 (index.html)
- **Test Coverage:** 38/38 tests passing (RAID calculator)
- **File Size:** 78.9 KB (main application)
- **Dependencies:** 1 (Decimal.js via CDN)
- **Supported Browsers:** Chrome, Firefox, Safari, Edge

---

## 🔗 Links

- **GitHub Repository:** [Private - Not yet public]
- **Documentation:** See QUICK_START_V2.5.md
- **Issue Tracker:** [GitHub Issues - Private]
- **Changelog:** See CHANGELOG.md

---

## 💡 Use Cases

### **For Pre-Sales Engineers**
- Size storage clusters for customer proposals
- Compare RAID vs erasure coding efficiency
- Calculate failure tolerance and durability
- Educate customers on storage concepts
- Generate "what-if" scenarios quickly

### **For Storage Architects**
- Design multi-site configurations
- Optimize capacity vs protection tradeoffs
- Model performance characteristics
- Validate vendor sizing tools

### **For Technical Sales**
- Demonstrate storage concepts to customers
- Show competitive advantages visually
- Answer "how much do I need?" questions
- Build trust with accurate calculations

### **For Students & Learners**
- Understand erasure coding mathematics
- Learn RAID level differences
- See real-world storage planning
- Interactive educational tool

---

## ⚡ Performance

- **Load Time:** < 1 second (no build process)
- **Calculation Speed:** Instant (client-side JavaScript)
- **Memory Usage:** Minimal (~10 MB)
- **Lighthouse Scores:**
  - Performance: 95+
  - Accessibility: 100
  - Best Practices: 100
  - SEO: 100

---

## 🔒 Security

- **No external API calls** - All calculations client-side
- **No user data collection** - Privacy-first design
- **No authentication required** - Open access
- **No server required** - Static HTML only

---

## 📬 Contact

**Denny Kalaf** - Technology Architect & Storage Solutions Expert

- **LinkedIn:** [linkedin.com/in/dennykalaf](https://linkedin.com/in/dennykalaf)
- **Email:** [email protected]
- **GitHub:** [@denny-architect](https://github.com/denny-architect)

### **Get the Full Version**

This Open Source Edition includes the Cleversafe calculator. The full private version includes:
- ✅ MinIO Reed-Solomon EC calculator
- ✅ Scality RING calculator  
- ✅ Scality Artesca calculator
- ✅ File storage calculators (Qumulo, VAST)
- ✅ Advanced comparison tools

**Interested in:**
- 💼 Enterprise consulting and custom calculator development
- 🤝 Collaboration on storage solutions
- 📊 Full version access for your organization
- 🎓 Training and workshops

**Let's connect!** Reach out via LinkedIn or email.

---

**Built with ❤️ by Denny Kalaf for storage professionals worldwide**

*PreSales Calculator v2.5.0 - Open Source Edition | Last Updated: October 2025*
