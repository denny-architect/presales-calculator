# Cleversafe Calculator Technical Specification

**PreSales Calculator v2.5.3 - Object Storage (Reed-Solomon Erasure Coding)**

## Document Overview

This specification documents the Cleversafe/IBM Cloud Object Storage (COS) calculator implementation including Reed-Solomon Cauchy erasure coding schemes, capacity formulas, failure tolerance analysis, durability calculations, and performance metrics. This serves as the authoritative reference for understanding, maintaining, and extending the Cleversafe calculator functionality.

---

## 1. Erasure Coding Fundamentals

### 1.1 Reed-Solomon Cauchy Overview

**What is Reed-Solomon Cauchy Erasure Coding?**
- Mathematical technique using Galois Field (GF(2⁸)) operations
- Splits data into N slices distributed across W storage nodes
- Requires only K slices to reconstruct original data
- Provides resilience against node, drive, and site failures

**Cleversafe Implementation:**
- Uses optimized Cauchy matrices for encoding/decoding
- Distributes slices across geographically dispersed Storage Nodes
- Supports single-site and multi-site topologies
- Industry-leading storage efficiency vs. traditional replication

### 1.2 Scheme Notation: W-K-R

**Format:** `Width - Threshold - Write Threshold`

**Parameters:**
- **Width (W):** Total number of Storage Nodes where data slices are written
- **Threshold (K):** Minimum Storage Nodes required to reconstruct (read) data
- **Write Threshold (R):** Minimum Storage Nodes that must acknowledge write for success

**Example: 9-6-7 Scheme**
```
Width: 9 Storage Nodes (total slice distribution)
Threshold: 6 Storage Nodes (minimum to read data)
Write Threshold: 7 Storage Nodes (minimum to confirm write)

Parity Slices: W - K = 9 - 6 = 3 slices
Node Read Tolerance: W - K = 3 nodes
Node Write Tolerance: W - R = 2 nodes
Storage Efficiency: K/W = 6/9 = 66.7%
```

### 1.3 Why Write Threshold > Threshold?

**Critical Design Principle:**
```
Write Threshold (R) > Threshold (K)
Ensures: R ≥ K + 1 (typically)

Reason:
- After writing to R nodes, system guarantees K nodes survive for reads
- Even if (R - K) nodes fail immediately post-write, K nodes remain
- Prevents "write acknowledged but data unreadable" scenarios
```

**Example:**
```
9-6-7 scheme:
- Write to 7 nodes (R=7)
- If 1 node fails immediately → 6 nodes remain (K=6) ✓
- Data remains readable

If R=K=6 (bad design):
- Write to 6 nodes
- If 1 node fails immediately → 5 nodes remain < 6 needed ✗
- Data becomes unreadable despite write success!
```

---

## 2. Standard Cleversafe Schemes

### 2.1 Predefined Schemes (Current Implementation)

| Scheme | Width | Threshold | Write | Efficiency | Parity | Node Tolerance | Use Case |
|--------|-------|-----------|-------|------------|--------|----------------|----------|
| **9-6-7** | 9 | 6 | 7 | 66.7% | 3 | 3 nodes | Standard deployment, balanced |
| **14-10-11** | 14 | 10 | 11 | 71.4% | 4 | 4 nodes | High capacity, moderate resilience |
| **16-11-12** | 16 | 11 | 12 | 68.8% | 5 | 5 nodes | Maximum resilience, enterprise |

### 2.2 Scheme Selection Guidelines

**9-6-7 Scheme:**
```
Best For:
- Small to medium deployments (9-20 nodes)
- Balanced capacity and protection
- 3-site configurations (3 nodes per site)

Pros:
- Lower node count requirement
- Good efficiency (66.7%)
- Proven in production

Cons:
- Lower failure tolerance (3 nodes) vs larger schemes
```

**14-10-11 Scheme:**
```
Best For:
- Medium to large deployments (14-30 nodes)
- Maximum storage efficiency
- Cost-sensitive projects

Pros:
- Highest efficiency (71.4%)
- 4-node failure tolerance
- Better capacity utilization

Cons:
- Requires more nodes (14 minimum)
- More complex site distribution
```

