# 🚀 Quick Start Guide - PreSales Calculator v2.5.3 (Open Source Edition)

Welcome to the **PreSales Calculator** - your comprehensive tool for storage analysis across Block (RAID) and Object (Reed-Solomon EC) protocols.

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

## 🌐 Using the Object (Reed-Solomon EC) Calculator

### **1. Select Calculator Type**
- Click "Calculator Type" dropdown
- Select **"Object (Reed-Solomon EC)"**

### **2. Select Vendor**
- A new "Vendor/Implementation" dropdown appears
- Select **"Cleversafe"** (currently the only active option)

### **3. Choose EC Scheme**

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

### **4. Configure Storage**
- **Drives per Node:** Number of drives in each storage node (default: 12)
- **Drive Capacity:** Size of each drive in TB (default: 8 TB)

### **5. Advanced: Annual Failure Rate (AFR)**
- Expand the "Advanced Configuration" section
- Adjust AFR percentage (default: 0.5% for enterprise drives)
- Impacts durability and MTTDL calculations

### **6. Deployment Topology**
- **Single Site:** All nodes in one location (⚠️ zero site failure tolerance)
- **3-Site:** Nodes distributed across three geographic locations

### **7. Calculate Results**
Click the **"Calculate Results"** button to see:

#### **Capacity Analysis:**
- Raw Capacity (total physical storage)
- Usable Capacity (after erasure coding overhead)
- Storage Efficiency percentage
- Expansion Factor (overhead multiplier)

#### **Failure Tolerance:**
- Storage Nodes (Read) - nodes that can fail while maintaining read access
- Storage Nodes (Write) - nodes that can fail while maintaining write access
- Site Failures - geographic sites that can fail
- Concurrent Drives - drives that can fail simultaneously

#### **Availability & Durability:**
- Data Durability (nines - e.g., 15 nines = 99.999999999999999%)
- MTTDL (Mean Time To Data Loss) in years

#### **Performance Indicators:**
- Write Amplification (network/storage overhead)
- Rebuild Time Estimate (drive replacement time)
- Network Overhead (percentage)

#### **Educational Context:**
- Explanation of your selected scheme
- Strengths and weaknesses
- Use case recommendations

---

## 💡 Tips & Best Practices

### **EC Scheme Selection**

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

### **Site Topology**

**Single Site:**
- ⚠️ **WARNING:** Zero site failure tolerance
- Use only for: Development, non-critical data, cost-sensitive deployments
- Risk: Loss of datacenter = data unavailability

**3-Site:**
- ✅ Survives 1 complete site failure
- Recommended for: Production, critical data, DR requirements
- Requires: Network connectivity between sites

### **Drive Capacity Planning**

**Larger drives (8TB+):**
- ✅ Lower $/TB cost
- ❌ Longer rebuild times
- ❌ Higher impact of single drive failure

**Smaller drives (4TB or less):**
- ✅ Faster rebuild times
- ✅ Lower blast radius per drive
- ❌ Higher $/TB cost

---

## 🔧 Troubleshooting

### **Issue: Calculator doesn't load**
- **Check:** Browser JavaScript is enabled
- **Try:** Open in different browser (Chrome, Firefox, Safari, Edge)

### **Issue: Vendor dropdown doesn't appear**
- **Solution:** Make sure "Object (Reed-Solomon EC)" is selected first
- **Note:** Vendor dropdown only appears for Object storage

### **Issue: Invalid EC scheme error**
- **Check:** EC Width ≥ EC Threshold
- **Check:** Write Threshold ≥ EC Threshold
- **Check:** Write Threshold ≤ EC Width

### **Issue: Results seem incorrect**
- **Verify:** Input values are realistic (drives per node, capacity, AFR)
- **Check:** Output unit selection (TB vs TiB can differ by ~9%)

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

### **Understanding RAID**
RAID combines multiple drives for performance and/or redundancy:
- **RAID 0:** Striping (no redundancy, maximum capacity/performance)
- **RAID 1:** Mirroring (50% capacity, survives 1 drive failure)
- **RAID 5:** Striping + parity (N-1 capacity, survives 1 drive failure)
- **RAID 6:** Striping + dual parity (N-2 capacity, survives 2 drive failures)
- **RAID 10:** Mirrored stripes (50% capacity, better performance)

---

## 🤝 Getting Help

### **For Questions or Custom Calculators:**
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

**PreSales Calculator v2.5.3 - Open Source Edition**

**Note:** This Open Source Edition features Block (RAID) and Object (Cleversafe) calculators. Full private version with additional vendors (Scality, Qumulo, VAST) available on request.

---

*Last Updated: October 2025*
