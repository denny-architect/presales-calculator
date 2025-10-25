# RAID Calculator Technical Specification

**PreSales Calculator v2.5.3 - Block Storage Calculator**

## Document Overview

This specification documents the RAID calculator implementation including capacity formulas, failure tolerance logic, edge cases, and testing methodology. This serves as the authoritative reference for understanding, maintaining, and extending the RAID calculator functionality.

---

## 1. Supported RAID Levels

### 1.1 Standard RAID Levels

| RAID Level | Name | Description | Min Drives | Usable Formula | Efficiency |
|------------|------|-------------|------------|----------------|------------|
| **RAID 0** | Striping | 100% usable, no redundancy | 2 | `1.0` (100%) | 100% |
| **RAID 1** | Mirroring | Full redundancy, 50% usable | 2 | `0.5` (50%) | 50% |
| **RAID 5** | Single Parity | One parity drive | 3 | `(N-1)/N` | 67-94% |
| **RAID 6** | Double Parity | Two parity drives | 4 | `(N-2)/N` | 50-88% |
| **RAID 10** | Mirrored Stripes | Mirroring + striping | 4 | `0.5` (50%) | 50% |

### 1.2 Nested RAID Levels

| RAID Level | Name | Description | Min Drives | Min Groups | Usable Formula |
|------------|------|-------------|------------|------------|----------------|
| **RAID 50** | Striped RAID 5 | Multiple RAID 5 groups striped | 6 | 2 | `(DPG-1)/DPG` |
| **RAID 60** | Striped RAID 6 | Multiple RAID 6 groups striped | 8 | 2 | `(DPG-2)/DPG` |

**Where:**
- `N` = Total number of drives
- `DPG` = Drives Per Group = `floor(N / NumGroups)`

---

## 2. Capacity Calculation Formulas

### 2.1 Raw Capacity Calculation

```javascript
// Formula
rawCapacityBytes = driveCapacity × unitConversionFactor × numDrives

// Example: 10 drives × 8 TB each
rawCapacityBytes = 8 × (1000^4) × 10 = 80,000,000,000,000 bytes
```

### 2.2 Usable Capacity by RAID Level

#### RAID 0 (Striping)
```javascript
usableRatio = 1.0  // 100% usable
usableCapacity = rawCapacity × 1.0
```

#### RAID 1 (Mirroring)
```javascript
usableRatio = 0.5  // 50% usable
usableCapacity = rawCapacity × 0.5
```

#### RAID 5 (Single Parity)
```javascript
usableRatio = (N - 1) / N
// Examples:
// 3 drives: (3-1)/3 = 66.7%
// 5 drives: (5-1)/5 = 80.0%
// 10 drives: (10-1)/10 = 90.0%
```

#### RAID 6 (Double Parity)
```javascript
usableRatio = (N - 2) / N
// Examples:
// 4 drives: (4-2)/4 = 50.0%
// 6 drives: (6-2)/6 = 66.7%
// 10 drives: (10-2)/10 = 80.0%
```

#### RAID 10 (Mirrored Stripes)
```javascript
usableRatio = 0.5  // 50% usable (mirroring overhead)
usableCapacity = rawCapacity × 0.5
```

#### RAID 50 (Striped RAID 5 Groups)
```javascript
drivesPerGroup = floor(N / NumGroups)
usableRatio = (drivesPerGroup - 1) / drivesPerGroup

// Example: 12 drives, 2 groups
// drivesPerGroup = 12/2 = 6
// usableRatio = (6-1)/6 = 83.3%
```

#### RAID 60 (Striped RAID 6 Groups)
```javascript
drivesPerGroup = floor(N / NumGroups)
usableRatio = (drivesPerGroup - 2) / drivesPerGroup

// Example: 12 drives, 2 groups
// drivesPerGroup = 12/2 = 6
// usableRatio = (6-2)/6 = 66.7%
```

### 2.3 Overhead Calculation

