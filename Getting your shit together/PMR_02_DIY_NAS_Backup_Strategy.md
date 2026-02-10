# Project Migration Record: DIY NAS & Backup Strategy

---

## I. Comprehensive Project Summary

### 1. Project Vision

#### 1.1 High-Level Goals

Build a DIY Network Attached Storage (NAS) system with enterprise-grade data protection to serve as the primary storage foundation for the AstralJunction home lab infrastructure, implementing a comprehensive 3-2-1 backup strategy with local and encrypted offsite backups.

#### 1.2 Motivations

- **Data Protection**: Current storage is scattered across external drives, cloud services, and local devices with no redundancy
- **Consolidation**: Bring all data under one managed system with proper backups
- **Cost Efficiency**: DIY NAS provides better value than COTS solutions (Synology/QNAP)
- **Learning**: Understand ZFS, RAID, and enterprise storage concepts deeply
- **Future-Proofing**: Expandable architecture that can grow with storage needs

#### 1.3 Apps, Tools, Services of Interest

**NAS Software**

- **TrueNAS SCALE** (chosen) - Debian Linux + native ZFS + Docker/Kubernetes support
- *TrueNAS CORE* (considered) - FreeBSD-based, more mature ZFS, less flexible
- *Unraid* (considered) - $59-$129, BTRFS support, great Docker support, not true ZFS

**Backup Tools**

- **restic** (chosen) - Client-side encryption, deduplication, supports B2/SFTP/local
- *rclone* (alternative) - More general-purpose, also good for encrypted backups
- *duplicacy* (alternative) - Lock-free design, commercial and free versions

**Cloud Storage**

- **Backblaze B2** - Object storage at ~$6/TB/month, S3-compatible, cheap egress
- First 3x storage in downloads free per month

**Local Backup**

- Time Machine to external HDD (whole Mac Mini backup)
- Docker volume backups via scripted dumps

#### 1.4 Strategic Objectives

1. Implement 3-2-1 backup rule (3 copies, 2 media types, 1 offsite)
2. Achieve zero-knowledge encryption for offsite backups
3. Maintain clean separation between compute (Mac Mini) and storage (NAS)
4. Enable ZFS benefits: checksums, snapshots, scrubs, bitrot protection
5. Support multiple NAS datasets for different data types

#### 1.5 Long-term Potential / Potential Workflows

- **Centralized storage**: All devices back up to NAS
- **Time Machine target**: MacBook and Mac Mini backup destination
- **Photo backup**: Automatic iPhone photo sync via Immich
- **Media library**: Plex accesses media directly from NAS
- **Document archive**: Paperless-ngx stores documents on NAS
- **Offsite automation**: Nightly encrypted backups to Backblaze B2

#### 1.6 Unique Value Proposition

Enterprise-grade data protection at consumer cost:

- ZFS checksumming prevents silent data corruption (bitrot)
- RAID-Z1 protects against single drive failure
- Encrypted offsite backups protect against disasters
- Full control over hardware and software stack

#### 1.7 Core Philosophies Discussed

- **"RAID is not a backup"** - RAID protects against drive failure, not user error or disasters
- **"3-2-1 is the gold standard"** - Industry-standard backup rule
- **"Zero-knowledge encryption"** - Encrypt client-side, never trust provider with keys
- **"Clean separation of compute and storage"** - Better flexibility, independent upgrades
- **"Staggered drive purchases"** - Buy drives in phases, create separate vdevs

---

### 2. Requirements Compilation

#### 2.1 Explicit Requirements

- RAID-Z1 (equivalent to RAID 5) for single-drive fault tolerance
- ZFS or BTRFS filesystem with checksums
- SMS/email alerts for drive failures
- Protection against bitrot/silent data corruption
- 24-40TB usable capacity
- Encrypted offsite backups

#### 2.2 Implied Requirements

- Network connectivity via Ethernet (Gigabit minimum)
- Low noise operation (home environment)
- Reasonable power consumption
- Good airflow/cooling for drives
- HBA card for direct drive access (not hardware RAID)

#### 2.3 Derived Requirements

- Minimum 4 drives for initial RAID-Z1 vdev
- ECC RAM ideal but not mandatory (cost tradeoff)
- HBA in IT mode (not RAID mode) for ZFS
- 8TB drives minimum for first vdev (to fit current 11TB of data)

#### 2.4 Potential Edge Cases / Expansion Areas

- Adding second RAID-Z1 vdev later (staggered purchase plan)
- Drive failure scenarios and hot spare strategy
- Upgrade path to higher capacity drives
- Potential future RAID-Z2 for dual parity