**16-11-12 Scheme:**
```
Best For:
- Large enterprise deployments (16+ nodes)
- Maximum resilience requirements
- Mission-critical data

Pros:
- Highest failure tolerance (5 nodes)
- Can survive multiple concurrent failures
- Best for geo-distributed deployments

Cons:
- Lower efficiency (68.8%) vs 14-10-11
- Requires largest node count (16)
```

### 2.3 Custom Scheme Builder

**Validation Rules:**

1. **Width Constraints:**
   ```javascript
   3 ≤ Width ≤ 32
   ```

2. **Threshold Constraints:**
   ```javascript
   1 ≤ Threshold < Width
   Threshold ≥ Width/2 (recommended for meaningful redundancy)
   ```

3. **Write Threshold Constraints:**
   ```javascript
   Threshold < Write Threshold ≤ Width
   Write Threshold ≥ Threshold + 1 (recommended)
   ```

4. **Minimum Parity:**
   ```javascript
   Parity = Width - Threshold
   Parity ≥ 2 (recommended for production use)
   ```

**Example Custom Schemes:**
```
6-4-5:  Small deployment, 2-node tolerance, 66.7% efficiency
12-8-9: Medium deployment, 4-node tolerance, 66.7% efficiency
20-14-15: Large deployment, 6-node tolerance, 70.0% efficiency
```

---

## 3. Capacity Calculations

### 3.1 Raw Capacity Formula

```javascript
rawCapacityTB = Width × DrivesPerNode × DriveCapacityTB

// Example: 9-6-7 scheme, 12 drives/node, 8TB drives
rawCapacityTB = 9 × 12 × 8 = 864 TB
```

### 3.2 Usable Capacity Formula

```javascript
efficiency = Threshold / Width
usableCapacityTB = rawCapacityTB × efficiency

// Example: 9-6-7 scheme
efficiency = 6 / 9 = 0.667 (66.7%)
usableCapacityTB = 864 TB × 0.667 = 576 TB
```

### 3.3 Expansion Factor

```javascript
expansionFactor = Width / Threshold

// Example: 9-6-7 scheme
expansionFactor = 9 / 6 = 1.5×

Meaning: For every 1 TB written by client, 1.5 TB is physically stored
```

### 3.4 Overhead Calculation

```javascript
overheadTB = rawCapacityTB - usableCapacityTB
overheadPercent = ((Width - Threshold) / Width) × 100

// Example: 9-6-7 scheme, 864 TB raw
overheadTB = 864 - 576 = 288 TB
overheadPercent = ((9 - 6) / 9) × 100 = 33.3%
```

### 3.5 Capacity Comparison: EC vs Replication

| Protection Method | Raw Capacity | Usable Capacity | Efficiency | Overhead |
|-------------------|--------------|-----------------|------------|----------|
| **3-way Replication** | 864 TB | 288 TB | 33.3% | 66.7% |
| **9-6-7 Erasure Coding** | 864 TB | 576 TB | 66.7% | 33.3% |
| **14-10-11 Erasure Coding** | 1,344 TB | 960 TB | 71.4% | 28.6% |

**Key Insight:** Erasure coding provides 2-3× better storage efficiency than replication while maintaining similar or better failure tolerance.

---

## 4. Failure Tolerance Analysis

### 4.1 Node Failure Tolerance

#### Read Operations (Node Read Tolerance)
```javascript
nodeReadTolerance = Width - Threshold

// Example: 9-6-7 scheme
nodeReadTolerance = 9 - 6 = 3 nodes

Meaning: System can tolerate loss of 3 Storage Nodes and still read data
         (only need 6 surviving nodes out of 9 total)
```

#### Write Operations (Node Write Tolerance)
```javascript
nodeWriteTolerance = Width - WriteThreshold

// Example: 9-6-7 scheme  
nodeWriteTolerance = 9 - 7 = 2 nodes

Meaning: System can tolerate loss of 2 Storage Nodes during writes
         (need 7 surviving nodes out of 9 to acknowledge writes)
```

