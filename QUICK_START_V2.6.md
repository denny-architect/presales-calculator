# 🚀 Quick Start Guide - PreSales Calculator v2.6

Welcome to the **PreSales Calculator** - your comprehensive tool for storage analysis across Block (RAID) and Object (Reed-Solomon EC) protocols including Cleversafe and Scality RING.

---

## 📦 Installation

### **Option 1: Direct Download**
1. Download the repository: https://github.com/denny-architect/presales-calculator-public
2. Extract the ZIP file
3. Open `index.html` in your browser

### **Option 2: Git Clone**
```bash
git clone https://github.com/denny-architect/presales-calculator-public.git
cd presales-calculator-public
open index.html
```

**No build process, no dependencies, no npm install!** Just open and use.

---

## 🎯 Using the Block (RAID) Calculator

### **1. Select Calculator Type**
- Click the "Calculator Type" dropdown
- Select **"Block (RAID)"**

### **2. Add Storage Units**
- Click **"Add Storage Unit"** button
- Enter capacity value (e.g., `8`)
- Select unit (e.g., `TB`)
- Add multiple units if needed

### **3. View Aggregated Capacity**
- The "Total Raw Capacity" updates automatically
- Change output unit dropdown to convert (TB, TiB, PB, etc.)

### **4. Configure RAID**
- Select RAID level (0, 1, 5, 6, 10)
- Enter number of drives
- Optionally add hot spare drives

### **5. View Results**
- Usable capacity calculation
- Storage efficiency percentage
- Failure tolerance analysis

---

## 🌐 Using the Object Storage Calculators

### **Step 1: Select Calculator Type**
- Click "Calculator Type" dropdown
- Select **"Object (Reed-Solomon EC)"**

### **Step 2: Select Vendor**
A new "Vendor/Implementation" dropdown appears with options:
- **Cleversafe** - Traditional dispersed storage with custom EC schemes
- **Scality RING** - HPE Alletra-optimized with visual topology (NEW in v2.6!)

---

## 📊 Cleversafe Calculator

### **1. Choose EC Scheme**

#### **Quick Selection - Presets:**
- **9/6** - Recommended for small/medium deployments
- **14/10** - Balanced efficiency and protection
- **16/11** - Higher efficiency for large deployments

#### **Custom Configuration:**
- Click **"Custom"** button
- Set EC Width (total slices)
- Set EC Threshold (minimum slices needed to read)
- Set Write Threshold (slices written for durability)
- System validates configuration automatically

### **2. Configure Storage**
- **Drives per Node:** Number of drives in each storage node (default: 12)
- **Drive Capacity:** Size of each drive in TB (default: 8 TB)

### **3. Advanced: Annual Failure Rate (AFR)**
- Expand the "Advanced Configuration" section
- Adjust AFR percentage (default: 0.5% for enterprise drives)
- Impacts durability and MTTDL calculations

### **4. Deployment Topology**
- **Single Site:** All nodes in one location (⚠️ zero site failure tolerance)
- **3-Site:** Nodes distributed across three geographic locations

### **5. Calculate Results**
Click **"Calculate Results"** to see comprehensive analysis including capacity, failure tolerance, durability, and performance indicators.

---

## ⭕ Scality RING Calculator (NEW in v2.6!)

### **1. Select RING Type**
Choose from 4 deployment architectures:
- **Mono-Site Standard Durability** - Single datacenter, standard durability
- **Mono-Site Enhanced Durability** - Single datacenter with enhanced protection
- **2-Site (Stretched) Enhanced Durability** - Two geographic locations with mirror replication
- **3-Site (Stretched) Enhanced Durability** - Three geographic locations with distributed erasure coding

### **2. Configure Server Count**
- Dropdown auto-populates based on RING type
- Options range from 3-51 servers depending on architecture
- System enforces minimum requirements per RING type

### **3. Select HPE Alletra Model**
Choose from 5 validated configurations:
- **Alletra 4120 24LFF** - 20 data drives (10/12/18/22 TB)
- **Alletra 4120 48SFF** - 44 data drives (2/4/8 TB)
- **Alletra 4140 60LFF** - 56 data drives (10/12/18/22 TB)
- **Alletra 4140 84LFF** - 80 data drives (10/12/18/22 TB)
- **Alletra 4210 18EDSFF** - 15 data drives (15/31 TB)

### **4. Select Drive Capacity**
Dropdown shows validated capacities for selected model

### **5. View Topology Visualization**
Interactive HTML5 Canvas displays:
- **Mono-Site:** Animated particles showing data distribution
- **2-Site:** Ricochet animation demonstrating mirror replication
- **3-Site:** Multi-colored fragments showing distributed erasure coding fragments

### **6. Review Results**
Automatic calculation shows:
- **Capacity Analysis:** Raw, usable, efficiency, overhead
- **Data Resiliency:** EC scheme, COS policy, fault tolerance
- **Educational Context:** Scheme explanations and recommendations
- **Warnings:** Keyspace alignment alerts when applicable

---

## 💡 Tips & Best Practices

### **Cleversafe EC Scheme Selection**