---

### 3. Architectural Overview

#### 3.1 Current Design Decisions with Rationale

**DIY vs COTS Decision:**
| Option | Verdict | Rationale |
|--------|---------|-----------|
| DIY NAS | ✅ Chosen | Full ZFS control, better value, upgradeability |
| Synology/QNAP | ❌ Rejected | Expensive, limited RAM, proprietary filesystem lock-in |

**Filesystem Choice:**

- **ZFS** chosen over BTRFS
- Rationale: Gold standard for data integrity, checksumming, native RAID-Z support

**RAID Level:**

- **RAID-Z1** chosen (equivalent to RAID 5)
- One drive's worth of parity (can lose 1 drive)
- Decision to skip RAID-Z2: Relies on 3-2-1 backup strategy instead of dual parity

**Architecture:**

```
AstralJunction (Mac Mini) ←─── Ethernet ───→ DIY NAS (TrueNAS SCALE)
     │                                              │
     ├── Docker containers                          ├── tank/photos (Immich data)
     ├── Plex (compute)                             ├── tank/media (Plex library)
     ├── Paperless-ngx (compute)                    ├── tank/documents (Paperless files)
     └── Cloudflare Tunnel                          ├── tank/backups (Time Machine)
                                                    └── tank/nextcloud (file sync)
```

**Storage Architecture:**

- One unified ZFS pool ("tank")
- Multiple datasets: photos, media, documents, backups, nextcloud
- Docker containers mount NAS shares as volumes via NFS/SMB

**Ruled Out Approaches:**

- **Hardware RAID controllers** - ZFS needs direct drive access, HBA in IT mode required
- **Thunderbolt DAS** - NAS connects via network, not direct to Mac Mini
- **Shared database containers** - Separate databases per app for isolation

#### 3.2 Technology Stack

**Hardware Specification (Finalized):**
| Component | Choice | Notes |
|-----------|--------|-------|
| Case | JONSBO N3 | Mini-ITX, 8 drive bays |
| Motherboard | ASUS PRIME H610I-PLUS D4 | Mini-ITX, 2 M.2 slots |
| CPU | Intel i3-13100 | Mid-range, plenty for NAS workload |
| RAM | 32GB DDR4 (non-ECC) | Savings invested in drives |
| HBA | LSI 9300-8i | IT mode, 8 internal ports |
| Boot Drive | 500GB NVMe M.2 SSD | TrueNAS boot, separate from data |
| PSU | Corsair SF750 | SFX, massively overkill but quiet |
| Cooling | Stock + 92mm Noctua + 40mm Noctua (HBA) | Dedicated HBA cooling |

**Software:**

- TrueNAS SCALE (Debian-based)
- ZFS filesystem
- restic for encrypted backups

**Cabling:**

- 2× SFF-8643 → 4× SATA breakout cables
- LSI HBA supports 8 drives total

#### 3.3 Key Architectural Constraints

