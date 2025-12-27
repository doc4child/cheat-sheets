# How to Clean, Partition, and Format a Disk Using DiskPart (Windows)

This article explains each command shown in the provided DiskPart screenshot and walks through the full process of cleaning, partitioning, and formatting a disk using **DiskPart** on Microsoft Windows.

> **Warning:** DiskPart is a powerful utility. The `clean` command permanently deletes all partitions and data on the selected disk. Always double-check the disk number before proceeding.

---

## Prerequisites

- Windows system with administrative access
- Command Prompt opened **as Administrator**
- Target disk correctly identified

---

## Step-by-Step Commands and Explanations

### 1. Open DiskPart

```
diskpart
```

**Explanation:**
Launches the DiskPart command-line disk management utility.

---

### 2. List All Disks

```
list disk
```

**Explanation:**
Displays all storage disks detected by Windows, along with their disk numbers, sizes, and status.

**Example output interpretation:**
- `Disk 0` – 465 GB (likely primary system drive)
- `Disk 1` – 1863 GB (secondary internal drive)
- `Disk 2` – 14 GB (USB flash drive)

The `*` under **GPT** indicates the disk uses a GPT partition style.

---

### 3. Select the Target Disk

```
select disk 2
```

**Explanation:**
Selects **Disk 2** as the active disk. All subsequent commands apply only to this disk.

---

### 4. Clean the Disk

```
clean
```

**Explanation:**
Removes all partitions and volume information from the selected disk. The disk becomes completely unallocated.

---

### 5. Create a Primary Partition

```
create part pri
```

**Explanation:**
Creates a single primary partition using all available space on the disk.

---

### 6. Select the New Partition

```
select part 1
```

**Explanation:**
Selects the first (and only) partition that was just created.

---

### 7. Format the Partition

```
format fs=ntfs quick
```

**Explanation:**
Formats the selected partition with the **NTFS** file system using a quick format.

- `fs=ntfs` – Specifies NTFS as the file system
- `quick` – Performs a fast format without scanning for bad sectors

---

### 8. Exit DiskPart

```
exit
```

**Explanation:**
Closes the DiskPart utility and returns you to the Command Prompt.

---

## Result

After completing these steps:

- The disk is fully cleaned
- A new primary partition is created
- The partition is formatted with NTFS
- The disk is ready for use (a drive letter may need to be assigned in Disk Management)

---

## Optional Next Step: Assign a Drive Letter

If the disk does not appear in File Explorer:

1. Open **Disk Management** (`diskmgmt.msc`)
2. Right-click the new volume
3. Choose **Change Drive Letter and Paths**
4. Assign an available drive letter

---

## Complete Command Set (Copy-Paste Ready)

```text
diskpart
list disk
select disk 2
clean
create part pri
select part 1
format fs=ntfs quick
exit
```

---

## Summary

This DiskPart workflow is commonly used to:

- Reinitialize USB flash drives
- Recover corrupted removable media
- Prepare disks for fresh use
- Remove hidden or invalid partitions

Use DiskPart carefully, and always confirm the correct disk number before running destructive commands like `clean`.