#### Rebuild Operations
```javascript
rebuildThreshold = Threshold  // Auto-calculated, same as read threshold

// Example: 9-6-7 scheme
rebuildThreshold = 6 nodes

Meaning: System can rebuild a failed node using any 6 surviving nodes
```

### 4.2 Site Failure Tolerance

**Single-Site Deployment:**
```
Site Tolerance: 0 sites
Risk: Loss of site = total data unavailability
Warning: "⚠️ Single Site Configuration: Zero site failure tolerance"
Use Case: Development, testing, non-critical workloads
```

**3-Site Deployment:**
```javascript
// Node distribution (example 9-6-7 scheme)
nodesPerSite = ceil(Width / 3)  // Round up for uneven distribution
nodesPerSite = ceil(9 / 3) = 3 nodes per site

// Calculate site tolerance
nodesAfterSiteLoss = Width - nodesPerSite = 9 - 3 = 6 nodes
siteTolerance = (nodesAfterSiteLoss >= Threshold) ? 1 : 0
siteTolerance = (6 >= 6) ? 1 : 0 = 1 site ✓

Meaning: Can lose 1 entire site (3 nodes) and still have 6 nodes ≥ threshold
```

**Site Tolerance for Different Schemes:**

| Scheme | Nodes/Site | After Site Loss | Site Tolerance |
|--------|------------|-----------------|----------------|
| 9-6-7 | 3 nodes | 6 nodes ≥ 6 | ✓ 1 site |
| 14-10-11 | 5 nodes | 9 nodes < 10 | ✗ 0 sites |
| 16-11-12 | 6 nodes | 10 nodes < 11 | ✗ 0 sites |

**Key Finding:** Standard 3-site even distribution only provides site tolerance for 9-6-7 scheme. For 14-10-11 and 16-11-12, need custom node distribution or stretched cluster design.

### 4.3 Drive Failure Tolerance

```javascript
driveTolerance = nodeReadTolerance × drivesPerNode

// Example: 9-6-7 scheme, 12 drives per node
driveTolerance = 3 × 12 = 36 drives

Meaning: Can tolerate loss of up to 36 drives (assuming distributed across failed nodes)
```

**Important Caveat:**
- Drive tolerance assumes failures are distributed across different nodes
- If all failed drives are in same node, effective tolerance is lower
- Node-level erasure coding (not drive-level) means losing N nodes = losing N×drives

### 4.4 Concurrent Failure Scenarios

**Scenario 1: Node Failures During Rebuild**
```
Initial: 9 healthy nodes
Event: 1 node fails → 8 nodes remain
Rebuild starts: Need 6 nodes to read, 7 to write
Additional failures tolerated during rebuild:
- Read operations: 8 - 6 = 2 more nodes can fail
- Write operations: 8 - 7 = 1 more node can fail
```

**Scenario 2: Rolling Failures**
```
9-6-7 scheme can tolerate 3 concurrent node failures:
- Nodes A, B, C fail simultaneously
- 6 nodes remain (D, E, F, G, H, I)
- Data readable (6 ≥ threshold of 6) ✓
- Data writable? (6 < write threshold of 7) ✗
  
Result: Read-only mode until 1 node recovers
```

**Scenario 3: Multi-Site Outage**
```
9-6-7 with 3-site deployment (3 nodes per site):
- Site A (3 nodes) fails
- Sites B+C remain (6 nodes total)
- Data readable (6 ≥ 6) ✓
- Data writable (6 < 7) ✗

Result: Survives site loss but enters read-only until nodes recover
```

---

## 5. Durability and Availability

### 5.1 Durability Calculation (MTTDL)

**Mean Time To Data Loss (MTTDL):**