```javascript
overheadBytes = rawCapacityBytes - usableCapacityBytes
overheadPercent = (overheadBytes / rawCapacityBytes) × 100
efficiencyPercent = (usableBytes / rawBytes) × 100
```

---

## 3. Failure Tolerance Logic

### 3.1 Drive Failure Tolerance by RAID Level

| RAID Level | Drives Tolerated | Notes |
|------------|------------------|-------|
| **RAID 0** | 0 drives | **Any drive failure = total data loss** |
| **RAID 1** | 1 drive | Can lose 50% of array (one drive in mirror) |
| **RAID 5** | 1 drive | Single parity allows reconstruction from N-1 drives |
| **RAID 6** | 2 drives | Double parity allows reconstruction from N-2 drives |
| **RAID 10** | 1 drive per mirror pair | Can survive multiple failures if not in same pair |
| **RAID 50** | 1 drive per RAID 5 group | Multiple group tolerance improves resilience |
| **RAID 60** | 2 drives per RAID 6 group | Highest resilience of nested RAID |

### 3.2 Protection Characteristics

#### RAID 0
```
Protection: None
Risk: Total data loss on any single drive failure
Use Case: Maximum performance, non-critical data, temporary storage
```

#### RAID 1
```
Protection: Full data duplication
Rebuild: Can rebuild from surviving mirror drive
Performance: Improved read, write penalty
Use Case: Boot drives, critical databases, high availability
```

#### RAID 5
```
Protection: Single parity drive
Parity Overhead: 1/N (e.g., 10 drives = 10% overhead)
Rebuild: Array can rebuild from any N-1 surviving drives
Risk: Vulnerable during rebuild (URE risk on large arrays)
Use Case: General-purpose storage, balanced capacity and protection
```

#### RAID 6
```
Protection: Double parity drives
Parity Overhead: 2/N (e.g., 10 drives = 20% overhead)
Rebuild: Array can rebuild from any N-2 surviving drives
Risk: Lower rebuild risk, can survive second failure during rebuild
Use Case: Large arrays, mission-critical data, low rebuild risk tolerance
```

#### RAID 10
```
Protection: Full mirroring across stripe sets
Mirror Overhead: 50% (fixed)
Rebuild: Fast rebuild from mirrored pair
Performance: Excellent read/write performance
Use Case: High-performance databases, transaction logs, VDI
```

---

## 4. Unit Conversion System

### 4.1 Supported Units

**Decimal Units (SI):**
- Bytes (B): `1` byte
- Kilobytes (KB): `1,000` bytes (`10^3`)
- Megabytes (MB): `1,000,000` bytes (`10^6`)
- Gigabytes (GB): `1,000,000,000` bytes (`10^9`)
- Terabytes (TB): `1,000,000,000,000` bytes (`10^12`)
- Petabytes (PB): `1,000,000,000,000,000` bytes (`10^15`)
- Exabytes (EB): `1,000,000,000,000,000,000` bytes (`10^18`)

**Binary Units (IEC):**
- Kibibytes (KiB): `1,024` bytes (`2^10`)
- Mebibytes (MiB): `1,048,576` bytes (`2^20`)
- Gibibytes (GiB): `1,073,741,824` bytes (`2^30`)
- Tebibytes (TiB): `1,099,511,627,776` bytes (`2^40`)
- Pebibytes (PiB): `1,125,899,906,842,624` bytes (`2^50`)
- Exbibytes (EiB): `1,152,921,504,606,846,976` bytes (`2^60`)

### 4.2 Precision Handling

**Implementation:**
```javascript
// Uses Decimal.js for arbitrary-precision arithmetic
const unitConversions = {
    'B': new Decimal(1),
    'KB': new Decimal(1000),
    'MB': new Decimal(1000).pow(2),
    'GB': new Decimal(1000).pow(3),
    'TB': new Decimal(1000).pow(4),
    'PB': new Decimal(1000).pow(5),
    'EB': new Decimal(1000).pow(6),
    'KiB': new Decimal(1024),
    'MiB': new Decimal(1024).pow(2),
    'GiB': new Decimal(1024).pow(3),
    'TiB': new Decimal(1024).pow(4),
    'PiB': new Decimal(1024).pow(5),
    'EiB': new Decimal(1024).pow(6)
};
```

