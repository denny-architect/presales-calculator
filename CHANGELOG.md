# Changelog

## v2.7 - October 2025

### Added
- **Scality ARTESCA Calculator** - S3 object storage with Kubernetes
- 8 use cases: GENERIC, VEEAM (1MB/4MB), CommVault, CTERA, Rubrik, Cohesity, Splunk
- Node options: 1, 3, 6
- 5 HPE Alletra models: 4120-24LFF, 4120-48SFF, 4140-60LFF, 4140-84LFF, 4210-18EDSFF
- Dual-level erasure coding: ECB8+2/ECB9+1 (local), ECN2+1/ECN5+1 (network)
- SSD metadata calculator: Xcore, XDM, Veeam-specific
- Overhead: 30.3% (1-node), 71.7% (3-node), 42.4% (6-node)
- Interactive S3 topology visualization with 60fps animation

### Changed
- Updated HPE Alletra drive capacities across RING and ARTESCA
- Added ARTESCA to Object Storage vendor dropdown

---

## v2.6 - October 2025

### Added
- **Scality RING Calculator** with HPE Alletra integration
- **4 RING Topologies:** Mono-Site Standard Durability, Mono-Site Enhanced Durability, 2-Site (Stretched) Enhanced Durability, 3-Site (Stretched) Enhanced Durability
- **5 HPE Alletra Models:** 4120-24LFF, 4120-48SFF, 4140-60LFF, 4140-84LFF, 4210-18EDSFF
- **Interactive Canvas Visualizations:** 60fps animated topology diagrams for all storage types
- **RAID Drive Array Visualization:** Animated drive displays with color-coded data/parity/mirror drives
- **Dynamic Canvas Scrolling:** Supports up to 100+ drives with horizontal scroll
- **Error Display:** Visual error messages in Drive Array Visualization section
- **ARC Erasure Coding:** ARC7+5, ARC9+3, ARC8+4, ARC5+7 schemes
- **COS Replication Policies:** COS2, COS3 for metadata protection
- **Keyspace Alignment Warnings:** Configuration optimization guidance

### Changed
- Updated RAID calculator with RAID 50/60 group visualization separators
- Enhanced Cleversafe calculator with distributed parity animation
- Improved dark mode styling consistency
- Updated all documentation to remove advertising content

### Technical
- Single-file HTML application (~120 KB)
- HTML5 Canvas for animations
- Decimal.js for precision arithmetic
- No build process required

---

**PreSales Calculator v2.7** - MIT License - Open source storage capacity analysis
