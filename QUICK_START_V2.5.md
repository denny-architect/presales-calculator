# 🚀 Quick Start Guide - PreSales Calculator v2.5.0

Get up and running in **60 seconds** with the PreSales Calculator!

---

## 🎯 Choose Your Calculator

PreSales Calculator v2.5.0 offers three storage protocol calculators:

1. **Block (RAID)** - Traditional RAID configurations
2. **Object (Reed-Solomon EC)** - Erasure coding for object storage
3. **File (SMB/NFS)** - File storage (Coming soon)

---

## 🏃 30-Second Start

### **Step 1: Open the Calculator**
1. Download or open `index.html`
2. Double-click to open in your browser
3. **That's it!** No installation, no server, no dependencies.

### **Step 2: Select Calculator Type**
Click the **"Calculator Type"** dropdown at the top:
- Select **"Block (RAID)"** for RAID calculations
- Select **"Object (Reed-Solomon EC)"** for erasure coding

### **Step 3: Start Calculating!**
- Results update in real-time as you change values
- Hover over ℹ️ icons for explanations
- Scroll down to see educational content

---

## 🧮 Block (RAID) Calculator

### **Quick Example: RAID 6 with 8 Drives**

**Step 1: Select Calculator**
- Calculator Type → **Block (RAID)**

**Step 2: Add Storage**
- Click **"Add Storage Unit"**
- Enter: **8** (quantity)
- Select: **TB** (unit)
- Default shows: **0.00 TB** aggregated

**Step 3: Configure RAID**
- Number of Drives: **8**
- Drive Capacity: **4** TB
- Drive Unit: **TB**
- RAID Level: **RAID 6**

**Step 4: View Results**
```
Raw Capacity: 32 TB (8 drives × 4 TB)
Usable Capacity: 24 TB
Efficiency: 75%
Overhead: 8 TB (25%)
Protection: Can lose 2 drives simultaneously
```

---

### **Storage Aggregation - Multiple Units**

**Example: Mixed Storage Units**

1. Click **"Add Storage Unit"** (3 times)
2. Enter:
   - Row 1: **2** × **TB**
   - Row 2: **500** × **GB**
   - Row 3: **256** × **GiB**

3. **Result:**
```
Aggregated: ~2.775 TB
```

**Why?**
- 2 TB = 2,000,000,000,000 bytes
- 500 GB = 500,000,000,000 bytes
- 256 GiB = 274,877,906,944 bytes
- Total = ~2,774,877,906,944 bytes ≈ 2.775 TB

---

### **All RAID Levels Explained**

#### **RAID 0 - Striping**
```
Example: 4 × 2 TB drives
Raw: 8 TB
Usable: 8 TB (100%)
Protection: None - Any failure = data loss
```

#### **RAID 1 - Mirroring**
```
Example: 2 × 4 TB drives
Raw: 8 TB
Usable: 4 TB (50%)
Protection: Can lose 1 drive
```

#### **RAID 5 - Single Parity**
```
Example: 4 × 2 TB drives
Raw: 8 TB
Usable: 6 TB (75%)
Protection: Can lose 1 drive
Formula: (n-1)/n
```

#### **RAID 6 - Dual Parity**
```
Example: 6 × 3 TB drives
Raw: 18 TB
Usable: 12 TB (66.7%)
Protection: Can lose 2 drives simultaneously
Formula: (n-2)/n
```

#### **RAID 10 - Mirrored Stripes**
```
Example: 4 × 2 TB drives
Raw: 8 TB
Usable: 4 TB (50%)
Protection: Can lose 1 drive per mirrored pair
```

#### **RAID 50 - Striped RAID 5 Groups**
```
Example: 6 drives, 2 groups
Raw: 6 × 2 TB = 12 TB
Groups: 2 RAID 5 groups (3 drives each)
Usable: 8 TB (66.7%)
Protection: Can lose 1 drive per group
```

#### **RAID 60 - Striped RAID 6 Groups**
```
Example: 8 drives, 2 groups
Raw: 8 × 2 TB = 16 TB
Groups: 2 RAID 6 groups (4 drives each)
Usable: 8 TB (50%)
Protection: Can lose 2 drives per group
```

---

## 🌐 Object (Reed-Solomon EC) Calculator - Cleversafe

### **Understanding the 9-6-7 Scheme**

When you first open the Object calculator with Cleversafe selected, you'll see:

```
Scheme Display:
     9    -    6    -    7
   Width  Threshold  Write Threshold
```

**What This Means:**
- **9 Storage Nodes** in your cluster
- **6 nodes** minimum to READ data (can lose 3 nodes)
- **7 nodes** minimum to WRITE data (can lose 2 nodes during write)