**Why Decimal.js?**
- Avoids JavaScript floating-point precision errors
- Handles very large numbers (exabyte-scale) accurately
- Preserves precision in multi-step calculations

### 4.3 Storage Aggregation

**Multi-Unit Summation:**
```javascript
// Example: Sum 5.5 TB + 2000 GB + 1024 GiB
totalBytes = (5.5 × 10^12) + (2000 × 10^9) + (1024 × 2^30)
totalBytes = 5,500,000,000,000 + 2,000,000,000,000 + 1,099,511,627,776
totalBytes = 8,599,511,627,776 bytes

// Convert to output unit (TB):
totalTB = 8,599,511,627,776 / (10^12) = 8.60 TB
```

---

## 5. Edge Cases and Validation

### 5.1 Input Validation Rules

| Validation | Rule | Error Message |
|------------|------|---------------|
| **Minimum Drives** | `numDrives >= raidConfig.minDrives` | "RAID X requires minimum Y drives. You have Z drives." |
| **Minimum Groups** | `numGroups >= raidConfig.minGroups` | "RAID X requires minimum Y groups." |
| **Even Distribution** | `numDrives % numGroups === 0` | "Number of drives must be evenly divisible by number of groups." |
| **Positive Capacity** | `driveCapacity > 0` | "Drive capacity must be greater than zero." |
| **Positive Drive Count** | `numDrives >= 1` | "Number of drives must be at least 1." |

### 5.2 Minimum Drive Requirements

```javascript
const raidFormulas = {
    '0': { minDrives: 2 },   // Need at least 2 drives to stripe
    '1': { minDrives: 2 },   // Need 2 drives to mirror
    '5': { minDrives: 3 },   // Need 3 drives (2 data + 1 parity)
    '6': { minDrives: 4 },   // Need 4 drives (2 data + 2 parity)
    '10': { minDrives: 4 },  // Need 4 drives (2 mirrored pairs)
    '50': { minDrives: 6, minGroups: 2 },  // 2 groups × 3 drives min
    '60': { minDrives: 8, minGroups: 2 }   // 2 groups × 4 drives min
};
```

### 5.3 RAID 50/60 Special Cases

**Group Distribution:**
```javascript
// RAID 50: 12 drives, 3 groups
drivesPerGroup = 12 / 3 = 4 drives per group
// Each group: RAID 5 with 4 drives = (4-1)/4 = 75% efficiency

// RAID 60: 12 drives, 2 groups  
drivesPerGroup = 12 / 2 = 6 drives per group
// Each group: RAID 6 with 6 drives = (6-2)/6 = 66.7% efficiency
```

**Invalid Configuration Example:**
```javascript
// RAID 50: 10 drives, 3 groups
drivesPerGroup = 10 / 3 = 3.333... (not evenly divisible)
// ERROR: "Number of drives must be evenly divisible by number of groups."
```

---

## 6. Test Cases and Validation

### 6.1 Unit Conversion Test Matrix

From `test-raid.html`:

| Input | Unit | Expected Bytes | Pass Criteria |
|-------|------|----------------|---------------|
| 1 | B | 1 | Exact match |
| 1 | KB | 1,000 | Exact match |
| 1 | MB | 1,000,000 | Exact match |
| 1 | GB | 1,000,000,000 | Exact match |
| 1 | TB | 1,000,000,000,000 | Exact match |
| 1 | KiB | 1,024 | Exact match |
| 1 | MiB | 1,048,576 | Exact match |
| 1 | GiB | 1,073,741,824 | Exact match |
| 1 | TiB | 1,099,511,627,776 | Exact match |