- ZFS expansion works by adding complete vdevs, not individual drives
- Initial data (~11TB) requires 8TB drives minimum for first vdev
- NAS is I/O-bound, not CPU-bound (CPU doesn't need to match Mac Mini power)
- HBA can run hot (60-70°C+), dedicated cooling recommended

#### 3.4 Potential Future Directions

- Add second 4-drive RAID-Z1 vdev (staggered purchase)
- Implement 10GbE networking if needed
- Add SSD cache (L2ARC) if performance requires
- Consider offsite NAS at friend's house for georedundancy

---

### Current Data Inventory (as of Project Discussion)

**Existing Storage:**

| Location              | Size   | Contents           |
| --------------------- | ------ | ------------------ |
| Mac Mini External HDD | 4TB    | Media files        |
| Mac Mini External SSD | 750GB  | Backups            |
| Google Drive          | ~500GB | Documents, files   |
| Box                   | ~200GB | Work-related files |
| MacBook               | ~256GB | Local files        |
| iPhone                | ~128GB | Photos, apps       |
| Wife's storage        | 4-5TB  | DSLR photos        |

**Total Existing**: ~11TB (Dan) + 4-5TB (wife) = ~15-16TB current

**Projected Growth**: 30-40TB usable capacity target

---

### Backup Strategy

**3-2-1 Rule Implementation:**

| Copy        | Location        | Media Type | Purpose           |
| ----------- | --------------- | ---------- | ----------------- |
| 1 (Primary) | NAS ZFS pool    | HDD array  | Active storage    |
| 2 (Local)   | External drives | USB HDD    | Fast recovery     |
| 3 (Offsite) | Backblaze B2    | Cloud      | Disaster recovery |

**Client-Side Encryption with restic:**

```bash
# Initialize encrypted repo on B2
restic -r b2:my-bucket init

# Backup with encryption
restic -r b2:my-bucket backup /docker-volumes
```

**What Backblaze Sees:**

- ✅ Encrypted binary blobs
- ✅ File sizes, timestamps
- ❌ Cannot see: file contents, names, directory structure

**Backup Automation (Planned):**

- Nightly cron job running restic
- Target: Docker volumes, PostgreSQL dumps, Paperless docs, Immich photos
- Quarterly restore tests to verify backups work

---

## II. Project Roadmap and Backlog

### ✅ Completed

- [x] Decided on DIY NAS over COTS (Synology/QNAP)
- [x] Selected TrueNAS SCALE as NAS operating system
- [x] Finalized hardware specification
- [x] Decided on RAID-Z1 with ZFS
- [x] Confirmed staggered drive purchase strategy
- [x] Selected restic + Backblaze B2 for offsite backup
- [x] Completed data inventory (~11TB existing + growth)
- [x] Determined clean compute/storage separation architecture

### 📋 Next Up (When Ready to Build)

- [ ] Purchase NAS components (case, motherboard, CPU, RAM, HBA, PSU)
- [ ] Purchase initial 4× 8TB drives (for first RAID-Z1 vdev)
- [ ] Assemble NAS hardware
- [ ] Install TrueNAS SCALE
- [ ] Configure ZFS pool with RAID-Z1
- [ ] Create datasets (photos, media, documents, backups, nextcloud)
- [ ] Configure network shares (NFS/SMB)
- [ ] Migrate data from external drives to NAS

### 🔮 Future / Backlog

- [ ] Set up Time Machine backup target on NAS
- [ ] Configure Docker volume mounts to NAS shares
- [ ] Install restic on Mac Mini
- [ ] Initialize encrypted Backblaze B2 repository
- [ ] Create automated backup scripts
- [ ] Set up backup monitoring and alerting
- [ ] Purchase second batch of drives (4× 8TB for second vdev)
- [ ] Configure SMART monitoring and alerts
- [ ] Schedule monthly ZFS scrubs
- [ ] Migrate wife's DSLR photos to NAS

### 🎯 Migration Strategy (Defined)

**Phase 1**: Build NAS, keep externals connected to Mac Mini
**Phase 2**: Migrate data to NAS (copy, don't move initially)
**Phase 3**: Verify all data migrated successfully, configure snapshots
**Phase 4**: Repurpose external drives (offsite rotation or scratch)
**Phase 5**: Configure automated offsite backup to B2

---

## III. Conversation Context Summary

### 3.1 Key Discussion Points

- DIY vs COTS NAS comparison (strongly favored DIY)
- ZFS benefits: checksums, snapshots, scrubs, bitrot protection
- RAID-Z1 vs RAID-Z2 tradeoffs
- ECC vs non-ECC RAM (decided non-ECC, invest savings in drives)
- Staggered drive purchase for budget management
- Clean separation between compute and storage

### 3.2 Notable Insights

- **"NAS is I/O-bound, not CPU-bound"** - Don't need to match Mac Mini CPU power
- **"Client-side encryption = zero-knowledge"** - Backblaze literally can't read your data
- **"RAID is not a backup"** - Different failure modes require different protections
- **"ZFS pool expansion requires full vdevs"** - Can't add single drives, plan vdev sizes

### 3.3 Unresolved Questions

- Exact timing of NAS build (after services are running and have data worth protecting)
- Whether to use NFS or SMB for Mac Mini ↔ NAS communication
- Specific alerting setup for drive failures

### 3.4 Potential Pivot Points

- Could add L2ARC SSD cache if performance insufficient
- Could upgrade to 10GbE if Gigabit becomes bottleneck
- Could consider second offsite location (friend's house)

### 3.5 Technical Decisions Validated

- 8TB drives required for first vdev (4× 4TB = only 12TB usable, too tight for 11TB data)
- HBA needs dedicated cooling (40mm Noctua fan)
- 500GB NVMe for boot is plenty (TrueNAS minimum 16GB)
- SF750 PSU is overkill but enables quiet operation

---

## Cross-References

- **Home Lab Infrastructure**: See PMR #1 (compute side of compute/storage separation)
- **PKM & Supernote**: Documents and notes will be stored on NAS
- **Self-Hosted Services**: All bulk data stored on NAS (Immich photos, Paperless docs, Plex media)
