# Changelog

All notable changes to PreSales Calculator will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [2.5.2] - 2025-10-23

### 🧹 **Cleanup Release**

Removed internal documentation and strategic references from public repository.

### Removed
- 8 internal documentation files (session notes, build logs, guides)
- Strategic vendor references from changelog and documentation
- Files removed: PRIVATE_VERSION_SNAPSHOT.md, PUBLIC_VERSION_READY.md, SESSION_START_GUIDE.md, NEW_SESSION_PROMPT.md, TONIGHT_SUMMARY.md, GIT_CHEAT_SHEET_DENNY.md, V2.5_BUILD_COMPLETE.md, PROGRESS_UPDATE.md

### Changed
- Cleaned CHANGELOG.md to focus on implemented features
- Updated roadmap to reflect current public offering

---

## [2.5.1] - 2025-10-22

### 📝 **Documentation Release**

Minor documentation update to ensure release artifacts reflect current state.

### Changed
- Updated README.md with improved vendor descriptions and contact information
- Updated documentation references from v2.5.0 to v2.5.1
- Refined QUICK_START guide for clarity

### Fixed
- Release documentation now matches current repository state

---

## [2.5.0] - 2025-10-21

### 🎯 **Major Release: Multi-Calculator Platform**

Complete architectural rebuild from v2.1/v2.2 as a "start from scratch" clean implementation. Transformed from single-purpose erasure coding calculator into multi-protocol storage analysis platform.

### Added
- **Multi-calculator architecture** with protocol-based navigation
- **Calculator Type dropdown**: Block (RAID), Object (Reed-Solomon EC), File (SMB/NFS)
- **Vendor/Implementation dropdown** for Object storage calculators
- **Block (RAID) Calculator** - Fully functional
  - Storage Aggregation system with dynamic rows
  - 13 storage units supported (B, KB, KiB, MB, MiB, GB, GiB, TB, TiB, PB, PiB, EB, EiB)
  - RAID levels: 0, 1, 5, 6, 10, 50, 60
  - Nested RAID support (RAID 50/60 with configurable groups)
  - High-precision arithmetic using Decimal.js
  - Mixed unit aggregation (e.g., 2 TB + 500 GB + 256 GiB)
  - Comprehensive capacity calculations and protection explanations
- **Object (Reed-Solomon EC) Calculator** - Cleversafe vendor
  - Carried forward from v2.1/v2.2 with rebranding
  - Width-Threshold-Write Threshold model
  - Storage Node configuration
  - Site topology options (Single vs 3-Site)
  - Comprehensive capacity, failure tolerance, availability, performance calculations
  - Educational content with step-by-step explanations
- **Vendor support architecture**
  - Cleversafe (active)
  - Scality RING (Coming Soon - disabled)
  - Scality Artesca (Coming Soon - disabled)
- **File (SMB/NFS) Calculator placeholder** for future file storage implementations
- **RAID Calculator test suite** (test-raid.html)
  - 38 comprehensive tests
  - Unit conversion validation
  - All RAID level formula validation
  - Storage aggregation testing
  - Edge case coverage
- **Updated CI/CD workflow** (.github/workflows/deploy.yml)
  - Removed outdated BUILD_LOG.md references
  - Added v2.5.0 documentation files
  - Updated version references (v2.0 → v2.5.0)
- **Professional documentation**
  - Comprehensive README.md (13.2 KB)
  - Detailed QUICK_START_V2.5.md (11.6 KB)
  - This CHANGELOG.md

### Changed
- **Complete rebrand** from "IBM COS" to vendor-agnostic terminology
  - "IBM COS Erasure Coding" → "Reed-Solomon Cauchy Erasure Coding"
  - "SliceStor" → "Storage Node" (two words, vendor-agnostic)
  - Removed all IBM branding and trademarks
- **Navigation labels** for protocol-first consistency
  - "RAID Calculator" → "Block (RAID)"
  - "Reed-Solomon EC" → "Object (Reed-Solomon EC)"
  - "Generic EC" → "File (SMB/NFS)"
- **Footer text** to reflect multi-calculator platform
- **HTML comments** updated for v2.5.0 structure
- **Console log messages** to match new naming conventions

### Removed
- **47 legacy files deleted** in cleanup operation:
  - 18 v2.1/v2.2 documentation files
  - 8 icon generation/troubleshooting files
  - 6 vendor research files (not implemented)
  - 5 hackathon/deployment files
  - 5 old GitHub/community files
  - 5 miscellaneous outdated files
- **"Generic" option** from Reed-Solomon vendor dropdown (replaced with specific vendors)
- **Old QUICK_START.md** (replaced with QUICK_START_V2.5.md)
- **index_v2.1_backup.html** (159 KB backup file)
- **All v2.1/v2.2 build artifacts** and intermediate documentation

### Fixed
- **RAID calculator initialization** - Added missing `initializeRAIDCalculator()` call in navigation handler
- **Duplicate navigation event listeners** - Removed redundant event handlers
- **Workflow deployment files** - Updated to reference correct v2.5.0 files

