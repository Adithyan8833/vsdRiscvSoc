# Task 2 Prove Your Local RISCV Setup (Run, Disassemble,Decode)

![Platform](https://img.shields.io/badge/Platform-VirtualBox-orange)
![Toolchain](https://img.shields.io/badge/Toolchain-RISC--V-blue)
![Status](https://img.shields.io/badge/Status-✅%20Complete-brightgreen)
![Environment](https://img.shields.io/badge/Environment-Ubuntu%2022.04-yellow)



## 🎯 Objective
Set up and verify a local RISC V toolchain by compiling and running 3 to 4 C programs. Generate and analyze their assembly and disassembly. Manually decode integer instructions and provide proof of local execution using system-specific identifiers.
 
## 🚀 Task 2.1 —  Uniqueness mechanism
Before compiling any program, set the following environment variables in your Linux shell to embed user- and machine-specific identifiers into the binary.
- #### Commands
      export U=$(id -un)
      export H=$(hostname -s)
      export M=$(cat /etc/machine-id | head -c 16)
      export T=$(date -u +%Y-%m-%dT%H:%M:%SZ)
      export E=$(date +%s)
  - #### Verify
        echo $U
        echo $H
        echo $M
        echo $T
        echo $E
    <img width="552" height="274" alt="identity" src="https://github.com/user-attachments/assets/00a19c28-7034-452c-94d2-25bd5c129015" />
## 🚀 Task 2.2 — Common header(unique.h)
Create a reusable header for printing build/run metadata like user, host, machine ID, build time, etc
- #### code
```
 #ifndef UNIQUE_H
 #define UNIQUE_H
 #include <stdio.h>
 #include <stdint.h>
 #include <time.h>
 #ifndef USERNAME
 #define USERNAME "unknown_user"
 #endif
 #ifndef HOSTNAME
 #define HOSTNAME "unknown_host"
 #endif
 #ifndef MACHINE_ID
 #define MACHINE_ID "unknown_machine"
 #endif
 #ifndef BUILD_UTC
 #define BUILD_UTC "unknown_time"
 1
#endif
 #ifndef BUILD_EPOCH
 #define BUILD_EPOCH 0
 #endif
 static uint64_t fnv1a64(const char *s) {
 const uint64_t OFF = 1469598103934665603ULL, PRIME = 1099511628211ULL;
 uint64_t h = OFF;
 for (const unsigned char *p=(const unsigned char*)s; *p; ++p) {
 h ^= *p; h *= PRIME;
 }
 return h;
 }
 static void uniq_print_header(const char *program_name) {
 time_t now = time(NULL);
 char buf[512];
 int n = snprintf(buf, sizeof(buf), "%s|%s|%s|%s|%ld|%s|%s",
 USERNAME, HOSTNAME, MACHINE_ID, BUILD_UTC,
 (long)BUILD_EPOCH, __VERSION__, program_name);
 (void)n;
 uint64_t proof = fnv1a64(buf);
 char rbuf[600];
 snprintf(rbuf, sizeof(rbuf), "%s|run_epoch=%ld", buf, (long)now);
 uint64_t runid = fnv1a64(rbuf);
 printf("=== RISC-V Proof Header ===\n");
 printf("User
 : %s\n", USERNAME);
 printf("Host
 : %s\n", HOSTNAME);
 printf("MachineID : %s\n", MACHINE_ID);
 printf("BuildUTC : %s\n", BUILD_UTC);
 printf("BuildEpoch : %ld\n", (long)BUILD_EPOCH);
 printf("GCC
 : %s\n", __VERSION__);
 printf("PointerBits: %d\n", (int)(8*(int)sizeof(void*)));
 printf("Program
 : %s\n", program_name);
 printf("ProofID
 printf("RunID
 : 0x%016llx\n", (unsigned long long)proof);
 : 0x%016llx\n", (unsigned long long)runid);
 printf("===========================\n");
 }
 #endif
```
## 🚀 Task 2.3 —  Programs to implement factorial.c
#### facctorial code
     #include "unique.h"
     static unsigned long long fact(unsigned n){ return (n<2)?1ULL:n*fact(n-1); }
     int main(void){
     uniq_print_header("factorial");
     unsigned n = 12;
     printf("n=%u, n!=%llu\n", n, fact(n));
     return 0;
     }
####  Compile CODE
     riscv64-unknown-elf-gcc-O0-g-march=rv64imac-mabi=lp64 \
     -DUSERNAME="\"$U\""-DHOSTNAME="\"$H\""-DMACHINE_ID="\"$M\"" \
     -DBUILD_UTC="\"$T\""-DBUILD_EPOCH=$E \
     factorial.c-o factorial
#### Run
     spike pk ./factorial
#### Output of Spike
<img width="1213" height="365" alt="factorial" src="https://github.com/user-attachments/assets/1413d99b-abac-4178-b924-11fecb67cdf4" />

####  Assembly (.s)
    riscv64-unknown-elf-gcc-O0-S factorial.c-o factorial.s
####  Disassembly of main
     riscv64-unknown-elf-objdump-d ./factorial | sed-n '/<main>:/,/^$/p' | tee
     factorial_main_objdump.txt
#### Output
<img width="839" height="523" alt="factorial" src="https://github.com/user-attachments/assets/167166d3-e53a-43f3-86d3-f97baa59e488" />



## 🚀 Task 2.4 —  Programs to implement  max_array.c
#### max_array code
    #include "unique.h"
    int main(void){
    uniq_print_header("max_array");
    int a[] = {42,-7,19,88,3,88,5,-100,37};
    int n = sizeof(a)/sizeof(a[0]), max=a[0];
    for(int i=1;i<n;i++) if(a[i]>max) max=a[i];
    printf("Array length=%d, Max=%d\n", n, max);
    return 0;
    }
####  Compile CODE
     riscv64-unknown-elf-gcc-O0-g-march=rv64imac-mabi=lp64 \
     -DUSERNAME="\"$U\""-DHOSTNAME="\"$H\""-DMACHINE_ID="\"$M\"" \
     -DBUILD_UTC="\"$T\""-DBUILD_EPOCH=$E \
     max_array.c-o max_array
#### Run
     spike pk ./max_array
#### Output of Spike
<img width="1213" height="350" alt="max_array" src="https://github.com/user-attachments/assets/3da82d53-50f5-4996-bd2f-53a8deb0fbb1" />

####  Assembly (.s)
    riscv64-unknown-elf-gcc-O0-S max_array.c-o max_array.s
####  Disassembly of main
     riscv64-unknown-elf-objdump-d ./max_array | sed-n '/<main>:/,/^$/p' | tee
     max_array_main_objdump.txt
#### Output
<img width="839" height="620" alt="max_array" src="https://github.com/user-attachments/assets/0c8e154a-eb81-43f3-9a82-8e5695d94b73" />



## 🚀 Task 2.5 —  Programs to implement  bitops.c
#### bitops code
    #include "unique.h"
    int main(void){
    uniq_print_header("bitops");
    unsigned x=0xA5A5A5A5u, y=0x0F0F1234u;
    printf("x&y=0x%08X\n", x&y);
    printf("x|y=0x%08X\n", x|y);
    printf("x^y=0x%08X\n", x^y);
    printf("x<<3=0x%08X\n", x<<3);
    printf("y>>2=0x%08X\n", y>>2);
    return 0;
    }
####  Compile CODE
     riscv64-unknown-elf-gcc-O0-g-march=rv64imac-mabi=lp64 \
     -DUSERNAME="\"$U\""-DHOSTNAME="\"$H\""-DMACHINE_ID="\"$M\"" \
     -DBUILD_UTC="\"$T\""-DBUILD_EPOCH=$E \
     bitops.c-o bitops
#### Run
     spike pk ./bitops
#### Output of Spike
<img width="1213" height="385" alt="bitops" src="https://github.com/user-attachments/assets/53e425a8-bbf5-4e08-84d4-dbe16fffae98" />

####  Assembly (.s)
    riscv64-unknown-elf-gcc-O0-S bitops.c-o bitops.s
####  Disassembly of main
     riscv64-unknown-elf-objdump-d ./bitops | sed-n '/<main>:/,/^$/p' | tee
     bitops_main_objdump.txt
#### Output
<img width="1005" height="587" alt="bitops" src="https://github.com/user-attachments/assets/c73a694c-acca-4ccf-a0fb-3f85764e5a64" />



## 🚀 Task 2.6 —  Programs to implement  bubble_sort.c
#### bubble_sort
    #include "unique.h"
    void bubble(int *a,int n){ for(int i=0;i<n-1;i++) for(int j=0;j<n-1-i;j++) if(a[j]>a[j
    +1]){int t=a[j];a[j]=a[j+1];a[j+1]=t;} }
    int main(void){
    uniq_print_header("bubble_sort");
    int a[]={9,4,1,7,3,8,2,6,5}, n=sizeof(a)/sizeof(a[0]);
    bubble(a,n);
    printf("Sorted:"); for(int i=0;i<n;i++) printf(" %d",a[i]); puts("");
    return 0;
    }
####  Compile CODE
     riscv64-unknown-elf-gcc-O0-g-march=rv64imac-mabi=lp64 \
     -DUSERNAME="\"$U\""-DHOSTNAME="\"$H\""-DMACHINE_ID="\"$M\"" \
     -DBUILD_UTC="\"$T\""-DBUILD_EPOCH=$E \
     bubble_sort.c-o bubble_sort
#### Run
     spike pk ./bubble_sort
#### Output of Spike
<img width="1213" height="317" alt="bubble_sort" src="https://github.com/user-attachments/assets/d5a08db6-64c2-4401-85c5-81ef23121524" />

####  Assembly (.s)
    riscv64-unknown-elf-gcc-O0-S bubble_sort.c-o bubble_sort.s
####  Disassembly of main
     riscv64-unknown-elf-objdump-d ./bubble_sort | sed-n '/<main>:/,/^$/p' | tee
     bubble_sort_main_objdump.txt
#### Output
<img width="853" height="578" alt="bubble_sort" src="https://github.com/user-attachments/assets/cd2aea70-a08f-4978-af4e-4beea7bcb8e3" />

## 📌 Conclusion

This repository demonstrates the successful completion of **RISC-V Task 2**:
 
- Compilation and execution of multiple C programs on Spike with unique identity headers  
- Generation of assembly and `main`-function disassemblies  
- Manual decoding of RISC-V integer instructions  
- Structured organization of all source files, outputs, and documentation  

All outputs were verified for **correctness**, **uniqueness**, and **alignment with task requirements**.  
This work confirms a functional RISC-V workflow and a clear understanding of RISC-V assembly interpretation.

#### [📸 Screenshots](https://github.com/Adithyan8833/vsdRiscvSoc/tree/main/Screenshot)
### 📂 [Task 1](https://github.com/Adithyan8833/vsdRiscvSoc/tree/main/Task1)





     
 

 