---

### **Quick Example: Default 9-6-7 Configuration**

**Step 1: Select Calculator**
- Calculator Type → **Object (Reed-Solomon EC)**

**Step 2: Select Vendor**
- Vendor/Implementation → **Cleversafe**

**Step 3: Use Defaults (or customize)**

**Default Configuration:**
```
Erasure Coding Scheme:
├── Width: 9 (Total Storage Nodes)
├── Threshold: 6 (Read minimum)
└── Write Threshold: 7 (Write minimum)

Storage Node Configuration:
├── Drives per Node: 12
├── Drive Capacity: 8 TB
├── AFR: 0.5%
└── Site Topology: Single Site
```

**Step 4: View Results**

**Capacity Analysis:**
```
Raw Capacity: 864 TB (9 nodes × 12 drives × 8 TB)
Usable Capacity: 576 TB
Storage Efficiency: 66.7%
Expansion Factor: 1.5×
```

**Failure Tolerance:**
```
Node Tolerance (Read): 3 nodes
Node Tolerance (Write): 2 nodes
Site Tolerance: None (single-site)
Drive Tolerance: 36 drives
```

**Availability:**
```
Durability: 12 nines (99.9999999999%)
MTTDL: ~1.85 Million years
```

**Performance:**
```
Write Amplification: 1.5×
Rebuild Time: ~11.6 days
Network Overhead: 50%
```

---

### **Try These Common Scenarios**

#### **Scenario 1: Higher Protection (12-8-9)**

**Goal:** More failure tolerance

**Configuration:**
```
Width: 12
Threshold: 8
Write Threshold: 9
Drives per Node: 12
Drive Capacity: 8 TB
```

**Results:**
```
Raw: 1,152 TB
Usable: 768 TB
Efficiency: 66.7%
Node Tolerance (Read): 4 nodes ← More protection!
Durability: 13 nines
```

---

#### **Scenario 2: Higher Efficiency (8-6-7)**

**Goal:** Less overhead, more usable capacity

**Configuration:**
```
Width: 8
Threshold: 6
Write Threshold: 7
Drives per Node: 12
Drive Capacity: 8 TB
```

**Results:**
```
Raw: 768 TB
Usable: 576 TB
Efficiency: 75% ← Better efficiency!
Node Tolerance (Read): 2 nodes ← Less protection
Durability: 11 nines
```

---

#### **Scenario 3: 3-Site Configuration**

**Goal:** Geographic redundancy

**Configuration:**
```
Width: 9
Threshold: 6
Write Threshold: 7
Site Topology: 3-Site
```

**Results:**
```
Site Tolerance: 1 site
(Assumes 3 nodes per site: 9 nodes ÷ 3 sites)
Can survive loss of entire datacenter
```

---

## 📚 Understanding Key Concepts

### **Width vs Threshold vs Write Threshold**

**Width (9):**
- Total number of Storage Nodes
- Data is sliced and distributed across ALL nodes
- More nodes = more parallel access

**Threshold (6):**
- Minimum nodes needed to READ data
- Can lose (Width - Threshold) nodes
- Example: 9 - 6 = Can lose 3 nodes

**Write Threshold (7):**
- Minimum nodes needed to WRITE successfully
- Always ≥ Threshold
- Ensures data survives immediate failures after write

**Why 7 > 6?**
If we write to 7 nodes and immediately lose 1 node, we still have 6 nodes (≥ Threshold) to read the data.

---

### **Storage Nodes vs Slices**

**Common Misconception:**
"Width = number of slices per object"

**Reality:**
- **Width = number of Storage Nodes**
- Objects are split into **many slices**
- Slices are distributed across **Width nodes**
- Each node may hold multiple slices

**Example:**
- 100 MB object
- Width = 9 nodes
- Object split into ~1,000 slices
- Each node gets ~111 slices

---

### **Drives per Node - Why It Matters**

**More Drives per Node:**
- ✅ Higher capacity per node
- ✅ More I/O parallelism
- ❌ Longer rebuild times
- ❌ Larger failure domain

**Example:**
```
Configuration A: 9 nodes × 12 drives = 108 drives
Configuration B: 9 nodes × 60 drives = 540 drives

Same Width (9), same Threshold (6)
But Configuration B has:
- 5× more capacity
- 5× longer rebuild time
```

---

## 🎓 Educational Features

### **Tooltips Everywhere**
Hover over any ℹ️ icon to see:
- What the parameter means
- Why it matters
- Example values

### **Step-by-Step Explanations**
Every result shows:
- The formula used
- Why the number matters
- What it means for your deployment

### **Warning System**
- 🔴 **Red warnings** for risks (RAID 0, single-site)
- 🟡 **Yellow cautions** for suboptimal configs
- 🟢 **Green highlights** for key metrics