### Technical Details
- **File size:** Main application ~78.9 KB (down from v2.1's 159 KB)
- **Test coverage:** 38/38 RAID calculator tests passing (100%)
- **Dependencies:** Decimal.js (via CDN) for high-precision arithmetic
- **Browser support:** Chrome, Firefox, Safari, Edge (all modern browsers)
- **Architecture:** Single-file HTML application with embedded CSS/JavaScript

### Documentation
- **README.md:** Complete rewrite with v2.5.0 features, architecture, examples
- **QUICK_START_V2.5.md:** Navigation guide, RAID examples, EC scenarios, troubleshooting
- **CHANGELOG.md:** Version history (this file)
- **V2.5_BUILD_COMPLETE.md:** Build summary and progress tracking
- **PROGRESS_UPDATE.md:** Current development status

---

## [2.2.1] - 2025-10-20

### Changed
- UI refinements and template locking
- Known issues documented

### Fixed
- Various UI inconsistencies

---

## [2.2.0] - 2025-10-20

### Changed
- Prepared architecture for multi-vendor support
- Internal enhancement planning

---

## [2.1.0] - 2025-10-19

### Added
- Erasure Coding algorithm implementation
- Width-Threshold-Write Threshold model
- Storage Node configuration
- Site topology options (Single/3-Site)
- Capacity analysis calculations
- Failure tolerance metrics
- Availability & durability calculations (MTTDL, 9's of durability)
- Performance indicators (Write amplification, rebuild time, network overhead)
- Educational content generation
- Step-by-step calculation explanations
- IBM COS specific implementation
- Collapsible AFR (Annual Failure Rate) section
- Real-time scheme display (9-6-7 visual)
- Test scenarios and validation suite

### Technical Details
- IBM Plex Sans typography
- IBM-inspired color palette
- Dark theme UI
- Two-column responsive layout
- Vanilla JavaScript implementation
- Single-file HTML application

---

## [2.0.0] - 2025-10-18

### Added
- Initial project setup
- GitHub Actions CI/CD workflow
- Lighthouse CI integration
- GitHub Pages deployment automation
- Favicon generation system
- Icon troubleshooting tools
- Project documentation structure
- License (MIT)
- Contributing guidelines

### Technical Details
- GitHub Actions workflow for automated deployment
- Lighthouse performance testing
- Release automation with ZIP/TAR.GZ archives

---

## Version History Summary

| Version | Date | Focus | Status |
|---------|------|-------|--------|
| **2.5.0** | 2025-10-21 | Multi-calculator platform rebuild | ✅ Current |
| 2.2.1 | 2025-10-20 | UI refinements | Superseded |
| 2.2.0 | 2025-10-20 | Vendor planning | Superseded |
| 2.1.0 | 2025-10-19 | EC algorithm complete | Superseded |
| 2.0.0 | 2025-10-18 | Initial release | Superseded |

---

## Roadmap

### [2.6.0] - Scality Integration (Planned)
- [ ] Scality RING Reed-Solomon EC calculator
- [ ] Scality Artesca (S3-only) calculator
- [ ] HPE-specific configurations
- [ ] Multi-site topology calculator enhancements

### [2.7.0] - File Storage Calculators (Planned)
- [ ] Qumulo scale-out NAS calculator
- [ ] VAST Data Universal Storage calculator
- [ ] Hammerspace Global Data Platform calculator
- [ ] Generic file storage calculator

### [3.0.0] - Enterprise Features (Planned)
- [ ] TCO (Total Cost of Ownership) calculator
- [ ] Cost per GB/TB calculations
- [ ] PDF report generation
- [ ] Side-by-side comparison mode
- [ ] API for external integrations
- [ ] Multi-language support

---

## Migration Guide

### From v2.1/v2.2 to v2.5.0

**Breaking Changes:**
- File structure completely rebuilt
- All v2.1/v2.2 specific documentation removed
- IBM COS branding removed

**What's Preserved:**
- Reed-Solomon EC calculator functionality (Cleversafe)
- Width-Threshold-Write Threshold model
- Educational content and explanations
- Calculation formulas and accuracy

**New Features:**
- RAID calculator (completely new)
- Multi-calculator navigation
- Vendor selection architecture
- Enhanced documentation

**Action Required:**
- No migration needed if using web interface
- Update any direct links or documentation references
- Update Git submodules if integrated

---

## Acknowledgments

### v2.5.0 Contributors
- Architecture redesign and implementation
- RAID calculator development
- Documentation overhaul
- Test suite creation
- Cleanup and optimization

### Technology Credits
- **Decimal.js** - High-precision arithmetic
- **Google Fonts** - IBM Plex Sans typography
- **GitHub Actions** - CI/CD automation
- **Lighthouse CI** - Performance testing

---

## License

MIT License - See [LICENSE](LICENSE) file for details

---

*For detailed changes in each version, see Git commit history.*
*For upcoming features, see [README.md](README.md) Roadmap section.*
