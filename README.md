# PreSales Calculator v2.7 - Open Source Edition

**Professional storage capacity and performance calculator for pre-sales engineering**

---

## 🎯 **What This Does**

Interactive web-based calculator for storage solution sizing:

- **RAID (Block Storage)**: Traditional RAID levels (0, 1, 5, 6, 10, 50, 60) with hot spare calculations
- **Cleversafe**: Dispersed storage with advanced erasure coding schemes
- **Scality RING**: Object storage with Reed-Solomon erasure coding
- **Scality ARTESCA**: S3-compatible object storage with dual-level erasure coding
- **File Storage**: SMB/NFS capacity planning (coming soon)

---

## 🚀 **Quick Start**

### **Method 1: GitHub Pages (Recommended)**
Visit the live calculator: `https://denny-architect.github.io/presales-calculator/`

### **Method 2: Download & Open**
1. Download `index.html`
2. Open in any modern web browser
3. No server or installation required!

---

## 💻 **Features**

### **RAID Calculator**
- Traditional RAID levels (0, 1, 5, 6, 10, 50, 60)
- Hot spare configuration (dedicated or pooled)
- Usable capacity calculations with overhead
- Real-time topology visualization
- Drive failure tolerance analysis
- Supports HDD and SSD configurations

### **Cleversafe Calculator**
- Dispersed storage technology
- Flexible erasure coding schemes (8+2, 10+4, 12+4, 14+4, 16+4, 20+4)
- Multi-site/multi-datacenter configurations
- Geographic distribution analysis
- Slice-based architecture
- Advanced failure domain tolerance
- Reed-Solomon error correction

### **Scality RING Calculator**
- Object storage with Reed-Solomon erasure coding
- Configurable EC schemes
- Multi-site support
- Site topology optimization
- AFR (Annual Failure Rate) calculations
- Capacity and protection analysis

### **Scality ARTESCA Calculator**
- 9 use case scenarios (Veeam, CommVault, NetBackup, Rubrik, Cohesity, CTERA, Splunk, Generic)
- Dual-level erasure coding (ECB + ECN)
- Performance analysis (write/read throughput, node scaling)
- HPE Alletra 4120/4140/4210 model support
- Animated topology visualization
- XDM metadata requirements
- S3-compatible object storage

---

## 🖥️ **Compatibility**

- ✅ Chrome, Firefox, Safari, Edge (latest versions)
- ✅ Desktop, tablet, mobile responsive
- ✅ No plugins or extensions required
- ✅ Works offline after initial load

---

## 📱 **Responsive Design**

Optimized for:
- MacBook Pro 14" (1512 x 982)
- Standard tablets (768px+)
- iPhone/Android mobile devices (390px+)

---

## 🔒 **Privacy**

- ✅ 100% client-side calculations (no data sent to servers)
- ✅ No tracking or analytics
- ✅ No cookies or local storage
- ✅ Works completely offline

---

## 📖 **Usage**

### **RAID Calculator:**
1. Select RAID level (0, 1, 5, 6, 10, 50, 60)
2. Choose number of drives
3. Select drive capacity (GB/TB)
4. Configure hot spares (optional)
5. View usable capacity and protection details

### **Cleversafe Calculator:**
1. Select erasure coding scheme (8+2 through 20+4)
2. Choose number of sites/datacenters
3. Pick site topology (geographic distribution)
4. Configure dispersed storage parameters
5. View slice distribution and capacity analysis

### **Scality RING Calculator:**
1. Select erasure coding scheme
2. Choose number of sites
3. Pick site topology
4. Configure object access patterns and AFR
5. View Reed-Solomon erasure coding results

### **Scality ARTESCA Calculator:**
1. Select node count (1, 3, or 6 nodes)
2. Choose HPE Alletra model
3. Select drive capacity
4. Pick use case scenario
5. View capacity, protection, and performance results

---

## 🛠️ **Technology Stack**

- Pure HTML5/CSS3/JavaScript (no frameworks)
- Decimal.js for high-precision calculations
- HTML5 Canvas for topology visualizations
- Responsive CSS with media queries

---

## 📝 **License**

MIT License - See LICENSE file for details

---

## 🙏 **Credits**

Built with ⚡ by **Denny Kalaf** | Technology Architect | Storage Specialist

Expertise in:
- Erasure coding algorithms
- Distributed storage systems
- Capacity planning
- Performance optimization

---

## 📧 **Contact**

LinkedIn: [linkedin.com/in/dennykalaf](https://linkedin.com/in/dennykalaf)

---

## 🔄 **Updates**

**v2.7 (October 2025)**
- Added performance analysis for ARTESCA
- Responsive design optimization
- UI/UX improvements
- Enhanced mobile support

---

## ⚠️ **Disclaimer**

This calculator provides estimates based on vendor specifications and typical use cases. Actual results may vary based on specific workload characteristics, hardware configurations, and environmental factors. Always validate sizing with vendor-specific tools and professional services for production deployments.

---

**Last Updated**: October 2025  
**Version**: 2.7