### 6.2 RAID Calculation Test Matrix

| Test | RAID | Drives | Capacity | Expected Usable | Expected Efficiency |
|------|------|--------|----------|-----------------|---------------------|
| RAID 0 Basic | 0 | 4 | 1 TB | 4.00 TB | 100.0% |
| RAID 1 Basic | 1 | 2 | 1 TB | 1.00 TB | 50.0% |
| RAID 5 Min | 5 | 3 | 1 TB | 2.00 TB | 66.7% |
| RAID 5 Standard | 5 | 5 | 1 TB | 4.00 TB | 80.0% |
| RAID 5 Large | 5 | 10 | 1 TB | 9.00 TB | 90.0% |
| RAID 6 Min | 6 | 4 | 1 TB | 2.00 TB | 50.0% |
| RAID 6 Standard | 6 | 6 | 1 TB | 4.00 TB | 66.7% |
| RAID 6 Large | 6 | 10 | 1 TB | 8.00 TB | 80.0% |
| RAID 10 Min | 10 | 4 | 1 TB | 2.00 TB | 50.0% |
| RAID 10 Large | 10 | 8 | 1 TB | 4.00 TB | 50.0% |

### 6.3 Edge Case Tests

```javascript
// Test: RAID 5 with insufficient drives
Input: RAID 5, 2 drives, 1 TB each
Expected: Error message "RAID 5 requires minimum 3 drives. You have 2 drives."

// Test: RAID 50 uneven distribution
Input: RAID 50, 10 drives, 3 groups
Expected: Error message "Number of drives must be evenly divisible by number of groups."

// Test: Zero drive capacity
Input: RAID 5, 5 drives, 0 TB each
Expected: All results show "—", explanation shows "Enter valid drive configuration"

// Test: Negative drives
Input: RAID 5, -5 drives, 1 TB each
Expected: Input validation prevents negative values
```

---

## 7. UI/UX Implementation Notes

### 7.1 Storage Aggregation Feature

**Purpose:** Allow users to sum heterogeneous storage units before calculating RAID capacity.

**Use Cases:**
- "I have 10×8TB drives + 5×12TB drives, what's my RAID 6 usable?"
- "Sum 3TB + 2000GB + 1024GiB for total raw capacity"

**Implementation:**
```javascript
// Dynamic row addition
function addStorageRow() {
    // Creates input row with quantity + unit dropdown
    // Auto-calculates on any change
}

// Aggregation across units
function calculateStorageAggregation() {
    let totalBytes = new Decimal(0);
    rows.forEach(row => {
        totalBytes = totalBytes.plus(quantity.times(unitBytes));
    });
    // Display in selected output unit
}
```

### 7.2 Dynamic RAID Group Configuration

**RAID 50/60 Only:**
```javascript
function handleRaidLevelChange() {
    const raidLevel = document.getElementById('raid-level').value;
    const groupsContainer = document.getElementById('raid-groups-container');
    
    if (raidLevel === '50' || raidLevel === '60') {
        groupsContainer.style.display = 'block';  // Show group selector
    } else {
        groupsContainer.style.display = 'none';   // Hide for other RAID levels
    }
}
```

### 7.3 Real-Time Calculation Updates

**Trigger Events:**
- `oninput` on drive count/capacity inputs
- `onchange` on dropdown selectors (RAID level, units, groups)

**Calculation Flow:**
```
User Input → Validate → Calculate → Update UI
             ↓
          Show Errors (if validation fails)
```

---

## 8. JavaScript Implementation Reference

### 8.1 Core Calculation Function

