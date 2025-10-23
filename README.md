# 🧮 PreSales Calculator v2.5.3 - Open Source Edition

**Multi-Protocol Storage Analysis Tool for Pre-Sales Engineers**

> **Note:** This is the **Open Source Edition** featuring the Cleversafe calculator. For enterprise consulting, custom calculators, or access to additional vendor implementations (Scality, Qumulo, VAST), please contact me.

A comprehensive web application for storage architects, pre-sales engineers, and technical teams to understand storage configurations, capacity planning, and failure tolerance analysis across Block, Object, and File storage protocols.

---

## 🌟 Features

### ✅ Current Capabilities (v2.5.3)

#### **Block (RAID) Calculator**
- Storage unit aggregation with decimal/binary conversion
- Multi-unit capacity summation (B, KB, MB, GB, TB, PB, EB and their binary equivalents)
- Precision decimal calculations using Decimal.js
- Support for RAID 0, 1, 5, 6, 10 configurations
- Usable capacity calculations
- RAID-specific failure tolerance analysis

#### **Object (Reed-Solomon EC) Calculator**
- **Vendor Support:**
  - Cleversafe (active)
  
- **Cleversafe Configurations:**
  - Standard schemes: 9/6, 14/10, 16/11
  - Custom scheme builder with validation
  - 3-site deployment topology support
  - Single-site configuration warnings

- **Analysis Capabilities:**
  - Capacity analysis (raw, usable, efficiency, expansion factor)
  - Failure tolerance (node read/write, site failures, concurrent drives)
  - Data durability calculations (nines, MTTDL)
  - Performance indicators (write amplification, rebuild time, network overhead)
  - Educational context with scheme explanations

---

## 🚀 Getting Started

### **Quick Start**

1. **Clone the repository:**
   ```bash
   git clone https://github.com/denny-architect/presales-calculator-public.git
   cd presales-calculator-public
   ```

2. **Open in browser:**
   ```bash
   open index.html
   # Or simply double-click index.html
   ```

3. **No build process required!** Pure HTML/CSS/JavaScript

### **Usage**

1. **Select Calculator Type:**
   - Block (RAID) - For RAID configurations
   - Object (Reed-Solomon EC) - For erasure coding systems
   - File (SMB/NFS) - Coming soon in private version

2. **For Object Storage:**
   - Choose vendor (currently: Cleversafe)
   - Select or customize EC scheme
   - Configure storage parameters
   - Click "Calculate Results"

3. **Review Analysis:**
   - Capacity metrics
   - Failure tolerance
   - Durability estimates
   - Performance indicators
   - Educational context

---

## 📋 Technical Architecture

### **Technology Stack**
- **Frontend:** Pure HTML5, CSS3, JavaScript (ES6+)
- **Styling:** IBM Plex Sans font, custom CSS with CSS Grid/Flexbox
- **Math Library:** Decimal.js for precision calculations
- **Icons:** Custom emoji-based icon system (no external dependencies)

### **File Structure**
```
presales-calculator-public/
├── index.html              # Main application (self-contained)
├── test-raid.html          # RAID calculator test suite
├── package.json            # npm metadata
├── README.md               # This file
├── CHANGELOG.md            # Version history
└── QUICK_START_V2.5.md     # Detailed quick start guide
```

### **Browser Compatibility**
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

---

## 🧪 Testing

### **RAID Calculator Test Suite**

Run the included test suite to verify RAID calculations:

```bash
open test-raid.html
```

**Test Coverage:**
- Unit conversion accuracy (decimal/binary)
- RAID 0, 1, 5, 6, 10 calculations
- Edge cases and validation
- Multi-unit aggregation

---

## 📚 Documentation

### **Available Guides**
- **README.md** - This file (overview)
- **QUICK_START_V2.5.md** - Detailed usage guide with screenshots
- **CHANGELOG.md** - Full version history

### **Educational Resources**

The calculator includes built-in educational content explaining:
- Erasure coding fundamentals
- RAID level comparisons
- Failure tolerance concepts
- Durability calculations (MTTDL)
- Performance trade-offs

---

## 🛣️ Roadmap

### **Planned Features**

#### **v2.6.0 - Enhanced Object Storage**
- [ ] Additional Cleversafe schemes (6/4, 8/6, 20/14)
- [ ] Multi-site capacity distribution
- [ ] Advanced durability modeling
- [ ] Performance comparison charts

#### **v2.7.0 - Additional Vendors** (Private version)
- [ ] Scality RING support
- [ ] Scality Artesca support
- [ ] Vendor comparison mode

#### **v2.8.0 - File Storage** (Private version)
- [ ] SMB/NFS calculator
- [ ] File-level RAID configurations
- [ ] Snapshot overhead calculations

---

## 🤝 Contributing

This is the **Open Source Edition** featuring core RAID and Cleversafe calculators.

**For collaboration, custom calculators, or enterprise implementations:**
- 💼 LinkedIn: [linkedin.com/in/dennykalaf](https://linkedin.com/in/dennykalaf)

---

## 📄 License

MIT License

Copyright (c) 2025 Dennis A. Kalaf

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## 🙏 Acknowledgments

Built with passion for storage architecture and technical excellence.

**Technology Architect | Storage Specialist | 20+ Years Experience**

Expertise in:
- Erasure coding algorithms
- Distributed storage systems
- RAID configurations
- Capacity planning
- Failure domain analysis

---

**PreSales Calculator v2.5.3 - Open Source Edition | Last Updated: October 2025**