```javascript
// Constants
totalDrives = Width × DrivesPerNode
afrDecimal = AFR / 100  // Convert percentage to decimal
mtbfSingleDrive = 8760 / afrDecimal  // Hours per year / failure rate
systemMTTFF = mtbfSingleDrive / totalDrives  // Mean Time To First Failure

// Durability calculation
nodeFailureTolerance = Width - Threshold
mttdlYears = (systemMTTFF / 8760) × (100 ^ nodeFailureTolerance)

// Example: 9-6-7, 12 drives/node, 0.5% AFR
totalDrives = 9 × 12 = 108 drives
afrDecimal = 0.5 / 100 = 0.005
mtbfSingleDrive = 8760 / 0.005 = 1,752,000 hours
systemMTTFF = 1,752,000 / 108 = 16,222 hours (1.85 years)
nodeFailureTolerance = 9 - 6 = 3 nodes
mttdlYears = (16,222 / 8760) × (100^3) = 1.85 × 1,000,000 = 1.85M years
```

### 5.2 Durability "Nines" Calculation

```javascript
durabilityNines = 9 + nodeFailureTolerance

// Examples:
// 9-6-7 scheme: 9 + 3 = 12 nines (99.9999999999%)
// 14-10-11 scheme: 9 + 4 = 13 nines (99.99999999999%)
// 16-11-12 scheme: 9 + 5 = 14 nines (99.999999999999%)
```

**Durability Percentage:**
```javascript
durabilityPercentage = (1 - 10^(-durabilityNines)) × 100

// Example: 12 nines
durabilityPercentage = (1 - 10^(-12)) × 100 = 99.9999999999%
```

### 5.3 Durability Comparison Table

| Configuration | Node Tolerance | AFR | MTTDL (Years) | Nines | Use Case |
|---------------|----------------|-----|---------------|-------|----------|
| 9-6-7, 0.5% AFR | 3 nodes | 0.5% | ~1.8M | 12 nines | Standard |
| 9-6-7, 1.0% AFR | 3 nodes | 1.0% | ~900K | 12 nines | Conservative |
| 14-10-11, 0.5% AFR | 4 nodes | 0.5% | ~18M | 13 nines | High durability |
| 16-11-12, 0.5% AFR | 5 nodes | 0.5% | ~180M | 14 nines | Ultra-high |

**Interpretation:**
- **12 nines:** 0.000000000001% annual data loss probability
- Industry-leading durability exceeding most compliance requirements
- Comparable to cloud providers (AWS S3 = 11 nines)

---

## 6. Performance Metrics

### 6.1 Write Amplification

```javascript
writeAmplification = Width / Threshold

// Example: 9-6-7 scheme
writeAmplification = 9 / 6 = 1.5×

Meaning: Client writes 1 TB → System writes 1.5 TB across 9 nodes
         (6 data slices + 3 parity slices = 9 total slices)
```

**Write Amplification by Scheme:**

| Scheme | Width/Threshold | Write Amplification | Impact |
|--------|-----------------|---------------------|--------|
| 9-6-7 | 9/6 | 1.5× | Moderate |
| 14-10-11 | 14/10 | 1.4× | Lower (better) |
| 16-11-12 | 16/11 | 1.45× | Moderate |
| 3-way replication | N/A | 3.0× | Very high |

**Key Insight:** Erasure coding reduces write amplification by 50-80% vs. replication.

### 6.2 Rebuild Time Estimation

```javascript
dataPerNodeTB = DrivesPerNode × DriveCapacityTB
rebuildRateMBps = 100  // Conservative estimate (depends on network/disk)
rebuildTimeHours = (dataPerNodeTB × 1024 × 1024) / (rebuildRateMBps × 3600)

// Example: 12 drives × 8TB = 96 TB per node
dataPerNodeTB = 96 TB
rebuildTimeHours = (96 × 1024 × 1024) / (100 × 3600) = 275 hours = 11.5 days

// If faster network (500 MBps):
rebuildTimeHours = (96 × 1024 × 1024) / (500 × 3600) = 55 hours = 2.3 days
```

**Factors Affecting Rebuild Time:**
- Network bandwidth between Storage Nodes
- Source node disk I/O throughput (reading from K nodes)
- Target node disk I/O throughput (writing reconstructed data)
- Number of concurrent rebuild streams
- Background I/O from client operations