```javascript
function calculateRAID() {
    // 1. Gather inputs
    const numDrives = parseInt(document.getElementById('raid-num-drives').value);
    const driveCapacity = parseFloat(document.getElementById('raid-drive-capacity').value);
    const driveUnit = document.getElementById('raid-drive-unit').value;
    const raidLevel = document.getElementById('raid-level').value;
    const outputUnit = document.getElementById('output-unit').value;

    // 2. Validate inputs
    if (!numDrives || !driveCapacity || numDrives < 1 || driveCapacity <= 0) {
        // Display "—" placeholders and return
        return;
    }

    // 3. Get RAID configuration
    const raidConfig = raidFormulas[raidLevel];
    
    // 4. Check minimum drives
    if (numDrives < raidConfig.minDrives) {
        // Display error message
        return;
    }

    // 5. Calculate raw capacity
    const driveUnitBytes = unitConversions[driveUnit];
    const totalRawBytes = new Decimal(driveCapacity).times(driveUnitBytes).times(numDrives);

    // 6. Calculate usable ratio (handle RAID 50/60 groups)
    let usableRatio;
    if (raidLevel === '50' || raidLevel === '60') {
        const numGroups = parseInt(document.getElementById('raid-num-groups').value) || 2;
        // Validate groups and distribution
        usableRatio = raidConfig.usable(numDrives, numGroups);
    } else {
        usableRatio = raidConfig.usable(numDrives);
    }

    // 7. Calculate final values
    const usableBytes = totalRawBytes.times(usableRatio);
    const outputUnitBytes = unitConversions[outputUnit];
    const usableCapacityFormatted = usableBytes.div(outputUnitBytes);
    const efficiencyPercent = usableRatio.times(100);

    // 8. Update UI with results
    document.getElementById('raid-usable-capacity').textContent = usableCapacityFormatted.toFixed(2);
    document.getElementById('raid-efficiency').textContent = efficiencyPercent.toFixed(1);
    // ... update other UI elements
}
```

### 8.2 RAID Formulas Object

```javascript
const raidFormulas = {
    '0': {
        name: 'RAID 0',
        description: 'Striping - 100% usable, no redundancy',
        usable: (n) => new Decimal(1),
        minDrives: 2,
        protection: 'None - Any drive failure causes total data loss'
    },
    '1': {
        name: 'RAID 1',
        description: 'Mirroring - 50% usable, full redundancy',
        usable: (n) => new Decimal(0.5),
        minDrives: 2,
        protection: 'Can lose 1 drive (50% of array)'
    },
    '5': {
        name: 'RAID 5',
        description: 'Single parity - (N-1)/N usable',
        usable: (n) => new Decimal(n - 1).div(n),
        minDrives: 3,
        protection: 'Can lose 1 drive'
    },
    '6': {
        name: 'RAID 6',
        description: 'Double parity - (N-2)/N usable',
        usable: (n) => new Decimal(n - 2).div(n),
        minDrives: 4,
        protection: 'Can lose 2 drives simultaneously'
    },
    '10': {
        name: 'RAID 10',
        description: 'Mirrored stripes - 50% usable',
        usable: (n) => new Decimal(0.5),
        minDrives: 4,
        protection: 'Can lose 1 drive per mirrored pair'
    },
    '50': {
        name: 'RAID 50',
        description: 'Striped RAID 5 groups',
        usable: (n, groups) => {
            const drivesPerGroup = Math.floor(n / groups);
            return new Decimal(drivesPerGroup - 1).div(drivesPerGroup);
        },
        minDrives: 6,
        minGroups: 2,
        protection: 'Can lose 1 drive per RAID 5 group'
    },
    '60': {
        name: 'RAID 60',
        description: 'Striped RAID 6 groups',
        usable: (n, groups) => {
            const drivesPerGroup = Math.floor(n / groups);
            return new Decimal(drivesPerGroup - 2).div(drivesPerGroup);
        },
        minDrives: 8,
        minGroups: 2,
        protection: 'Can lose 2 drives per RAID 6 group'
    }
};
```

---

## 9. Known Limitations and Future Enhancements

### 9.1 Current Limitations

1. **No Hot Spare Modeling:**
   - Calculator doesn't account for hot spare drives
   - Usable capacity assumes all drives are active members

