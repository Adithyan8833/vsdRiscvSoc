# vsdRiscvSoc

## Task 1 — Install base developer tools

This section lists the essential build prerequisites for the RISC-V simulator, proxy kernel, and related tools, including compilers, linkers, autotools, and necessary libraries. 

## Task 2 — Create a clean workspace and capture your home path 

Sets up a dedicated working directory (~/riscv_toolchain) from the home path to keep all toolchain files isolated and manageable. This ensures a clean environment that's easy to update, remove, or archive later.

   - ####  Go to the home directory
    cd
   - ####  Save current directory path to a variable
         pwd=$PWD
  - ####  Create the build directory if it doesn't exist
         mkdir -p riscv_toolchain
  - #### Enter the toolchain directory
         cd riscv_toolchain 

   ## Task 3 — Get a prebuilt RISC‑V GCC toolchain 

Downloads and extracts a prebuilt riscv64-unknown-elf-gcc toolchain with newlib support, used to compile bare-metal or user-space RISC-V programs. This avoids the need for building the toolchain from source, saving time and setup complexity.

- ####  Command to download files from the internet
       wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"

- #### Extract (x), Verbose (v), Gunzip (z), From file (f)
       tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
  
## Task 4 — Add toolchain to your PATH (current shell + persistent) 

  Add the RISC-V toolchain binaries to the system PATH for persistent access.

  ## Task 5 — Install Device Tree Compiler (DTC) 

  DTC is needed by many RISC‑V tools as a common SoC/simulator dependency.
   - #### Makes sure you're installing the latest available versions.
           sudo apt-get update
   - ####  Installs Device Tree Compiler (DTC) package.
        -  The -y flag automatically agrees to prompts.
  
               sudo apt-get install -y device-tree-compiler
  - ####  To Verify Installation
        dtc --version
       - Expected output
          ```
             Version: DTC 1.6.1
          ```
   ## Task 6 — Build and install Spike (RISC‑V ISA simulator)
  Spike is the official RISC-V ISA simulator used to run and verify compiled ELF binaries, and installing it in the same prefix keeps all tools organized together.

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

  ## Task 7 — Build and install the RISC‑V Proxy Kernel (riscv-pk)
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








    