### 6.3 Network Overhead

```javascript
networkOverhead = ((Width - Threshold) / Threshold) × 100

// Example: 9-6-7 scheme
networkOverhead = ((9 - 6) / 6) × 100 = 50%

Meaning: Erasure coding adds 50% network traffic compared to local writes
         (distributing 9 slices across network vs 6 slices)
```

**Network Overhead by Scheme:**

| Scheme | Calculation | Network Overhead |
|--------|-------------|------------------|
| 9-6-7 | (9-6)/6 | 50% |
| 14-10-11 | (14-10)/10 | 40% |
| 16-11-12 | (16-11)/11 | 45.5% |

### 6.4 Read Performance

**Parallel Read Optimization:**
```
Normal Read:
- Contact Threshold (K) nodes
- Wait for K slices to arrive
- Reconstruct object
- Latency = MAX(latency of K slowest nodes)

Fast Read Optimization:
- Contact all Width (W) nodes simultaneously
- Use first K slices to arrive
- Ignore slowest (W - K) responses
- Latency = MAX(latency of K fastest nodes)
```

**Expected Performance:**
- First-byte latency: ~10-50ms (network + disk seek)
- Throughput: Scales with number of nodes (parallel reads)
- Cache hit: Sub-millisecond (memory-cached slices)

---

## 7. Site Topology Configurations

### 7.1 Single-Site Deployment

**Configuration:**
```
All Storage Nodes in one datacenter/location
No geographic distribution
```

**Characteristics:**
- ✅ Lower latency (no WAN hops)
- ✅ Higher bandwidth (LAN speeds)
- ✅ Simpler network architecture
- ✅ Lower cost (no cross-site links)
- ❌ Zero site failure tolerance
- ❌ Vulnerable to site-level disasters
- ❌ No geographic data sovereignty

**Use Cases:**
- Development and testing environments
- Non-critical workloads
- Cost-sensitive projects
- High-performance computing (HPC) storage

**Warning Display:**
```html
⚠️ Single Site Configuration: Zero site failure tolerance.
Loss of site results in data unavailability.
```

### 7.2 3-Site Deployment

**Configuration:**
```
Storage Nodes distributed across 3 geographic locations
Typically: Site A (3 nodes) + Site B (3 nodes) + Site C (3 nodes) = 9 nodes
```

**Characteristics:**
- ✅ Site failure tolerance (1 site for 9-6-7 scheme)
- ✅ Geographic disaster protection
- ✅ Data sovereignty compliance
- ✅ Read-after-site-loss capability
- ❌ Higher latency (WAN hops)
- ❌ Lower bandwidth (limited by WAN)
- ❌ Complex network design
- ❌ Higher cost (inter-site links)

**Node Distribution Example (9-6-7):**
```
Site A (East Coast): Nodes 1, 2, 3
Site B (Central):    Nodes 4, 5, 6
Site C (West Coast): Nodes 7, 8, 9

Site A fails → 6 nodes remain (Sites B + C)
Data readable: 6 nodes ≥ threshold (6) ✓
Data writable: 6 nodes < write threshold (7) ✗

Result: Read-only until Site A recovers or nodes rebalanced
```

**3-Site Requirements:**
```
Minimum network: 1 Gbps WAN links (10 Gbps recommended)
Latency tolerance: <50ms RTT between sites (preferred)
Bandwidth: Scales with data ingest rate
```

### 7.3 Stretched Cluster (2-Site) Considerations

**Not Explicitly Implemented (Future Enhancement)**

**Theoretical 2-Site Configuration:**
```
9-6-7 scheme split across 2 sites:
- Site A: 4-5 nodes
- Site B: 4-5 nodes

Problem: Losing either site drops below threshold (6)
Result: No site tolerance with standard scheme

Solution: Use asymmetric schemes or tie-breaker node
- Site A: 5 nodes
- Site B: 5 nodes  
- Tie-breaker: 1 node (lightweight, quorum)
Still doesn't provide true site failure tolerance for 9-6-7
```