**9/6 (3 parity slices):**
- ✅ Good for: Small deployments (9-20 nodes)
- ✅ Lower network overhead
- ❌ Lower storage efficiency (~66%)

**14/10 (4 parity slices):**
- ✅ Good for: Medium deployments (14-30 nodes)
- ✅ Balanced efficiency (~71%)
- ✅ Better failure tolerance than 9/6

**16/11 (5 parity slices):**
- ✅ Good for: Large deployments (16+ nodes)
- ✅ Higher storage efficiency (~69%)
- ✅ Best for geographically distributed systems

### **Scality RING Architecture Selection**

**Mono-Site (Standard or Enhanced Durability):**
- ✅ Highest storage efficiency (up to 71% with ARC9+3)
- ✅ Lowest latency (single location)
- ❌ No geographic redundancy
- Use: Lab, dev/test, single datacenter production

**2-Site (Stretched) Enhanced Durability:**
- ✅ Survives complete site failure
- ✅ Synchronous mirror replication
- ❌ Lower efficiency (75% with ARC8+4)
- Use: Metro-area DR, active-active datacenters

**3-Site (Stretched) Enhanced Durability:**
- ✅ Survives complete site failure
- ✅ Best efficiency for multi-site (83% with ARC5+7)
- ✅ Optimized distributed erasure coding
- Use: Geographic DR, campus deployments, high availability

### **HPE Alletra Model Selection**

**24LFF / 60LFF / 84LFF (3.5" Large Form Factor):**
- ✅ Best for: High capacity (10-22 TB drives)
- ✅ Lower $/TB
- ❌ Longer rebuild times

**48SFF (2.5" Small Form Factor):**
- ✅ Best for: Performance workloads
- ✅ More drives = better parallelism
- ❌ Lower capacity per drive

**18EDSFF (E1.S Enterprise & Datacenter SSD Form Factor):**
- ✅ Best for: NVMe flash, highest density
- ✅ Highest capacity per drive (15-31 TB)
- Use: Performance-critical applications

---

## 🔧 Troubleshooting

### **Issue: Calculator doesn't load**
- **Check:** Browser JavaScript is enabled
- **Try:** Open in different browser (Chrome, Firefox, Safari, Edge)

### **Issue: Vendor dropdown doesn't appear**
- **Solution:** Make sure "Object (Reed-Solomon EC)" is selected first
- **Note:** Vendor dropdown only appears for Object storage

### **Issue: RING topology visualization doesn't show**
- **Check:** Canvas support in browser (all modern browsers)
- **Try:** Refresh page or clear browser cache

### **Issue: Invalid EC scheme error (Cleversafe)**
- **Check:** EC Width ≥ EC Threshold
- **Check:** Write Threshold ≥ EC Threshold
- **Check:** Write Threshold ≤ EC Width

### **Issue: Keyspace warning appears (RING)**
- **Info:** Warning for Mono-Standard and 3-Site with certain server counts
- **Action:** Consider adjusting to recommended server count (divisible by 12)

---

## 📚 Additional Resources

### **Understanding Erasure Coding**
Erasure coding splits data into N slices and adds M parity slices:
- **EC Width (N+M):** Total slices
- **EC Threshold (N):** Minimum slices needed to reconstruct
- **Parity (M):** Redundant slices for protection

**Example: 14/10**
- Creates 14 total slices
- Needs only 10 to read
- Can lose 4 slices (drives/nodes/sites)

### **Understanding Scality ARC**
Scality's ARC (Asynchronous Replication and Coding):
- **ARC7+5:** 7 data + 5 parity fragments (mono-site, 3 servers)
- **ARC9+3:** 9 data + 3 parity fragments (mono-site, high efficiency)
- **ARC8+4:** 8 data + 4 parity fragments (2-site stretched)
- **ARC5+7:** 5 data + 7 parity fragments (3-site stretched, optimal)

### **Understanding COS Policies**
Copy on Site (COS) replication for metadata and small objects:
- **COS3:** 3% of data replicated 3 times (Mono & 2-Site)
- **COS2:** 3% of data replicated 2 times (3-Site - more efficient)

### **Understanding RAID**
RAID combines multiple drives for performance and/or redundancy:
- **RAID 0:** Striping (no redundancy, maximum capacity/performance)
- **RAID 1:** Mirroring (50% capacity, survives 1 drive failure)
- **RAID 5:** Striping + parity (N-1 capacity, survives 1 drive failure)
- **RAID 6:** Striping + dual parity (N-2 capacity, survives 2 drive failures)
- **RAID 10:** Mirrored stripes (50% capacity, better performance)

---

## 🤝 Getting Help

### **For Questions:**
- 💼 LinkedIn: [linkedin.com/in/dennykalaf](https://linkedin.com/in/dennykalaf)

### **Report Issues:**
- GitHub Issues: https://github.com/denny-architect/presales-calculator-public/issues

---

## 🎓 About the Author

**Denny Kalaf** - Technology Architect & Storage Solutions Expert
- 20+ years in storage architecture and distributed systems
- Expertise: Erasure coding, RAID, capacity planning, object storage
- Patent holder in dispersed storage networks

---

**PreSales Calculator v2.6**

---

*Last Updated: October 2025*
