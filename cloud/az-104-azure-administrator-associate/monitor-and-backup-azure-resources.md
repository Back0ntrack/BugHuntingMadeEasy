# Monitor and Backup Azure Resources

Azure Backup is a **managed backup service** that protects:

* Azure VMs
* Azure Files
* SQL Server
* SAP HANA
* On-premises servers
* On-premises VMs

without requiring dedicated backup infrastructure.

```
Azure Resource    
    ↓
Azure Backup    
    ↓
Recovery Services Vault
```

#### Why Azure Backup&#x20;

1. Zero Infrastructure
2. Long term retention
3. Security
4. Encryption
5. Soft Delete
6. No Internet exposure
7. High Availability

### Recovery Service Vault

Azure Backup uses a Recovery Services vault to manage and store the backup data. A vault is a storage-management entity, which provides a simple experience to carry out and monitor backup and restore operations. With Azure Backup, you don't need to worry about deploying or managing storage accounts.

### Snapshot

A snapshot is a point-in-time backup of all disks on the VM. For Azure VMs, Azure Backup uses different extensions for each supporting operating system:

| Extension        | OS      | Description                                                                                                 |
| ---------------- | ------- | ----------------------------------------------------------------------------------------------------------- |
| VM Snapshot      | Windows | The extension works with Volume Shadow Copy Service (VSS) to take a copy of the data on disk and in memory. |
| VM SnapshotLinux | Linux   | The snapshot is a copy of the disk.                                                                         |

### Backup Policy

You can define the backup frequency and retention duration for your backups. Currently, the VM backup can be triggered daily or weekly, and can be stored for multiple years. The backup policy supports two access tiers: _snapshot tier_ and the _vault tier_. By using the Enhanced policy, you can trigger hourly backups.