**Recommendation:** For 2-site, need larger schemes (14+ nodes) or custom distribution.

---

## 8. Educational Content

### 8.1 Key Concepts Explained

**Width Parameter:**
```
Definition: Total number of Storage Nodes where data is distributed
Impact: More nodes = higher parallelism, better failure tolerance
Trade-off: More nodes = higher network overhead, complexity

Example: Width = 9
Every object written → split into 9 slices → distributed to 9 nodes
Read operation → parallel requests to 9 nodes → first 6 slices used
```

**Threshold Parameter:**
```
Definition: Minimum Storage Nodes needed to reconstruct original data
Impact: Lower threshold = higher failure tolerance (more parity)
Trade-off: Lower threshold = more overhead, less efficiency

Example: Threshold = 6
Need any 6 out of 9 nodes available to read data
Can lose 3 nodes (9 - 6 = 3) without data loss
```

**Write Threshold Parameter:**
```
Definition: Minimum Storage Nodes that must acknowledge write success
Purpose: Ensures durability even if nodes fail immediately after write
Constraint: Must be > Threshold (typically Threshold + 1)

Example: Write Threshold = 7
Write must succeed to 7 nodes before client acknowledgment
If 1 node fails post-write → 6 nodes remain (≥ Threshold) ✓
```

### 8.2 Galois Field Mathematics (Simplified)

**What is GF(2⁸)?**
```
Galois Field 2^8 = finite field with 256 elements {0, 1, ..., 255}
Used for encoding/decoding erasure codes
Enables efficient XOR-based parity calculations
```

**Cauchy Matrices:**
```
Special matrices with all elements from GF(2⁸)
Properties:
- Any subset of rows is linearly independent
- Enables reconstruction from any K slices
- Optimized for CPU operations (XOR-heavy)
```

**Why Cauchy vs. Vandermonde?**
```
Vandermonde matrices:
- Classic Reed-Solomon approach
- Require full matrix inversion
- Computationally expensive

Cauchy matrices (Cleversafe choice):
- XOR-dominant operations (faster on CPUs)
- More efficient encoding/decoding
- Better parallelization
- Industry standard for storage systems
```

---

## 9. Implementation Details

### 9.1 Application State Management

```javascript
// Global state object
const appState = {
    ecConfig: {
        width: 9,
        threshold: 6,
        writeThreshold: 7,
        rebuildThreshold: 6,  // Auto-calculated (= threshold)
        drivesPerNode: 12,
        driveCapacityTB: 8,
        afr: 0.5,
        siteTopology: 'single'  // 'single' or 'three-site'
    }
};
```

### 9.2 Calculation Flow

```javascript
function calculateReedSolomonResults() {
    // 1. Update config from user inputs
    updateScheme();        // Read width/threshold/writeThreshold
    updateECConfig();      // Read drives, capacity, AFR, site topology
    
    // 2. Perform calculations
    calculateCapacity();           // Raw, usable, efficiency, expansion
    calculateFailureTolerance();   // Node, site, drive tolerance
    calculateAvailability();       // MTTDL, durability nines
    calculatePerformance();        // Write amp, rebuild, network overhead
    
    // 3. Generate educational content
    generateEducationalContent();
}
```

### 9.3 Core Calculation Functions

**Capacity Calculation:**
```javascript
function calculateCapacity() {
    const config = appState.ecConfig;
    
    // Raw capacity
    const rawCapacityTB = config.width × config.drivesPerNode × config.driveCapacityTB;
    
    // Storage efficiency
    const efficiency = (config.threshold / config.width) × 100;
    
    // Usable capacity
    const usableCapacityTB = rawCapacityTB × (config.threshold / config.width);
    
    // Expansion factor
    const expansionFactor = config.width / config.threshold;
    
    // Update UI
    document.getElementById('raw-capacity').textContent = rawCapacityTB.toFixed(2);
    document.getElementById('usable-capacity').textContent = usableCapacityTB.toFixed(2);
    document.getElementById('storage-efficiency').textContent = efficiency.toFixed(1);
    document.getElementById('expansion-factor').textContent = expansionFactor.toFixed(2);
}
```

