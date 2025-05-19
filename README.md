# VSFSck: A Consistency Checker for Very Simple File System (VSFS)

## Overview

This project implements `vsfsck`, a consistency checker tool for a custom-designed virtual file system image `vsfs.img`. It is developed as part of **CSE321 Project 2**.

The tool validates and repairs inconsistencies in key file system components:
- Superblock
- Inode and data bitmaps
- Inodes
- Data blocks

---

## Features

1. **Superblock Validator**
   - Verifies magic number (`0xd34d`)
   - Confirms block size (`4096`) and total blocks (`64`)
   - Checks validity of block pointers
   - Ensures inode size is 256 bytes and inode count is valid

2. **Data Bitmap Consistency Checker**
   - Ensures all used data blocks are referenced by valid inodes
   - Verifies all inode-referenced data blocks are marked as used

3. **Inode Bitmap Consistency Checker**
   - Checks that each set bit corresponds to a valid inode
   - Valid inodes must have `link_count > 0` and `deletion_time == 0`

4. **Duplicate Block Checker**
   - Detects blocks referenced by more than one inode

5. **Bad Block Checker**
   - Identifies blocks with indices outside the valid range (8–63)

---

## File System Layout

- **Block Size**: 4096 Bytes  
- **Total Blocks**: 64  
- **Layout**:
  - Block 0: Superblock
  - Block 1: Inode bitmap
  - Block 2: Data bitmap
  - Blocks 3–7: Inode table
  - Blocks 8–63: Data blocks  
- **Inode Size**: 256 Bytes

---

## How to Build and Run

### Prerequisites

- GCC compiler
- `ghex` (for inspecting the binary image file)

Install `ghex` if not already available:

```bash
sudo apt install ghex 

# Compile the Code
gcc -o vsfsck superblock.c inode_bitmap.c data_blocks.c main.c

# Run the Checker
./vsfsck vsfs.img

# Optional: View the Image File
ghex vsfs.img

# Recompile (if code is modified)
gcc -o vsfsck superblock.c inode_bitmap.c data_blocks.c main.c

# Re-check the image
./vsfsck vsfs.img