2. **Fixed RAID 10 Efficiency:**
   - Always assumes 50% efficiency (mirroring)
   - Doesn't model RAID 10 far/near/offset variations

3. **No RAID 50/60 Nested Calculations:**
   - Doesn't show per-group breakdown
   - Doesn't calculate optimal group distribution

4. **No Sector Size Considerations:**
   - Assumes standard sector alignment
   - Doesn't account for 4K native drives vs 512e

5. **No Rebuild Time Estimation:**
   - Doesn't model rebuild duration
   - Doesn't calculate URE (Unrecoverable Read Error) risk

### 9.2 Planned Enhancements (Future Versions)

**v2.6.0 - Enhanced RAID Features:**
- [ ] Hot spare calculation (reduce usable by N spare drives)
- [ ] Rebuild time estimation based on drive size and rebuild rate
- [ ] URE risk calculator for RAID 5/6 arrays
- [ ] RAID level comparison mode (show all RAID options side-by-side)

**v2.7.0 - Advanced RAID:**
- [ ] RAID 10 variants (near, far, offset)
- [ ] Triple-parity RAID (RAID-7, ZFS RAIDZ3)
- [ ] Per-group breakdown for RAID 50/60
- [ ] Optimal group distribution calculator

**v2.8.0 - Performance Modeling:**
- [ ] IOPS calculations by RAID level
- [ ] Read/write performance estimates
- [ ] Cache impact modeling
- [ ] Stripe size optimization recommendations

---

## 10. References and Standards

### 10.1 RAID Standards

- **RAID Advisory Board (RAB):** Original RAID level definitions
- **SNIA (Storage Networking Industry Association):** Modern RAID terminology
- **IEEE:** Storage standards and specifications

### 10.2 Capacity Standards

- **IEC 60027-2:** Binary prefixes (KiB, MiB, GiB, TiB, PiB, EiB)
- **SI (International System of Units):** Decimal prefixes (KB, MB, GB, TB, PB, EB)

### 10.3 Related Documentation

- `test-raid.html` - Comprehensive test suite with validation
- `index.html` - Main calculator implementation (lines 1736-1998)
- `README.md` - User-facing documentation

---

## Appendix A: Quick Reference Tables

### A.1 RAID Efficiency Quick Reference

| Drives | RAID 0 | RAID 1 | RAID 5 | RAID 6 | RAID 10 |
|--------|--------|--------|--------|--------|---------|
| 2 | 100% | 50% | N/A | N/A | N/A |
| 3 | 100% | 50% | 66.7% | N/A | N/A |
| 4 | 100% | 50% | 75.0% | 50.0% | 50% |
| 5 | 100% | 50% | 80.0% | 60.0% | 50% |
| 6 | 100% | 50% | 83.3% | 66.7% | 50% |
| 8 | 100% | 50% | 87.5% | 75.0% | 50% |
| 10 | 100% | 50% | 90.0% | 80.0% | 50% |
| 12 | 100% | 50% | 91.7% | 83.3% | 50% |

### A.2 Failure Tolerance Quick Reference

| RAID Level | Simultaneous Drive Failures | Notes |
|------------|----------------------------|-------|
| RAID 0 | 0 | Total data loss on any failure |
| RAID 1 | 1 | 50% of drives can fail |
| RAID 5 | 1 | Vulnerable during rebuild |
| RAID 6 | 2 | Can survive rebuild failures |
| RAID 10 | 1 per mirror pair | Best multi-failure tolerance |
| RAID 50 | 1 per group | Better than RAID 5 |
| RAID 60 | 2 per group | Best resilience |

---

**Document Version:** 1.0  
**Last Updated:** October 25, 2025  
**Maintained By:** PreSales Calculator Project  
**Related Specs:** `CLEVERSAFE_CALCULATOR_SPEC.md`, `SCALITY_RING_SESSION_HANDOFF.md`