**Failure Tolerance Calculation:**
```javascript
function calculateFailureTolerance() {
    const config = appState.ecConfig;
    
    // Node tolerance
    const nodeReadTolerance = config.width - config.threshold;
    const nodeWriteTolerance = config.width - config.writeThreshold;
    
    // Site tolerance (3-site only)
    let siteTolerance = 0;
    if (config.siteTopology === 'three-site') {
        const nodesPerSite = Math.ceil(config.width / 3);
        const remainingNodes = config.width - nodesPerSite;
        siteTolerance = (remainingNodes >= config.threshold) ? 1 : 0;
    }
    
    // Drive tolerance
    const driveTolerance = nodeReadTolerance × config.drivesPerNode;
    
    // Update UI
    document.getElementById('node-read-tolerance').textContent = `${nodeReadTolerance} nodes`;
    document.getElementById('node-write-tolerance').textContent = `${nodeWriteTolerance} nodes`;
    document.getElementById('site-tolerance').textContent = siteTolerance === 0 ? 'None' : `${siteTolerance} site`;
    document.getElementById('drive-tolerance').textContent = `${driveTolerance} drives`;
}
```

---

## 10. Testing and Validation

### 10.1 Test Scenarios

**Standard Scheme Tests:**
```javascript
// Test 1: 9-6-7 scheme validation
Input: Width=9, Threshold=6, WriteThreshold=7, 12 drives/node, 8TB drives
Expected:
- Raw: 864 TB
- Usable: 576 TB
- Efficiency: 66.7%
- Node Read Tolerance: 3 nodes
- Node Write Tolerance: 2 nodes

// Test 2: 14-10-11 scheme validation
Input: Width=14, Threshold=10, WriteThreshold=11, 12 drives/node, 8TB drives
Expected:
- Raw: 1,344 TB
- Usable: 960 TB
- Efficiency: 71.4%
- Node Read Tolerance: 4 nodes
- Node Write Tolerance: 3 nodes

// Test 3: 16-11-12 scheme validation
Input: Width=16, Threshold=11, WriteThreshold=12, 12 drives/node, 8TB drives
Expected:
- Raw: 1,536 TB
- Usable: 1,056 TB
- Efficiency: 68.8%
- Node Read Tolerance: 5 nodes
- Node Write Tolerance: 4 nodes
```

### 10.2 Edge Case Tests

```javascript
// Test: Minimum viable scheme
Input: Width=3, Threshold=2, WriteThreshold=3
Expected: Calculations work, but warning about low redundancy

// Test: Invalid write threshold (too low)
Input: Width=9, Threshold=6, WriteThreshold=5
Expected: Validation error or warning

// Test: Invalid write threshold (too high)
Input: Width=9, Threshold=6, WriteThreshold=10
Expected: Validation error

// Test: Single-site 3-site toggle
Input: 9-6-7 scheme, toggle between single and 3-site
Expected:
- Single: Site tolerance = "None", warning displayed
- 3-Site: Site tolerance = "1 site", warning hidden
```

### 10.3 Site Tolerance Validation Matrix

| Scheme | Topology | Nodes/Site | Remaining After Loss | Meets Threshold? | Site Tolerance |
|--------|----------|------------|----------------------|------------------|----------------|
| 9-6-7 | 3-site | 3 | 6 | Yes (6 ≥ 6) | 1 site |
| 14-10-11 | 3-site | 5 | 9 | No (9 < 10) | 0 sites |
| 16-11-12 | 3-site | 6 | 10 | No (10 < 11) | 0 sites |

---

## 11. Known Limitations and Future Enhancements

### 11.1 Current Limitations

1. **Limited Standard Schemes:**
   - Only 3 predefined schemes (9-6-7, 14-10-11, 16-11-12)
   - Missing common schemes: 6-4-5, 8-6-7, 12-8-9, 20-14-15

2. **Basic Site Tolerance Logic:**
   - Only supports simple even distribution (ceil(Width/3))
   - Doesn't model custom site distributions
   - No 2-site or 4+ site topologies

