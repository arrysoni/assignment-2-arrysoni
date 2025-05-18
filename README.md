[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/KO-MdZgM)

# Assignment-2: JBOD Linear Device Integration

## 📚 CMPSC 311 – Fall 2024 Lab 2

This lab implements a **Linear Device** using a JBOD (Just a Bunch of Disks) architecture. The assignment simulates disk-level memory management and integrates disk read operations across multiple disks as a unified block device.

---

## 🛠️ Functionality Overview

### 1. `int mdadm_mount(void)`
- **Purpose**: Mounts the JBOD device.
- **Return**:
  - `1` on successful mount
  - `-1` if already mounted
- **Notes**: Must be the first function called before read operations. All operations without a successful mount will fail.

### 2. `int mdadm_unmount(void)`
- **Purpose**: Unmounts the JBOD device.
- **Return**:
  - `1` on successful unmount
  - `-1` if not mounted
- **Notes**: No operations should be performed after unmount. Only one unmount allowed.

### 3. `int mdadm_read(uint32_t start_addr, uint32_t read_len, uint8_t *read_buf)`
- **Purpose**: Reads up to 1024 bytes from the linearized JBOD disk starting from a logical address.
- **Parameters**:
  - `start_addr`: Logical address (within 1MB).
  - `read_len`: Number of bytes to read (≤ 1024).
  - `read_buf`: Destination buffer.
- **Return Codes**:
  - `number of bytes read`: on success
  - `-1`: read out-of-bounds
  - `-2`: read length > 1024
  - `-3`: JBOD is unmounted
  - `-4`: other constraints violated

---

## 📂 Project Structure

| File        | Purpose                                                                 |
|-------------|-------------------------------------------------------------------------|
| `jbod.h`    | Constants & JBOD interface; **EDIT DISK AND BLOCK PARAMETERS HERE**     |
| `jbod.o`    | Precompiled JBOD driver object                                          |
| `mdadm.c`   | Main implementation file for all `mdadm_*` functions                    |
| `mdadm.h`   | Header file for `mdadm.c`                                               |
| `tester.c`  | Unit testing file to verify function correctness                        |
| `tester.h`  | Header for tester                                                       |
| `util.c`    | Utilities used by JBOD and tester                                       |
| `util.h`    | Utility header file                                                     |
| `Makefile`  | Compile and build instructions using `gcc-9`                            |

---

## 🧪 How to Build and Test

### 🧱 Compiler Setup
Use `gcc-9.5.0`. On lab machines:

```bash
/home/software/user_conf/bin/new_soft
vi ~/.software  # uncomment gcc-9.5.0 only
# Save and close file

## 
make clean
make
./tester
./tester -w traces/simple-input