---

## 🧪 Testing Your Knowledge

### **Quick Quiz: RAID**

**Question:** You have 10 × 4 TB drives. What's the usable capacity for RAID 5?

<details>
<summary>Click to reveal answer</summary>

**Answer:** 36 TB

**Calculation:**
- Raw: 10 drives × 4 TB = 40 TB
- RAID 5 formula: (n-1)/n = (10-1)/10 = 9/10 = 90%
- Usable: 40 TB × 0.9 = 36 TB
- Protection: Can lose 1 drive

</details>

---

### **Quick Quiz: Reed-Solomon EC**

**Question:** 12-8-9 scheme with 12 drives per node @ 8 TB. What's usable capacity?

<details>
<summary>Click to reveal answer</summary>

**Answer:** 768 TB

**Calculation:**
- Raw: 12 nodes × 12 drives × 8 TB = 1,152 TB
- Efficiency: 8/12 = 66.67%
- Usable: 1,152 TB × 0.6667 = 768 TB
- Node Tolerance: 12 - 8 = 4 nodes can fail

</details>

---

## 🔥 Pro Tips

### **RAID Calculator**
1. **Use storage aggregation** for mixed storage types
2. **Test RAID 50/60** when you need nested redundancy
3. **Compare efficiencies** side-by-side (manual calculations)
4. **Remember:** RAID 10 = RAID 1+0 (mirrors first, then stripe)

### **Object EC Calculator**
1. **Start with 9-6-7** as your baseline (proven configuration)
2. **Increase Width** for more parallelism, not just protection
3. **3-Site topology** if disaster recovery is critical
4. **Higher drive density** = longer rebuilds (consider carefully)
5. **AFR matters** - Use vendor-specific values if available

---

## 🆘 Troubleshooting

### **RAID Calculator Issues**

**Problem:** "RAID 50 requires at least 6 drives"
- **Solution:** Nested RAID needs minimum drives per group
- RAID 50: 3 drives/group × 2 groups = 6 minimum
- RAID 60: 4 drives/group × 2 groups = 8 minimum

**Problem:** Storage aggregation shows 0.00 TB
- **Solution:** Click "Add Storage Unit" and enter values
- Units must be selected from dropdown
- Supports decimal values (e.g., 2.5 TB)

---

### **Object EC Calculator Issues**

**Problem:** "Write Threshold must be ≥ Threshold"
- **Solution:** Write Threshold ≥ Read Threshold always
- Example: If Threshold = 6, Write Threshold must be 6, 7, 8, or 9

**Problem:** "Width must be ≥ Write Threshold"
- **Solution:** Can't write to more nodes than you have
- Example: If Width = 9, Write Threshold max = 9

---

## 📖 Next Steps

### **Learn More**
- Read **README.md** for full technical details
- Check **V2.5_BUILD_COMPLETE.md** for build summary
- Review **CHANGELOG.md** for version history

### **Advanced Topics**
- MTTDL calculations explained
- Rebuild time estimation formulas
- Site topology best practices
- Drive density optimization

### **Get Help**
- Check tooltips (ℹ️ icons) in the calculator
- Review educational content sections
- Contact: [Your LinkedIn] | [Your Email]

---

## 🎯 Common Use Cases

### **For Customer Proposals**
1. Open calculator
2. Enter customer's drive configuration
3. Compare RAID vs EC efficiency
4. Screenshot results
5. Add to proposal document

### **For Sizing Meetings**
1. Share screen with calculator open
2. Live-adjust parameters with customer
3. Show trade-offs (capacity vs protection)
4. Export results (screenshot or notes)

### **For Training**
1. Start with default configurations
2. Adjust one parameter at a time
3. Observe result changes
4. Read educational explanations
5. Build intuition for storage math

---

## ⚡ Keyboard Shortcuts

- **Tab** - Move between inputs
- **Arrow Up/Down** - Increment/decrement numbers
- **Enter** - No action needed (auto-calculates)

---

## 🎨 UI Tips

### **Dark Theme**
- Designed for extended use
- Reduces eye strain
- Professional appearance for customer demos

### **Responsive Layout**
- Desktop: Two-column layout
- Mobile: Single-column (stacked)
- Works on all screen sizes

### **Color Coding**
- 🔵 **Blue** - Interactive elements
- 🟢 **Green** - Success/key metrics
- 🔴 **Red** - Warnings/risks
- 🟡 **Yellow** - Cautions

---

**Ready to calculate? Open `index.html` and start exploring!** 🚀

---

*Questions? Feedback? Contact [Your Name] via LinkedIn or email.*

*Last Updated: October 2025*