3. **Simplified Durability Model:**
   - MTTDL uses simplified exponential formula
   - Doesn't account for correlated failures
   - No URE (Unrecoverable Read Error) modeling

4. **No Multi-Site Capacity Distribution:**
   - Doesn't show per-site capacity breakdown
   - Doesn't calculate WAN bandwidth requirements
   - No site rebalancing logic

5. **No Performance Profiling:**
   - Rebuild time is estimated, not modeled
   - No IOPS or throughput calculations
   - No cache impact modeling

### 11.2 Planned Enhancements

**v2.6.0 - Enhanced Cleversafe Features:**
- [ ] Additional standard schemes (6-4-5, 8-6-7, 12-8-9, 20-14-15)
- [ ] Custom site distribution calculator
- [ ] Multi-site capacity breakdown
- [ ] WAN bandwidth estimation

**v2.7.0 - Advanced Durability:**
- [ ] URE risk modeling (drive read errors during rebuild)
- [ ] Correlated failure analysis
- [ ] Multi-region disaster scenarios
- [ ] Monte Carlo durability simulation

**v2.8.0 - Performance Deep-Dive:**
- [ ] IOPS calculator by workload type
- [ ] Throughput modeling (sequential vs random)
- [ ] Cache layer impact analysis
- [ ] Rebuild time under load

---

## 12. References and Resources

### 12.1 Academic Papers

- **"Erasure Codes for Storage Applications"** - James S. Plank
- **"A Tutorial on Reed-Solomon Coding for Fault-Tolerance in RAID-like Systems"** - Plank
- **"Cauchy Reed-Solomon Codes"** - Bloemer et al.

### 12.2 Industry Standards

- **SNIA (Storage Networking Industry Association)** - Object storage standards
- **IEEE** - Reliability and durability calculations
- **ISO/IEC 14543** - Data encoding standards

### 12.3 Cleversafe/IBM Documentation

- IBM Cloud Object Storage (COS) Architecture
- Cleversafe Dispersed Storage Technical Overview
- Reed-Solomon Coding in Production Systems

### 12.4 Related Project Documentation

- `index.html` - Main calculator implementation (lines 1115-1735)
- `RAID_CALCULATOR_SPEC.md` - RAID calculator specification
- `SCALITY_RING_SESSION_HANDOFF.md` - Scality RING calculator spec
- `README.md` - User-facing documentation

---

## Appendix A: Quick Reference Formulas

### A.1 Capacity Formulas

```javascript
Raw Capacity = Width × DrivesPerNode × DriveCapacityTB
Usable Capacity = Raw Capacity × (Threshold / Width)
Storage Efficiency = (Threshold / Width) × 100
Expansion Factor = Width / Threshold
Overhead = Raw - Usable = Raw × ((Width - Threshold) / Width)
```

### A.2 Failure Tolerance Formulas

```javascript
Node Read Tolerance = Width - Threshold
Node Write Tolerance = Width - WriteThreshold
Drive Tolerance = Node Read Tolerance × DrivesPerNode
Site Tolerance (3-site) = (Width - ceil(Width/3) >= Threshold) ? 1 : 0
```

### A.3 Performance Formulas

```javascript
Write Amplification = Width / Threshold
Network Overhead = ((Width - Threshold) / Threshold) × 100
Rebuild Time Hours = (DataPerNode TB × 1024 × 1024) / (RebuildRate MBps × 3600)
```

### A.4 Durability Formulas

```javascript
MTTFF Hours = (8760 / AFR) / TotalDrives
MTTDL Years = (MTTFF / 8760) × (100 ^ NodeReadTolerance)
Durability Nines = 9 + NodeReadTolerance
Durability % = (1 - 10^(-Nines)) × 100
```

---

**Document Version:** 1.0  
**Last Updated:** October 25, 2025  
**Maintained By:** PreSales Calculator Project  
**Related Specs:** `RAID_CALCULATOR_SPEC.md`, `SCALITY_RING_SESSION_HANDOFF.md`
