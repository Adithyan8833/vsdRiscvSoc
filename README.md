# vsdRiscvSoc

![Platform](https://img.shields.io/badge/Platform-VirtualBox-orange)
![Toolchain](https://img.shields.io/badge/Toolchain-RISC--V-blue)
![Status](https://img.shields.io/badge/Status-✅%20Complete-brightgreen)
![Environment](https://img.shields.io/badge/Environment-Ubuntu%2022.04-yellow)

#  Task 1

## 🎯 Objective

Successfully install essential base developer tools required for compiling the RISC-V simulator, proxy kernel, and associated components.  
This includes compilers, linkers, autotools, and waveform visualizer **GTKWave**.

## 📋 Prerequisites

- ✅ Oracle VirtualBox installed on Windows  
- ✅ Ubuntu 22.04 LTS (64-bit) set up in a Virtual Machine  
- ✅ Minimum 2 vCPUs, 4GB RAM, 30GB disk space  
- ✅ Internet connectivity inside VM  
- ✅ `sudo` access in Ubuntu 

## 🚀 Task 1.1 — Install base developer tools

This section lists the essential build prerequisites for the RISC-V simulator, proxy kernel, and related tools, including compilers, linkers, autotools, and necessary libraries. 

## 🚀 Task 1.2 — Create a clean workspace and capture your home path 

Sets up a dedicated working directory (~/riscv_toolchain) from the home path to keep all toolchain files isolated and manageable. This ensures a clean environment that's easy to update, remove, or archive later.

- ####  Go to the home directory
      cd
- ####  Save current directory path to a variable
      pwd=$PWD
- ####  Create the build directory if it doesn't exist
      mkdir -p riscv_toolchain
- #### Enter the toolchain directory
      cd riscv_toolchain 

## 🚀 Task 1.3 — Get a prebuilt RISC‑V GCC toolchain 

Downloads and extracts a prebuilt riscv64-unknown-elf-gcc toolchain with newlib support, used to compile bare-metal or user-space RISC-V programs. This avoids the need for building the toolchain from source, saving time and setup complexity.

- ####  Command to download files from the internet
      wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"

- #### Extract (x), Verbose (v), Gunzip (z), From file (f)
      tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
  
## 🚀 Task 1.4 — Add toolchain to your PATH (current shell + persistent) 

Add the RISC-V toolchain binaries to the system PATH for persistent access.

## 🚀 Task 1.5 — Install Device Tree Compiler (DTC) 

DTC is needed by many RISC‑V tools as a common SoC/simulator dependency.
- #### Makes sure you're installing the latest available versions.
      sudo apt-get update
- ####  Installs Device Tree Compiler (DTC) package.

  - The -y flag automatically agrees to prompts.
  
        sudo apt-get install -y device-tree-compiler

- ####  To Verify Installation
      dtc --version
  - Expected output
    ```
    Version: DTC 1.6.1
    ```
## 🚀 Task 1.6 — Build and install Spike (RISC‑V ISA simulator)
spike is the official RISC-V ISA simulator used to run and verify compiled ELF binaries, and installing it in the same prefix keeps all tools organized together.

- ####  Navigate to toolchain root
      cd $PWD/riscv_toolchain
- ####  Clone Spike source
      git clone https://github.com/riscv/riscv-isa-sim.git
- #### Enter Spike source directory
      cd riscv-isa-sim
- #### Create and enter build directory
      mkdir -p build && cd build
- #### Configure build with install prefix
      ../configure --prefix=$PWD/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14
- #### Compile Spike
      make -j$(nproc)
- #### bInstall built binaries
      sudo make install

## 🚀 Task 1.7 — Build and install the RISC‑V Proxy Kernel (riscv-pk)
The proxy kernel (pk) is a minimal runtime that lets Spike run your RISC-V programs like an OS.

- #### Clone and build the RISC-V Proxy Kernel (riscv-pk)
      cd $pwd/riscv_toolchain
      git clone https://github.com/riscv/riscv-pk.git
      cd riscv-pk
      mkdir -p build && cd build
- #### Configure the build for riscv64-unknown-elf target
      ../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf 
- #### Build and install
      make -j$(nproc) 
      sudo make install
## 🚀 Task 1.8 — Ensure the cross bin directory is in PATH
The proxy kernel (pk) is a minimal runtime that lets Spike run your RISC-V programs like an OS

- #### Command (current shell):
      export PATH=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0
      2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH
- #### Command (persistent):
      echo 'export PATH=$HOME/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0
      2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH' >> 
      ~/.bashrc 
      source ~/.bashrc\

## 🚀 Task 1.9 — (Optional) Install Icarus Verilog
 For digital design education, you’ll often verify RTL with Icarus Verilog and view 
waveforms with GTKWaves. This is not required for the C uniqueness test but is part of a 
complete RISC‑V learning toolchain.

## 🚀 Task 1.10 — Quick sanity checks 
Confirms the toolchain and simulator are visible and runnable from your shell.
- #### command
      which riscv64-unknown-elf-gcc 
      riscv64-unknown-elf-gcc -v 
      which spike 
      spike --version || spike -h 
      which pk
- #### Output
      /home/adi231/riscv_toolchain/.../bin/riscv64-unknown-elf-gcc
      /home/adi231/riscv_toolchain/.../bin/spike
      /home/adi231/riscv_toolchain/.../bin/pk
  
## 🚀 Final Deliverable: A Unique C Test (Username & Machine Dependent)

#### code
    #include <stdint.h>
    #include <stdio.h>

    #define USERNAME "adi231"
    #define HOSTNAME "ubuntu-risk"

    static uint64_t fnv1a64(const char *s) {
    const uint64_t FNV_OFFSET = 1469598103934665603ULL;
    const uint64_t FNV_PRIME  = 1099511628211ULL;
    uint64_t h = FNV_OFFSET;
    for (const unsigned char *p = (const unsigned char*)s; *p; ++p) {
            h ^= (uint64_t)(*p);
            h *= FNV_PRIME;
            }
     return h;
      }

      nt main(void) {
      const char *user = USERNAME;
      const char *host = HOSTNAME;

      char buf[256];
      int n = snprintf(buf, sizeof(buf), "%s@%s", user, host);
      if (n <= 0) return 1;

      uint64_t uid = fnv1a64(buf);

      printf("RISC-V Uniqueness Check\n");
      printf("User: %s\n", user);
      printf("Host: %s\n", host);
      printf("UniqueID: 0x%016llx\n", (unsigned long long)uid);

      #ifdef __VERSION__
      unsigned long long vlen = (unsigned long long)sizeof(__VERSION__) - 1;
      printf("GCC_VLEN: %llu\n", vlen);
      #endif

      return 0;
      }

- #### OUTPUT
      RISC-V Uniqueness Check
      User: adi231
      Host: ubuntu-risk
      UniqueID: 0x94d1814471449918
      GCC_VLEN: 5

## SCREENSHOT 
#### INPUT
<img width="1218" height="684" alt="Screenshot from 2025-08-03 16-09-29" src="https://github.com/user-attachments/assets/fe54ba6d-68c1-4243-9341-6d72bfddaaca" />






 






#### OUTPUT
<img width="1218" height="183" alt="Screenshot from 2025-08-03 16-08-33" src="https://github.com/user-attachments/assets/7bb0ba74-c873-46dd-b243-39e1824c653d" />








    
