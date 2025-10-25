# 🧮 PreSales Calculator v2.6 - Open Source Edition

**Multi-Protocol Storage Analysis Tool for Pre-Sales Engineers**

A comprehensive web application for storage architects, pre-sales engineers, and technical teams to understand storage configurations, capacity planning, and failure tolerance analysis across Block, Object, and File storage protocols.

---

## 🌟 Features

### ✅ Current Capabilities (v2.6)

#### **Block (RAID) Calculator**
- Storage unit aggregation with decimal/binary conversion
- Multi-unit capacity summation (B, KB, MB, GB, TB, PB, EB and their binary equivalents)
- Precision decimal calculations using Decimal.js
- Support for RAID 0, 1, 5, 6, 10 configurations
- Usable capacity calculations
- RAID-specific failure tolerance analysis

#### **Object (Reed-Solomon EC) Calculator**
- **Vendor Support:**
  - Cleversafe with standard schemes (9/6, 14/10, 16/11)
  - Scality RING with HPE Alletra integration (NEW in v2.6!)
  
- **Cleversafe Configurations:**
  - Standard schemes with custom builder
  - 3-site deployment topology support
  - Single-site configuration warnings

- **Scality RING Configurations:**
  - 4 RING types: Mono-Site Standard Durability, Mono-Site Enhanced Durability, 2-Site (Stretched) Enhanced Durability, 3-Site (Stretched) Enhanced Durability
  - 5 HPE Alletra models with accurate drive configurations
  - Interactive topology visualizations with HTML5 Canvas
  - Animated datacenter representations showing data flow
  - ARC erasure coding schemes (ARC7+5, ARC9+3, ARC8+4, ARC5+7)
  - COS replication policies (COS2, COS3)
  - Keyspace alignment warnings

- **Analysis Capabilities:**
  - Capacity analysis (raw, usable, efficiency, expansion factor)
  - Failure tolerance (node read/write, site failures, concurrent drives)
  - Data durability calculations (nines, MTTDL)
  - Performance indicators (write amplification, rebuild time, network overhead)
  - Educational context with scheme explanations
  - Visual topology diagrams with animated data flows

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

2. **For Object Storage:**
   - Choose vendor (Cleversafe or Scality RING)
   - Select or customize EC scheme
   - Configure storage parameters
   - View interactive topology visualizations (RING)
   - Click "Calculate Results"

3. **Review Analysis:**
   - Capacity metrics
   - Failure tolerance
   - Durability estimates
   - Performance indicators
   - Educational context
   - Visual topology representation (RING)

---

## 📋 Technical Architecture

### **Technology Stack**
- **Frontend:** Pure HTML5, CSS3, JavaScript (ES6+)
- **Styling:** IBM Plex Sans font, custom CSS with CSS Grid/Flexbox
- **Math Library:** Decimal.js for precision calculations
- **Graphics:** HTML5 Canvas for topology visualizations
- **Icons:** Font Awesome and custom emoji-based system

### **File Structure**
```
presales-calculator-public/
├── index.html                          # Main application (self-contained)
├── test-raid.html                      # RAID calculator test suite
├── test-scality-ring-with-topology.html  # RING calculator development
├── package.json                        # npm metadata
├── README.md                           # This file
├── CHANGELOG.md                        # Version history
└── QUICK_START_V2.6.md                 # Detailed quick start guide
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

### **Scality RING Test Suite**

Test the RING calculator with topology visualizations:

```bash
open test-scality-ring-with-topology.html
```

**Test Coverage:**
- 4 RING type configurations
- 5 HPE Alletra model calculations
- Accuracy validation against live calculator
- Topology visualization rendering
- Animation performance testing

---

## 📚 Documentation

### **Available Guides**
- **README.md** - This file (overview)
- **QUICK_START_V2.6.md** - Detailed usage guide
- **CHANGELOG.md** - Full version history

### **Educational Resources**

The calculator includes built-in educational content explaining:
- Erasure coding fundamentals
- RAID level comparisons
- Failure tolerance concepts
- Durability calculations (MTTDL)
- Performance trade-offs
- Scality ARC (Asynchronous Replication and Coding)
- COS (Copy on Site) policies
- Multi-site topology architectures

---

## 🎨 New in v2.6

### **Scality RING Calculator Integration**
- Full HPE Alletra integration with 5 server models
- 4 RING deployment topologies with official Scality naming
- Interactive Canvas-based topology visualizations:
  - Mono-Site: Animated particle system showing data distribution
  - 2-Site: Ricochet animation demonstrating mirror replication
  - 3-Site: Multi-colored fragments showing distributed erasure coding
- Clean, crisp 60fps animations
- RAID drive array visualization with scrollable canvas
- Educational content distinguishing replication from erasure coding
- Keyspace alignment warnings for optimal configurations

### **Enhanced Documentation**
- Removed advertising content
- Focused on technical capabilities
- Clean professional presentation

---

## 🤝 Contributing

This is an open-source storage calculator for the community.

**For collaboration or questions:**
- 💼 LinkedIn: [linkedin.com/in/dennykalaf](https://linkedin.com/in/dennykalaf)
- 🐙 GitHub: Issues and Pull Requests welcome!

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

**PreSales Calculator v2.6 - Open Source Edition | Last Updated: October 2025**
