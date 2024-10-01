This guide explains how to set up the i386-elf-gcc toolchain and QEMU on an ARM64 Ubuntu virtual machine (using UTM on M1/M2 Mac) to successfully build and run the mCertiKOS project. If you're encountering issues like missing toolchains or QEMU errors, follow the steps below.

Prerequisites
Make sure your Ubuntu VM is up-to-date by running:
Command:
sudo apt update

Step 1: Install the i386-elf Toolchain
To set up the i386-elf-gcc cross-compiler:

Command:
sudo apt install gcc-i386-elf

If you encounter issues with missing packages, you can build binutils and gcc manually.

Step 2: Build and Install binutils for i386-elf
Download and extract binutils:

Command:
wget https://ftp.gnu.org/gnu/binutils/binutils-2.40.tar.gz
tar -xvf binutils-2.40.tar.gz
cd binutils-2.40

Build and install binutils:

Command:
mkdir build-binutils
cd build-binutils
../configure --target=i386-elf --prefix=/usr/local/i386-elf --disable-nls --disable-werror
make
sudo make install

Step 3: Build and Install gcc for i386-elf
Download and extract gcc:

Command:
cd ~
wget https://ftp.gnu.org/gnu/gcc/gcc-12.3.0/gcc-12.3.0.tar.gz
tar -xvf gcc-12.3.0.tar.gz
cd gcc-12.3.0
./contrib/download_prerequisites

Build and install gcc:

Command:
mkdir build-gcc
cd build-gcc
../configure --target=i386-elf --prefix=/usr/local/i386-elf --disable-nls --enable-languages=c --without-headers
make all-gcc
sudo make install-gcc

Step 4: Set the Path for i386-elf-gcc
Add the i386-elf-gcc binaries to your system's PATH:

Command:
export PATH=$PATH:/usr/local/i386-elf/bin

Make this change permanent by adding the line to your .bashrc or .zshrc:

Command:
echo 'export PATH=$PATH:/usr/local/i386-elf/bin' >> ~/.bashrc
source ~/.bashrc

Step 5: Build libgcc for the i386-elf Target
If you encounter errors related to libgcc.a not being found, follow these steps:

Command:
Build libgcc:
cd ~/gcc-12.3.0/build-gcc
make all-target-libgcc
sudo make install-target-libgcc

Verify that libgcc.a is installed:

Command:
ls /usr/local/i386-elf/lib/gcc/i386-elf/12.3.0/

You should see libgcc.a in this directory.

Step 6: Build the Project
Navigate to your project folder and use the following command to build:

Command:
cd ~/Desktop/lab1/mcertikos
make GCCPREFIX=i386-elf-

Step 7: Install and Run QEMU
If you encounter an error related to qemu-system-i386, install QEMU:

Command:
sudo apt install qemu qemu-system-i386

Verify QEMU installation:

Command:
qemu-system-i386 --version
