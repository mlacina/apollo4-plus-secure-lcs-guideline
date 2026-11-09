# Apollo4 Plus — Transition to Secure LCS Guideline

**Author:** Mariusz Łacina  
**Document version:** 1.6  
**Date:** November 6, 2026

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. Python Environment Setup](#2-python-environment-setup)
  - [2.1. Install System Prerequisites](#21-install-system-prerequisites)
  - [2.2. Install and Configure pyenv](#22-install-and-configure-pyenv)
  - [2.3. Install Python 3.10.12](#23-install-python-31012)
  - [2.4. Create the Python Virtual Environment](#24-create-the-python-virtual-environment)
  - [2.5. Install Python Dependencies](#25-install-python-dependencies)
  - [2.6. Verify and Reuse the Environment](#26-verify-and-reuse-the-environment)
- [3. OpenSSL 1.0.2u Installation](#3-openssl-102u-installation)
  - [3.1. Prepare Installation Directories](#31-prepare-installation-directories)
  - [3.2. Download OpenSSL 1.0.2u](#32-download-openssl-102u)
  - [3.3. Configure OpenSSL](#33-configure-openssl)
  - [3.4. Compile and Install OpenSSL](#34-compile-and-install-openssl)
  - [3.5. Verify the OpenSSL Installation](#35-verify-the-openssl-installation)
  - [3.6. Configure the OpenSSL Environment](#36-configure-the-openssl-environment)
- [4. SEGGER J-Link Installation](#4-segger-j-link-installation)
  - [4.1. Download SEGGER J-Link](#41-download-segger-j-link)
  - [4.2. Install SEGGER J-Link](#42-install-segger-j-link)
  - [4.3. Verify the Installation](#43-verify-the-installation)
- [5. AmbiqSuite SDK R4.5.0 Installation](#5-ambiqsuite-sdk-r450-installation)
  - [5.1. Download AmbiqSuite SDK](#51-download-ambiqsuite-sdk)
  - [5.2. Extract AmbiqSuite SDK](#52-extract-ambiqsuite-sdk)
  - [5.3. Verify the SDK Installation](#53-verify-the-sdk-installation)
  - [5.4. Create a Provisioning Working Directory](#54-create-a-provisioning-working-directory)
  - [5.5. Verify the Provisioning Working Directory](#55-verify-the-provisioning-working-directory)
- [6. OEM Security Asset Preparation](#6-oem-security-asset-preparation)
  - [6.1. Generate OEM Cryptographic Keys](#61-generate-oem-cryptographic-keys)
  - [6.2. Generate the OEM Root of Trust (HBK1)](#62-generate-the-oem-root-of-trust-hbk1)
  - [6.3. Generate the OEM Key Request](#63-generate-the-oem-key-request)
  - [6.4. Generate the OEM Security Configuration Asset](#64-generate-the-oem-security-configuration-asset)
  - [6.5. Generate Encrypted OEM Asset Packages](#65-generate-encrypted-oem-asset-packages)
  - [6.6. Generate the OEM Provisioning Data Blob](#66-generate-the-oem-provisioning-data-blob)
  - [6.7. Provisioning Assets Summary](#67-provisioning-assets-summary)
- [7. OEM Certificate Chain Generation](#7-oem-certificate-chain-generation)
  - [7.1. Prepare the Main Firmware Image](#71-prepare-the-main-firmware-image)
  - [7.2. Generate the OEM Root Certificate](#72-generate-the-oem-root-certificate)
  - [7.3. Generate the OEM Key Certificate](#73-generate-the-oem-key-certificate)
  - [7.4. Configure the OEM Content Certificate](#74-configure-the-oem-content-certificate)
  - [7.5. Generate the OEM Content Certificate](#75-generate-the-oem-content-certificate)
  - [7.6. Certificate Chain Memory Allocation](#76-certificate-chain-memory-allocation)
  - [7.7. Verify Generated Certificate Files](#77-verify-generated-certificate-files)
  - [7.8. Certificate Generation Summary](#78-certificate-generation-summary)
- [8. Secure OTA Firmware Package Generation](#8-secure-ota-firmware-package-generation)
  - [8.1. Create the OTA Working Directory](#81-create-the-ota-working-directory)
  - [8.2. Create the Key Configuration File](#82-create-the-key-configuration-file)
  - [8.3. Create the Secure Firmware OTA Configuration](#83-create-the-secure-firmware-ota-configuration)
  - [8.4. Generate the Secure OTA Package](#84-generate-the-secure-ota-package)
  - [8.5. Verify the Generated Package](#85-verify-the-generated-package)
  - [8.6. Secure OTA Package Placement](#86-secure-ota-package-placement)
  - [8.7. Secure OTA Package Summary](#87-secure-ota-package-summary)
- [9. INFO0 and Secure Boot Metadata Generation](#9-info0-and-secure-boot-metadata-generation)
  - [9.1. Generate the INFO0 Configuration Image](#91-generate-the-info0-configuration-image)
  - [9.2. Generate the Certificate Chain Pointer Table](#92-generate-the-certificate-chain-pointer-table)
  - [9.3. Generate the OTA Descriptor](#93-generate-the-ota-descriptor)
- [10. OEM Provisioning and Transition to Secure LCS](#10-oem-provisioning-and-transition-to-secure-lcs)
  - [10.1. Single-Pass J-Link Provisioning Script](#101-single-pass-j-link-provisioning-script)
  - [10.2. Execute OEM Provisioning](#102-execute-oem-provisioning)
  - [10.3. Verify Secure LCS and Secure Boot Configuration Using UART](#103-verify-secure-lcs-and-secure-boot-configuration-using-uart)
    - [10.3.1.  Connect the UART Interface](#1031-connect-the-uart-interface)
    - [10.3.2.  Prepare the UART Communication](#1032-prepare-the-uart-communication)
    - [10.3.3.  Enter Boot Override Mode](#1033-enter-boot-override-mode)
    - [10.3.4.  Execute HELLO and STATUS Commands](#1034-execute-hello-and-status-commands)
    - [10.3.5.  Verify the Reported Security Configuration](#1035-verify-the-reported-security-configuration)
    - [10.3.6.  Retrieve and Record the Device SOC-ID](#1036-retrieve-and-record-the-device-soc-id)
- [11. Secure Debug Certificate Generation and Installation](#11-secure-debug-certificate-generation-and-installation)
  - [11.1. Secure Debug Certificate Structure](#111-secure-debug-certificate-structure)
  - [11.2. Prepare the Certificate Generation Environment](#112-prepare-the-certificate-generation-environment)
  - [11.3. Generate the OEM Debug-Key Certificate](#113-generate-the-oem-debug-key-certificate)
  - [11.4. Generate the OEM Debug Enabler Certificate](#114-generate-the-oem-debug-enabler-certificate)
  - [11.5. Generate the Device-Specific OEM Debug Developer Certificate](#115-generate-the-device-specific-oem-debug-developer-certificate)
  - [11.6. Secure Debug Certificate Installation Through UART](#116-secure-debug-certificate-installation-through-uart)
    - [11.6.1.  Create the Secure Debug Wired Download Configuration](#1161-create-the-secure-debug-wired-download-configuration)
    - [11.6.2.  Generate the Secure Debug Wired Package](#1162-generate-the-secure-debug-wired-package)
    - [11.6.3.  Install the Certificate Using Boot Override UART](#1163-install-the-certificate-using-boot-override-uart)
    - [11.6.4.  Activate the Secure Debug Certificate](#1164-activate-the-secure-debug-certificate)
    - [11.6.5.  Verify Debug Access](#1165-verify-debug-access)
    - [11.6.6.  Removing the Secure Debug Certificate Using J-Link](#1166-removing-the-secure-debug-certificate-using-j-link)



## Disclaimer and Responsibility

This document is an independent technical guide prepared to assist engineers in implementing secure provisioning and secure boot functionality on Ambiq Apollo4 Plus devices.

The author has made every reasonable effort to ensure the accuracy of the information and to provide a practical, functional solution based on available documentation, engineering experience, and laboratory testing. Nevertheless, the procedures and examples are provided "as is", without any warranty, express or implied, regarding their correctness, completeness, suitability, or reliability.

The user is solely responsible for evaluating, validating, and ensuring the correct operation, security, safety, and regulatory compliance of their own product. This includes verifying the suitability of the described procedures, protecting cryptographic assets, and assessing the consequences of irreversible security configuration and provisioning operations.

The author assumes no responsibility for device failures, loss of data, security vulnerabilities, or other consequences arising from the use or implementation of the information provided in this document.

Users are strongly encouraged to consult the latest official Ambiq documentation and perform independent validation before applying these procedures to production devices.



## 1. Introduction

This document describes the process of configuring Secure Boot and transitioning an Apollo4 Plus device from Device Manufacturer (DM) Life Cycle State to Secure Enabled (SE) Life Cycle State.

The procedure uses security provisioning utilities provided with AmbiqSuite SDK R4.5.0. The original utilities are located in the ./tools/apollo4b_scripts directory. For the purposes of this guide, a separate working copy named ./tools/apollo4b_scripts_prov is used to prepare OEM cryptographic assets, certificates, provisioning packages, and secure firmware updates.

Secure Enabled Life Cycle State (SE LCS) imposes significantly stricter security restrictions than the default development environment. Once the device enters this state, standard debugging interfaces, including SWD/J- Link access, are disabled by default.

Debug access may be restored selectively through an authenticated Secure Debug Certificate, provided that the necessary security configuration and an appropriate certificate installation mechanism are available. Secure Debug Certificates can also be installed after the device has entered SE LCS, using a supported update interface and subject to the configured security policies.

Secure Boot additionally enforces authentication of executable firmware. If image authentication fails or the required security assets are invalid, the device may refuse to execute the application. Such behavior is intentional and is designed to prevent unauthorized firmware from running.

Before transitioning a device to SE LCS, it is strongly recommended to establish and verify a reliable firmware recovery mechanism.

The Ambiq Secure Bootloader provides a wired firmware update interface that can be configured for this purpose. Unlike an application-based update mechanism, the Secure Bootloader's wired update functionality does not depend on successful execution of the customer application. This distinction is particularly important when an invalid or corrupted firmware image prevents the application from starting.

The same wired interface may also be used to install an authenticated, device-specific Secure Debug Certificate, allowing selected debugging capabilities to be enabled according to the applicable security policy.

An application-level OTA mechanism may download and stage a secure firmware update package containing a new application image and its corresponding OEM Content Certificate. Following a device reset, the Ambiq Secure Bootloader authenticates the update and, if valid, installs the new firmware and the associated Content Certificate.

The application cannot bypass Secure Boot authentication or install arbitrary executable firmware. Furthermore, an application-based update mechanism cannot provide recovery if the current application fails to boot. For this reason, an independent Secure Bootloader wired update interface is strongly recommended.

Additional debugging capabilities may be controlled through the Debug Control Unit (DCU) and the corresponding HAL functions, provided that the relevant permissions are not restricted by the device's security configuration.

The security configuration, firmware recovery strategy, and Secure Debug access policy should therefore be carefully considered and validated before the device is transitioned to SE LCS.



### MRAM Memory Organization

Table 1 presents the 2 MB MRAM memory map adopted for the Apollo4 Plus secure provisioning procedure described in this guide.

The first 96 KB of MRAM is reserved for the Ambiq Secure Bootloader (SBL). The remaining memory is divided into separate regions for the main application, OTA firmware update storage, and security-related certificates.

The application and OTA image storage regions are allocated 928 KB each, providing space to stage a new firmware image without overwriting the currently installed application.

A dedicated 16 KB block is reserved for the OTA Descriptor, which provides the information required by the Secure Bootloader to locate and process the staged update package. The complete OTA package, including its metadata and cryptographic overhead, must fit within the allocated staging region.

The uppermost 80 KB of MRAM is reserved for security-related data. This area contains the Certificate Chain Pointer Array, OEM Root Certificate, OEM Key Certificate, OEM Content Certificate, and a reserved location for the Secure Debug Certificate.

Each of these five regions occupies a separate 16 KB memory block, corresponding to the MRAM protection granularity supported by Apollo4 Plus. This arrangement allows individual blocks to be protected independently, according to the selected security policy, without unnecessarily restricting access to adjacent certificate regions.

The memory addresses shown in Table 1 are used consistently throughout this guide. Any modifications to the proposed layout require corresponding updates to the INFO0 configuration, certificate pointers, OTA descriptor, firmware image configuration, and provisioning scripts.

|  | START | STOP | SIZE |
| --- | --- | --- | --- |
| Secure Bootloader | 0x00000000 | 0x00017FFF | 96 KB |
| Application | 0x00018000 | 0x000FFFFF | 928 KB |
| OTA Descriptor | 0x00100000 | 0x00103FFF | 16 KB |
| OTA Image | 0x00104000 | 0x001EBFFF | 928 KB |
| Certificate Chain Pointers | 0x001EC000 | 0x001EFFFF | 16 KB |
| OEM Root Certificate | 0x001F0000 | 0x001F3FFF | 16 KB |
| OEM Key Certificate | 0x001F4000 | 0x001F7FFF | 16 KB |
| OEM Content Certificate | 0x001F8000 | 0x001FBFFF | 16 KB |
| Secure Debug Certificate | 0x001FC000 | 0x001FFFFF | 16 KB |

Table 1. MRAM memory map used as the base for this guideline. Users can modify it according to their needs. Any changes in below memory addresses require review and modification of configuration files used in this guide.



## 2. Python Environment Setup

The Apollo4 Plus secure provisioning utilities are executed using Python on Ubuntu Linux. To provide a consistent execution environment and avoid conflicts with the system Python installation, this guide uses Python 3.10.12 managed by pyenv and a dedicated Python virtual environment.

The following procedure assumes Ubuntu 24.04 LTS and the Bash shell.



### 2.1. Install System Prerequisites

Update the Ubuntu package index:

```bash
sudo apt update
```

Install the packages required to build Python and run the provisioning utilities:

```bash
sudo apt install -y build-essential git curl wget ca-certificates make perl pkg-config zlib1g-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev libffi-dev liblzma-dev libncurses-dev libxml2-dev libxmlsec1-dev libzstd-dev tk-dev xz-utils unzip zip udev
```



### 2.2. Install and Configure pyenv

The pyenv utility allows multiple Python versions to coexist independently of the operating system's default Python installation.

Install pyenv:

```bash
git clone https://github.com/pyenv/pyenv.git "$HOME/.pyenv"
```

Open the Bash configuration file:

```bash
nano ~/.bashrc
```

Add the following three lines at the end of the file:

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
```

Save the file by pressing Ctrl+O, followed by Enter, and exit the editor using Ctrl+X. These settings make pyenv available automatically whenever a new Bash terminal is opened.

Reload the Bash configuration in the current terminal:

```bash
source ~/.bashrc
```

Execute:

```bash
pyenv --version
```

The command should display the installed pyenv version.

The environment is now ready for installing Python 3.10.12, as described in the next section.



### 2.3. Install Python 3.10.12

Install the required Python version:

```bash
pyenv install 3.10.12
```

Verify the installation:

```bash
"$HOME/.pyenv/versions/3.10.12/bin/python3" --version
```

Expected result:

```bash
Python 3.10.12
```

This installation is separate from the Python interpreter provided by Ubuntu.



### 2.4. Create the Python Virtual Environment

Create a dedicated virtual environment named apollo4-env:

```bash
"$HOME/.pyenv/versions/3.10.12/bin/python3" -m venv "$HOME/apollo4-env"
```

Activate the environment:

```bash
source "$HOME/apollo4-env/bin/activate"
```

After activation, the terminal prompt should include the environment name:

```bash
(apollo4-env) user@ubuntu:~$
```

To leave the virtual environment, execute:

```bash
deactivate
```

Verify that the active interpreter is Python 3.10.12:

```bash
python --version
```

The expected result:

```bash
Python 3.10.12
```



### 2.5. Install Python Dependencies

Upgrade the Python package management utilities:

```bash
python -m pip install --upgrade pip setuptools wheel
```

Install the Python packages required by the Apollo4 Plus provisioning tools:

```bash
python -m pip install pyserial==3.5 pycryptodome==3.23.0
```

Check that the installed package dependencies are consistent:

```bash
python -m pip check
```



### 2.6. Verify and Reuse the Environment

Verify that the Python interpreter and required cryptographic and serial communication modules are available:

```bash
python -c "import sys, serial; from Crypto.Cipher import AES; print(sys.version); print('Apollo4 Python environment: OK')"
```

Expected confirmation:

```bash
Apollo4 Python environment: OK
```

The virtual environment remains installed after closing the terminal. It does not need to be recreated for subsequent provisioning sessions.

Activate the environment before the next steps:

```bash
source "$HOME/apollo4-env/bin/activate"
```



## 3. OpenSSL 1.0.2u Installation

The OEM provisioning utilities included in AmbiqSuite SDK R4.5.0 require the legacy OpenSSL 1.0.2 library for certain cryptographic operations.

Ubuntu 24.04 provides a newer version of OpenSSL, which does not include the legacy libcrypto.so.1.0.0 shared library required by these utilities.

To ensure compatibility, OpenSSL 1.0.2u is installed separately in the user's home directory, without modifying or replacing the system OpenSSL installation.

The following procedure downloads, compiles, and installs OpenSSL 1.0.2u from source.



### 3.1. Prepare Installation Directories

Create a directory for the OpenSSL source files and a separate directory for the compiled library:

```bash
mkdir -p "$HOME/ambiq-legacy/src" "$HOME/ambiq-legacy/openssl-install"
```

The following directories will be used:

| Directory | Purpose |
| --- | --- |
| ~/ambiq-legacy/src | Downloaded archive and extracted source files |
| ~/ambiq-legacy/openssl-install | Compiled OpenSSL binaries and shared libraries |



### 3.2. Download OpenSSL 1.0.2u

Change to the source directory:

```bash
cd "$HOME/ambiq-legacy/src"
```

Download the OpenSSL 1.0.2u source archive:

```bash
curl -fL -o openssl-1.0.2u.tar.gz https://www.openssl.org/source/old/1.0.2/openssl-1.0.2u.tar.gz
```

Extract the downloaded archive:

```bash
tar -xzf openssl-1.0.2u.tar.gz
```

The source files will be extracted into the openssl-1.0.2u directory.



### 3.3. Configure OpenSSL

Change to the extracted source directory:

```bash
cd "$HOME/ambiq-legacy/src/openssl-1.0.2u"
```

Configure the build:

```bash
./config --prefix="$HOME/ambiq-legacy/openssl-install" --openssldir="$HOME/ambiq-legacy/openssl-install" shared no-ssl2 no-ssl3
```

The configuration parameters have the following purposes:

| Parameter | Description |
| --- | --- |
| --prefix | Specifies the installation directory |
| --openssldir | Specifies the OpenSSL configuration directory |
| shared | Enables building shared libraries, including libcrypto.so.1.0.0 |
| no-ssl2 | Disables the obsolete SSLv2 protocol |
| no-ssl3 | Disables the obsolete SSLv3 protocol |

The installation remains independent of the OpenSSL version provided by Ubuntu.



### 3.4. Compile and Install OpenSSL

Generate the required build dependencies:

```bash
make depend
```

Compile OpenSSL using the available processor cores:

```bash
make -j"$(nproc)"
```

The compilation process may display warnings associated with this legacy OpenSSL version.

After compilation, install the compiled binaries and libraries:

```bash
make install_sw
```

The installation does not require sudo, because all files are installed within the user's home directory.



### 3.5. Verify the OpenSSL Installation

Check the installed version:

```bash
"$HOME/ambiq-legacy/openssl-install/bin/openssl" version
```

Expected result:

```bash
OpenSSL 1.0.2u  20 Dec 2019
```

This confirms that the required legacy OpenSSL executable has been installed successfully.



### 3.6. Configure the OpenSSL Environment

The Ambiq OEM provisioning utilities require access to the legacy OpenSSL executable and its shared libraries.

Add the OpenSSL installation directory to the current terminal's executable search path:

```bash
export PATH="$HOME/ambiq-legacy/openssl-install/bin:$PATH"
```

Configure the shared library search path:

```bash
export LD_LIBRARY_PATH="$HOME/ambiq-legacy/openssl-install/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}"
```

The first command makes the legacy OpenSSL executable available through PATH.

The second command allows applications and Python modules that depend on the legacy OpenSSL shared libraries to locate them during execution.

In particular, it provides access to libcrypto.so.1.0.0, which is required by the OEM certificate generation utilities.

These settings apply only to the current terminal session. They do not permanently modify the Ubuntu system configuration.

The same environment variables must be configured when opening a new terminal session for OEM provisioning.

Before executing the Ambiq provisioning utilities, activate the Python virtual environment:

```bash
source "$HOME/apollo4-env/bin/activate"
```



## 4. SEGGER J-Link Installation

SEGGER J-Link is used to communicate with Apollo4 Plus devices through the Serial Wire Debug (SWD) interface.

This guide uses SEGGER J-Link Software and Documentation Pack V9.92.0, installed on Ubuntu 24.04 LTS.

The J-Link Commander utility (JLinkExe) is used later in the secure provisioning procedure to execute the programming script.



### 4.1. Download SEGGER J-Link

Open the official SEGGER download page:

https://www.segger.com/downloads/jlink/

Locate the J-Link Software and Documentation Pack and select version V9.92.0 or newer for Linux.

Download the 64-bit DEB Installer for x86-64 Ubuntu.

The installation package should be saved in the user's Downloads directory:

```bash
~/Downloads/JLink_Linux_V992_x86_64.deb
```



### 4.2. Install SEGGER J-Link

Open an Ubuntu terminal and install the downloaded package:

```bash
sudo apt install -y "$HOME/Downloads/JLink_Linux_V992_x86_64.deb"
```

Ubuntu installs the J-Link software and its required package dependencies.

The installation includes J-Link Commander (JLinkExe), which will be used to execute the provisioning commands.



### 4.3. Verify the Installation

Verify that the J-Link Commander executable is available:

```bash
command -v JLinkExe
```

The command should display the path to the installed executable.

Check the installed J-Link package and version:

```bash
dpkg-query -W -f='${Package} ${Version}\n' jlink
```

Confirm that the installed package corresponds to J-Link V9.92.0.

The SEGGER J-Link software is now installed and ready for use with the Apollo4 Plus provisioning procedure.



## 5. AmbiqSuite SDK R4.5.0 Installation

AmbiqSuite SDK provides the software libraries, examples, and utilities required to develop applications and configure secure provisioning for Apollo4 Plus devices.

This guide uses AmbiqSuite SDK R4.5.0.

The SDK includes Python utilities for generating OEM cryptographic assets, certificate chains, provisioning packages, and secure firmware update images.

A separate working copy of the provisioning tools will be created to keep the original SDK files unchanged.



### 5.1. Download AmbiqSuite SDK

Obtain the AmbiqSuite SDK R4.5.0 distribution from Ambiq:

https://contentportal.ambiq.com/documents/20123/387817/AmbiqSuite-R4-5-0.zip

Download the SDK archive and save it in the Ubuntu Downloads directory:

```bash
~/Downloads/AmbiqSuite_R4.5.0.zip
```

The following procedure assumes that the unzip utility has already been installed as part of the Ubuntu system prerequisites.



### 5.2. Extract AmbiqSuite SDK

Open an Ubuntu terminal.

Extract the SDK archive into the user's home directory:

```bash
unzip -q "$HOME/Downloads/AmbiqSuite_R4.5.0.zip" -d "$HOME"
```

The SDK root directory must be located at:

```bash
~/AmbiqSuite_R4.5.0
```

If the archive uses a different top-level directory name, rename the extracted SDK directory accordingly, keeping its internal structure unchanged.



### 5.3. Verify the SDK Installation

Verify that the Apollo4 Plus provisioning tools are available:

```bash
ls -ld "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts"
```

The command should display the directory containing the Apollo4 Plus security provisioning utilities.



### 5.4. Create a Provisioning Working Directory

To preserve the original SDK files, create a separate working copy of the provisioning scripts:

```bash
cp -a "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts" "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

The resulting directories are:

| Directory | Purpose |
| --- | --- |
| tools/apollo4b_scripts | Original SDK provisioning utilities |
| tools/apollo4b_scripts_prov | Working copy used throughout this guide |

The working copy will contain the configuration files, Python scripts, and generated binary files used during OEM provisioning.



### 5.5. Verify the Provisioning Working Directory

Change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Verify that the main provisioning utilities and directories are present:

```bash
ls -d oem_tools_pkg create_info0.py create_cust_image_blob.py
```

The command should display all entries without errors.

All subsequent provisioning commands in this guide use

```bash
~/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov
```

as the default working directory, unless another directory is explicitly specified.

The AmbiqSuite SDK installation and provisioning workspace preparation are now complete.



## 6. OEM Security Asset Preparation

OEM provisioning is the process of preparing the cryptographic keys, Root of Trust, security configuration, and encrypted provisioning data required to transition an Apollo4 Plus device from Device Manufacturer (DM) LCS to Secure Enabled (SE) LCS.

AmbiqSuite SDK R4.5.0 provides Python utilities for generating the required security assets and combining them into a provisioning data blob.

This procedure uses the ICV-encrypted provisioning method, in which OEM assets are encrypted using key material associated with a response package supplied by Ambiq.

The Python environment, OpenSSL 1.0.2u libraries, and provisioning working directory have already been prepared in the preceding chapters.



> [!NOTE]
> **OEM Key Selection**
>
> The AmbiqSuite SDK includes pre-generated OEM keys that can be used for development and testing.
>
> - **Using SDK-provided keys:** Skip the key-generation procedure and continue with Section 6.4, *Generate the OEM Security Configuration Asset*.
>
> - **Using newly generated OEM keys:** Obtain a new, matching `icv_response_pkg.bin` from Ambiq before proceeding with encrypted OEM provisioning. An ICV response package generated for a different key set cannot be reused.




> [!CAUTION]
> **Production Security**
>
> The pre-generated OEM keys provided with the AmbiqSuite SDK are intended for development and testing only.
>
> **These keys must not be used in production devices** because they are distributed with the SDK and cannot be considered secret.
>
> For production, generate your own OEM keys and obtain the corresponding ICV response package from Ambiq.




### 6.1. Generate OEM Cryptographic Keys

Apollo4 Plus uses asymmetric RSA keys for certificate-based authentication and symmetric AES keys for cryptographic operations and OEM asset provisioning.

AmbiqSuite provides the OEM Key Generation Utility for generating these assets.

The utility uses the following configuration file:

```bash
./oem_tools_pkg/am_oem_key_gen_util/oemKeyGenConfig.cfg
```

The configuration defines the key generation parameters and associated output files.

To generate a new OEM key set, change to the key generation directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/am_oem_key_gen_util"
```

Execute:

```bash
python3 am_oem_keys_gen_util.py ./oemKeyGenConfig.cfg
```

The utility generates the asymmetric and symmetric keys required for OEM provisioning, OEM certificate generation, and Secure Debug certificate generation.

The generated key files are stored in the corresponding RSA and AES output directories.

The OTP key-bank contents must be prepared separately.



### 6.2. Generate the OEM Root of Trust (HBK1)

The Hash of Boot Key 1 (HBK1) establishes the OEM Root of Trust used to authenticate the OEM certificate chain.

HBK1 is derived from the OEM Root Certificate public key using the truncated SHA-256 hash format supported by Apollo4 Plus.

Change to the HBK generation directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/cert_utils/am_hbk_gen"
```

Execute:

```bash
python3 am_hbk_gen_util.py -key ../../am_oem_key_gen_util/oemRSAKeys/oemRootCertPublicKey.pem -endian L -hash_format SHA256_TRUNC
```

The generated files are stored in:

```bash
./oem_tools_pkg/cert_utils/am_hbk_gen/hbk_gen_util_outPut
```

The binary file containing HBK1 is:

```bash
hbkPubKeyHash_bin.bin
```

This file is subsequently referenced by the OEM provisioning data generator.

The same OEM Root key must be used when generating the OEM Root Certificate.



### 6.3. Generate the OEM Key Request

The ICV-encrypted provisioning method requires an ICV response package generated by Ambiq.

The OEM Key Request Utility creates a request containing the public key information required by Ambiq to prepare the corresponding encrypted response.

The utility uses the configuration file:

```bash
./oem_tools_pkg/oem_asset_prov_utils/oem_key_request/am_config/am_dmpu_oem_key_request.cfg
```

Change to the OEM Key Request directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/oem_asset_prov_utils/oem_key_request"
```

Execute:

```bash
python3 am_oem_key_request_util.py ./am_config/am_dmpu_oem_key_request.cfg
```

The generated request package is:

```bash
oem_request_pkg.bin
```

Submit this file to Ambiq through the designated provisioning process.

Ambiq processes the request and returns:

```bash
./oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/inputData/icv_response_pkg.bin
```

The request and response form a matched pair. The ICV response must correspond to the OEM key material used for provisioning.



### 6.4. Generate the OEM Security Configuration Asset

The OEM Asset Generation Utility generates a binary file containing the security configuration that will be programmed into the device during OEM provisioning.

The configuration file is:

```bash
./oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/am_config/oem_asset_gen.cfg
```

It defines several important device security settings, including:

- MRAM write and copy protection.

- Secure Boot and firmware update policies.

- Wired update interface configuration.

- Boot override configuration.

- Debug access restrictions.

- Customer OTP access permissions.

The MRAM protection configuration should be consistent with the memory map presented in Table 1.

Apollo4 Plus supports MRAM protection with 16 KB granularity. The security-related regions at the end of MRAM have therefore been allocated separate 16 KB blocks to allow independent protection.

For applications using a Customer Secondary Bootloader (CSBL), the PLONEXIT configuration must also be considered. Its value determines the protection-lock behavior during the handoff from the Ambiq Secure Bootloader.

Review the OEM security configuration before generating the asset.

Change to the OEM Asset Package directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/oem_asset_prov_utils/oem_asset_package"
```

Execute:

```bash
python3 oem_asset_gen_util.py ./am_config/oem_asset_gen.cfg
```

The generated file is:

```bash
oem_asset_test_data.bin
```

This file contains the OEM security configuration and is used as an input to the encrypted asset packaging process.



### 6.5. Generate Encrypted OEM Asset Packages

The OEM Asset Packaging Utility encrypts the security assets required for OEM provisioning.

The ICV-encrypted provisioning procedure uses the corresponding Ambiq response package and the OEM cryptographic assets prepared in the previous sections.

The configuration files specify the required input files, encryption parameters, and output filenames.

Three encrypted asset packages are generated.

**Step 1 — Encrypt the OEM Security Asset**

Configuration file:

```bash
./am_config/oem_asset_enc.cfg
```

Execute:

```bash
python3 am_dmpu_oem_asset_pkg_util.py ./am_config/oem_asset_enc.cfg
```

Output:

```bash
oem_asset_pkg.bin
```

**Step 2 — Encrypt the KCE Asset**

Configuration file:

```bash
./am_config/am_asset_oem_ce.cfg
```

Execute:

```bash
python3 am_dmpu_oem_asset_pkg_util.py ./am_config/am_asset_oem_ce.cfg
```

Output:

```bash
oem_prov_asset_kce_pkg.bin
```

**Step 3 — Encrypt the KCP Asset**

Configuration file:

```bash
./am_config/am_asset_oem_cp.cfg
```

Execute:

```bash
python3 am_dmpu_oem_asset_pkg_util.py ./am_config/am_asset_oem_cp.cfg
```

Output:

```bash
oem_prov_asset_kcp_pkg.bin
```

All three commands are executed from the same OEM Asset Package directory.

The resulting encrypted files are used by the OEM Provisioning Data Generation Utility.



### 6.6. Generate the OEM Provisioning Data Blob

The OEM Provisioning Data Generation Utility combines the encrypted OEM security assets into a single provisioning package.

The resulting package contains the information required by the Ambiq OEM Provisioning Tool (OPT) to initialize the device's security configuration.

The utility uses the following configuration file:

```bash
./am_config/am_dmpu_data_gen.cfg
```

The configuration specifies:

- The OEM Root of Trust (HBK1).

- The encrypted KCP and KCE assets.

- The encrypted OEM security configuration.

- Initial software version parameters.

- Debug Control Unit (DCU) settings.

- Additional provisioning configuration fields.

Review the configuration and confirm that the referenced files correspond to the OEM key set and security configuration selected for the device.

Remain in the OEM Asset Package directory.

Execute:

```bash
python3 am_dmpu_prov_data_gen_util.py ./am_config/am_dmpu_data_gen.cfg
```

The generated output file is:

```bash
dmpu_prov_data_blob.bin
```

This file contains the final OEM provisioning data.

During device provisioning, the blob will be loaded into SRAM and processed by the Ambiq OEM Provisioning Tool.



### 6.7. Provisioning Assets Summary

The following files are generated or prepared during OEM security asset provisioning:


| File | Description |
|------|-------------|
| `hbkPubKeyHash_bin.bin` | OEM Root of Trust (HBK1) |
| `oem_request_pkg.bin` | OEM key request submitted to Ambiq |
| `icv_response_pkg.bin` | Corresponding response received from Ambiq |
| `oem_asset_test_data.bin` | OEM security configuration |
| `oem_asset_pkg.bin` | Encrypted OEM security asset |
| `oem_prov_asset_kce_pkg.bin` | Encrypted KCE asset |
| `oem_prov_asset_kcp_pkg.bin` | Encrypted KCP asset |
| `dmpu_prov_data_blob.bin` | Final OEM provisioning data |


The final provisioning data blob is used together with the Ambiq-signed OEM Provisioning Tool:

```bash
./oem_tools_pkg/oem_prov_tool/opt_image_pkg.bin
```

During device provisioning, the OPT image and the provisioning data blob are loaded into SRAM at addresses 0x10030000 and 0x10037000, respectively.

The next chapter describes the generation of the OEM certificate chain required to authenticate firmware during Secure Boot.



## 7. OEM Certificate Chain Generation

The Apollo4 Plus Secure Boot mechanism authenticates customer firmware using a chain of three OEM certificates:

1. OEM Root Certificate — establishes trust in the OEM certificate hierarchy through the Root of Trust (HBK1).

2. OEM Key Certificate — authenticates the public key used by the OEM Content Certificate.

3. OEM Content Certificate — authenticates the application firmware image and defines its associated

security attributes.

The certificate chain must be generated using the same OEM RSA keys selected for provisioning in Chapter 6.



### 7.1. Prepare the Main Firmware Image

The OEM Content Certificate contains information about the application firmware, including its location and size. Therefore, the firmware image must be available before the Content Certificate is generated.

For this procedure, the application firmware is named:

```bash
main_firmware_image.bin
```

Place the compiled application binary in the following directory:

```bash
./oem_tools_pkg/cert_utils/cert_gen_utils/inputData/
```

The application must be linked to execute from MRAM at:

```bash
0x00018000
```

According to the memory map defined in Table 1, the application region extends from 0x00018000 to 0x000FFFFF, providing 928 KB of available space.

Verify that the application binary exists and does not exceed the allocated region.

The firmware image is stored unencrypted in its execution region in MRAM. Encryption of a transport or update package, when required, is handled separately during Secure OTA package generation.




> [!CAUTION]
> **Important:** Whenever the application firmware changes, a corresponding new OEM Content Certificate must be generated.



### 7.2. Generate the OEM Root Certificate

The OEM Root Certificate establishes the first certificate in the OEM authentication chain.

It is associated with the OEM Root of Trust (HBK1) generated in Chapter 6.

The certificate generation utility uses the configuration file:

```bash
./am_config/am_oem_root_cert_hbk1.cfg
```

Change to the certificate generation directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/cert_utils/cert_gen_utils"
```

Generate the OEM Root Certificate:

```bash
python3 am_cert_key_util.py ./am_config/am_oem_root_cert_hbk1.cfg
```

The generated files are:

| File | Description |
| --- | --- |
| am_cert_key_util_output/oem_root_cert_hbk1.bin | Binary OEM Root Certificate |
| am_cert_key_util_output/oem_root_cert_hbk1_Cert.txt | Text representation of the certificate |

The binary certificate will subsequently be installed in the designated MRAM certificate region.



### 7.3. Generate the OEM Key Certificate

The OEM Key Certificate authenticates the public key used for signing the OEM Content Certificate.

It is generated using the same OEM RSA key hierarchy and the following configuration file:

```bash
./am_config/am_oem_key_cert.cfg
```

Remain in the certificate generation directory.

Execute:

```bash
python3 am_cert_key_util.py ./am_config/am_oem_key_cert.cfg
```

The generated files are:

| File | Description |
| --- | --- |
| am_cert_key_util_output/oem_key_cert.bin | Binary OEM Key Certificate |
| am_cert_key_util_output/oem_key_cert_Cert.txt | Text representation of the certificate |

The OEM Key Certificate forms the second level of the authentication chain.



### 7.4. Configure the OEM Content Certificate

The OEM Content Certificate authenticates the application image and specifies its location, maximum size, and protection attributes.

The generator uses the following configuration file:

```bash
./am_config/am_oem_cnt_cert.cfg
```

The application image information is provided through:

```bash
./inputData/images_table.tbl
```

Review the image table and configure the entry corresponding to main_firmware_image.bin.

The following parameters must be consistent with the selected application image and MRAM memory map:

| Parameter | Value / Requirement |
| --- | --- |
| ImageName | ./inputData/main_firmware_image.bin |
| RAMloadAdd | 0x00018000 |
| flashStoreAdd | 0xFFFFFFFF |
| Maxsize | Not greater than 0x000E8000 and not smaller than the application image |
| Enc-Scheme | 0 — plaintext image in MRAM |
| WriteProtect | According to the selected firmware protection policy |
| CopyProtect | According to the selected firmware protection policy |
| ExtendedFormat | 0 — unsupported extended format disabled |

For this procedure, the application is executed directly from its installed MRAM location.

If image write protection is enabled, the Secure Bootloader applies protection at 16 KB granularity.

If copy protection is enabled, the application must be built accordingly. In particular, literal data in protected executable regions may cause execution failures.

The configuration should preserve the selected firmware update strategy, including the ability to install authenticated firmware updates.



### 7.5. Generate the OEM Content Certificate

After preparing the application binary and configuring the image table, generate the OEM Content Certificate.

Remain in the certificate generation directory.

Execute:

```bash
python3 am_cert_content_util.py ./am_config/am_oem_cnt_cert.cfg
```

The generated files are:

| File | Description |
| --- | --- |
| am_cert_content_util_output/content_cert.bin | Binary OEM Content Certificate |
| am_cert_content_util_output/content_cert_Cert.txt | Text representation of the certificate |

The generated Content Certificate is associated with the application image specified in images_table.tbl.

The binary Content Certificate will also be included in the Secure OTA package prepared in a subsequent chapter.



### 7.6. Certificate Chain Memory Allocation

The three certificates will be installed in dedicated MRAM regions, according to the memory map defined in Table 1.

Each certificate occupies an independently allocated 16 KB MRAM protection region.

| Certificate | Start Address | End Address |
| --- | --- | --- |
| OEM Root Certificate | 0x001F0000 | 0x001F3FFF |
| OEM Key Certificate | 0x001F4000 | 0x001F7FFF |
| OEM Content Certificate | 0x001F8000 | 0x001FBFFF |

The certificate pointer table is located at:

```bash
0x001EC000
```

It contains three consecutive 32-bit addresses, stored in little-endian format:

| Offset | Certificate | Address |
| --- | --- | --- |
| 0x00 | OEM Root Certificate | 0x001F0000 |
| 0x04 | OEM Key Certificate | 0x001F4000 |
| 0x08 | OEM Content Certificate | 0x001F8000 |

The certificate chain pointer in INFO0 will reference this table.

The remaining MRAM region starting at 0x001FC000 is reserved for the Secure Debug Certificate.

The certificate pointer table, INFO0 image, and MRAM installation procedure will be prepared in the subsequent provisioning chapters.



### 7.7. Verify Generated Certificate Files

Verify that all three binary certificate files have been generated:

```bash
ls -l ./am_cert_key_util_output/oem_root_cert_hbk1.bin ./am_cert_key_util_output/oem_key_cert.bin ./am_cert_content_util_output/content_cert.bin
```

Each certificate must fit within its allocated 16 KB MRAM region.

Check the certificate file sizes:

```bash
wc -c ./am_cert_key_util_output/oem_root_cert_hbk1.bin ./am_cert_key_util_output/oem_key_cert.bin ./am_cert_content_util_output/content_cert.bin
```

Each generated certificate must be nonempty and no larger than 16384 bytes.



### 7.8. Certificate Generation Summary

The following binary files are required for Secure Boot provisioning:

| File | Target MRAM Address |
| --- | --- |
| oem_root_cert_hbk1.bin | 0x001F0000 |
| oem_key_cert.bin | 0x001F4000 |
| content_cert.bin | 0x001F8000 |

The three certificates establish the OEM authentication chain required by the Apollo4 Plus Secure Bootloader.

Their corresponding addresses will be stored in the certificate pointer table, and the table location will be provided through INFO0.

The next chapter describes the preparation of the Secure OTA firmware package.



## 8. Secure OTA Firmware Package Generation

The Apollo4 Plus Secure Bootloader (SBL) supports authenticated and encrypted firmware update packages.

For this procedure, the firmware update package will contain the application image and its corresponding OEM Content Certificate generated in Chapter 7.

The package is prepared using the AmbiqSuite SDK R4.5.0 create_cust_image_blob.py utility.

The resulting binary will be staged in MRAM and subsequently processed by the Secure Bootloader.



### 8.1. Create the OTA Working Directory

Change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Create the amota directory:

```bash
mkdir -p ./amota
```

The directory will contain the following files:

| File | Description |
| --- | --- |
| keys.ini | RSA and AES key configuration |
| firmware_ota.ini | Secure firmware package configuration |
| main_firmware_ota.bin | Generated Secure OTA package |



### 8.2. Create the Key Configuration File

The keys.ini file specifies the cryptographic keys used for firmware authentication and encryption.

The RSA keys must correspond to the OEM certificate chain generated in Chapter 7.

The AES keys must correspond to the keys included in the OEM provisioning data prepared in Chapter 6.

Create the configuration file:

```bash
nano ./amota/keys.ini
```

Insert the following content:

```ini
[Root Key]
index = 0x0
format = pem
filename = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/oemRootCertKeyPair.pem
passfile = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/pwdOemRootCertKey_Rsa.txt

[Key Cert Key]
index = 0x1
format = pem
filename = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/oemKeyCertKeyPair.pem
passfile = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/pwdOemKeyCertKey_Rsa.txt

[Content Cert Key]
index = 0x2
format = pem
filename = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/oemContentCertKeyPair.pem
passfile = oem_tools_pkg/am_oem_key_gen_util/oemRSAKeys/pwdOemContentCertKey_Rsa.txt

[Symmetric Keys]
kcp = oem_tools_pkg/am_oem_key_gen_util/oemAESKeys/kcp.bin
kce = oem_tools_pkg/am_oem_key_gen_util/oemAESKeys/kce.bin
kb0 = oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/inputData/keyBank0.bin
kb1 = oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/inputData/keyBank1.bin
kb2 = oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/inputData/keyBank2.bin
kb3 = oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/inputData/keyBank3.bin
```

Save the file.

The key configuration uses the OEM key set restored in Chapter 6.



### 8.3. Create the Secure Firmware OTA Configuration

The firmware_ota.ini file defines the firmware image, destination address, cryptographic algorithms, Content Certificate, and generated output file.

Create the configuration file:

```bash
nano ./amota/firmware_ota.ini
```

Insert the following content:

```ini
[Settings]
ambiq_owned = 0x0
chip = apollo4b

app_file = ./oem_tools_pkg/cert_utils/cert_gen_utils/inputData/main_firmware_image.bin
load_address = 0x00018000

enc_algo = 0x1
auth_algo = 0x1
auth_key = 0x2
kek_index = 0x80

image_type = secure-firmware
ota = 0x1
sbl_ota = 0x0
offset = 0x0

certificate = ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_content_util_output/content_cert.bin

output = ./amota/main_firmware_ota.bin
key_table = ./amota/keys.ini
loglevel = 0x2
```

Save the file.

The configuration parameters are described below.


| Parameter | Value | Description |
|-----------|-------|-------------|
| `chip` | `apollo4b` | Apollo4 Blue/Plus tool configuration |
| `app_file` | `main_firmware_image.bin` | Application firmware |
| `load_address` | `0x00018000` | Application destination address |
| `enc_algo` | `0x1` | AES-128 CTR encryption |
| `auth_algo` | `0x1` | RSA authentication |
| `auth_key` | `0x2` | OEM Content Certificate signing key |
| `kek_index` | `0x80` | First OTP key-bank AES key |
| `image_type` | `secure-firmware` | Authenticated firmware image |
| `ota` | `0x1` | Firmware update package |
| `sbl_ota` | `0x0` | Customer firmware update |
| `certificate` | `content_cert.bin` | Corresponding OEM Content Certificate |


The load_address parameter specifies where the application will be installed after successful authentication and decryption.

It does not specify the temporary OTA staging address.

The OEM Content Certificate must correspond to the application binary included in this package.



### 8.4. Generate the Secure OTA Package

Remain in the provisioning working directory:

```bash
~/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov
```

Execute:

```bash
python3 create_cust_image_blob.py -c ./amota/firmware_ota.ini
```

The utility processes the application firmware and its Content Certificate using the selected RSA and AES keys.

The generated output file is:

```bash
./amota/main_firmware_ota.bin
```

This file is the Secure OTA image that will be staged in MRAM.

The package includes the metadata and cryptographic information required by the Secure Bootloader to authenticate and process the firmware update.



### 8.5. Verify the Generated Package

Verify that the generated file exists:

```bash
ls -l ./amota/main_firmware_ota.bin
```

Display its size:

```bash
stat -c '%n: %s bytes' ./amota/main_firmware_ota.bin
```

The generated package must fit entirely within the allocated OTA staging region.

According to Table 1, the staging area is:

| Parameter | Address / Size |
| --- | --- |
| OTA staging start | 0x00104000 |
| OTA staging end | 0x001EBFFF |
| Available capacity | 0x000E8000 (928 KiB) |

Check the package size against this limit:

```bash
test "$(stat -c %s ./amota/main_firmware_ota.bin)" -le $((0xE8000)) && echo "OTA package size: PASS" || echo "OTA package size: FAIL"
```

The Secure OTA package contains additional metadata, authentication information, and the OEM Content Certificate. Therefore, the maximum application image size that fits in the OTA staging area is smaller than the full 928 KiB application region.



### 8.6. Secure OTA Package Placement

The generated Secure OTA package will be programmed into MRAM at:

```bash
0x00104000
```

The OTA descriptor will be stored separately at:

```bash
0x00100000
```

The destination address for the authenticated application is:

```bash
0x00018000
```

These addresses serve different purposes:

| Address | Purpose |
| --- | --- |
| 0x00018000 | Installed application firmware |
| 0x00100000 | OTA descriptor |
| 0x00104000 | Temporary Secure OTA package |

During the provisioning sequence, the Secure OTA package will be staged in MRAM before the device transitions to Secure Enabled LCS.

Following provisioning and reset, the Secure Bootloader will process the OTA descriptor, authenticate and decrypt the update package, and install the application firmware together with its corresponding Content Certificate.

The installed application will execute from its designated MRAM region.



### 8.7. Secure OTA Package Summary

The resulting file:

```bash
./amota/main_firmware_ota.bin
```

contains the encrypted and authenticated firmware update package prepared for the Apollo4 Plus Secure Bootloader.

The generated package uses the OEM cryptographic key hierarchy and Content Certificate prepared in the preceding chapters.

The below function can be used to verify the certificate chain and OTA inputs. The function performs host-side cryptographic verification of the OEM certificate chain, firmware integrity, and Secure OTA configuration. It also checks the generated OTA package size. It does not access or modify the target device, nor does it decrypt or authenticate the generated OTA package itself.

Copy the entire code block below and paste it into the Ubuntu terminal using Ctrl+Shift+V. Press Enter to execute the function.

```bash
apollo4_verify_crypto() (
    cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov" || return 1

    python3 - <<'PY'
import hashlib
import struct
import sys
from pathlib import Path
from configparser import ConfigParser
from Crypto.PublicKey import RSA
from Crypto.Hash import SHA256
from Crypto.Signature import pss

BASE = Path.cwd()
CERT = BASE / "oem_tools_pkg/cert_utils/cert_gen_utils"
ASSET = BASE / "oem_tools_pkg/oem_asset_prov_utils/oem_asset_package"

def check(condition, message):
    if not condition:
        raise ValueError(message)
    print("[PASS]", message)

def load(path):
    check(path.is_file() and path.stat().st_size > 0,
          f"File available: {path.name}")
    return path.read_bytes()

def ini(path):
    cfg = ConfigParser(interpolation=None)
    check(bool(cfg.read(path)), f"Configuration: {path.name}")
    return cfg

def swap32(data):
    return b"".join(data[i:i+4][::-1]
                    for i in range(0, len(data), 4))

def digest_matches(value, data):
    digest = hashlib.sha256(data).digest()
    versions = (digest, digest[::-1],
                swap32(digest), swap32(digest)[::-1])
    return any(value == v for v in versions)

def load_key(cfg, section):
    pem = BASE / cfg.get(section, "filename")
    pwd = BASE / cfg.get(section, "passfile")
    password = load(pwd).strip()
    key = RSA.import_key(load(pem), passphrase=password)
    check(key.has_private(), f"{section}: private key")
    return key

def read_cert(path, magic, private_key, trailer=0):
    data = load(path)
    check(len(data) >= 16 + 404 + 384 + trailer,
          f"{path.name}: certificate length")

    header = struct.unpack_from("<4I", data)
    check(header[0] == magic, f"{path.name}: certificate type")

    signed_size = header[2] * 4
    check(signed_size + 384 + trailer == len(data),
          f"{path.name}: signed size")

    modulus = data[16:400]
    expected = private_key.n.to_bytes(384, "big")
    check(modulus in (expected, swap32(expected),
                      expected[::-1], swap32(expected)[::-1]),
          f"{path.name}: RSA modulus matches OEM key")

    signature = data[signed_size:signed_size+384]
    digest = SHA256.new(data[:signed_size])
    verified = False

    for candidate in (signature, signature[::-1],
                      swap32(signature), swap32(signature)[::-1]):
        try:
            pss.new(private_key.publickey(), salt_bytes=32).verify(
                digest, candidate)
            verified = True
            break
        except (ValueError, TypeError):
            pass

    check(verified, f"{path.name}: RSA-PSS/SHA-256 signature")
    return data

try:
    print("=== Apollo4 Plus Cryptographic Verification ===")

    keys = ini(BASE / "amota/keys.ini")
    fwcfg = ini(BASE / "amota/firmware_ota.ini")

    root_key = load_key(keys, "Root Key")
    key_key = load_key(keys, "Key Cert Key")
    content_key = load_key(keys, "Content Cert Key")

    root = read_cert(
        CERT / "am_cert_key_util_output/oem_root_cert_hbk1.bin",
        0x53426B63, root_key)

    key = read_cert(
        CERT / "am_cert_key_util_output/oem_key_cert.bin",
        0x53426B63, key_key)

    content = read_cert(
        CERT / "am_cert_content_util_output/content_cert.bin",
        0x53426363, content_key, trailer=8)

    check(digest_matches(root[424:456], key[16:420]),
          "Root -> Key public-key hash")

    check(digest_matches(key[424:456], content[16:420]),
          "Key -> Content public-key hash")

    versions = [
        struct.unpack_from("<I", c, 420)[0]
        for c in (root, key, content)
    ]
    check(len(set(versions)) == 1,
          "Certificate software versions agree")

    hbk = load(
        BASE / "oem_tools_pkg/cert_utils/am_hbk_gen/"
               "hbk_gen_util_outPut/hbkPubKeyHash_bin.bin")

    root_hashes = [
        hashlib.sha256(root[16:420]).digest(),
        hashlib.sha256(root[16:400]).digest()
    ]
    hbk_ok = any(
        hbk in (
            d[:len(hbk)], d[-len(hbk):],
            d[:len(hbk)][::-1], d[-len(hbk):][::-1]
        )
        for d in root_hashes
    )
    if hbk_ok:
        print("[PASS] HBK1 matches OEM Root public-key hash")
    else:
        print("[WARN] HBK1 truncation/format not independently confirmed")

    image_table = CERT / "inputData/images_table.tbl"
    entries = [
        line.split() for line in image_table.read_text().splitlines()
        if line.strip() and not line.lstrip().startswith("#")
    ]
    check(len(entries) == 1 and len(entries[0]) == 8,
          "Single-image certificate table")

    entry = entries[0]
    firmware_path = (CERT / entry[0]).resolve()
    firmware = load(firmware_path)

    check(int(entry[1], 0) == 0x18000,
          "Image table load address")
    check(int(entry[2], 0) == 0xFFFFFFFF,
          "Image table flash storage address")
    check(int(entry[3], 0) == 0xE8000,
          "Image table maximum size")
    check(all(int(x, 0) == 0 for x in entry[4:]),
          "Image table security flags")
    check(len(firmware) <= 0xE8000,
          "Application fits MRAM region")

    image_hash = content[432:464]
    image_addr, image_max, image_flags = struct.unpack_from(
        "<III", content, 464)

    check(digest_matches(image_hash, firmware),
          "Firmware SHA-256 matches Content Certificate")
    check(image_addr == 0x18000,
          "Content Certificate image address")
    check(image_max == 0xE8000,
          "Content Certificate maximum size")
    check(image_flags == 0,
          "Content Certificate image flags")

    signed_size = struct.unpack_from("<I", content, 8)[0] * 4
    actual_size = struct.unpack_from(
        "<I", content, signed_size + 384 + 4)[0]
    check(actual_size == len(firmware),
          "Content Certificate actual image size")

    s = "Settings"
    app_file = (BASE / fwcfg.get(s, "app_file")).resolve()
    cert_file = (BASE / fwcfg.get(s, "certificate")).resolve()
    key_table = (BASE / fwcfg.get(s, "key_table")).resolve()

    check(app_file == firmware_path,
          "OTA references certified firmware")
    check(cert_file == (
        CERT / "am_cert_content_util_output/content_cert.bin").resolve(),
        "OTA references generated Content Certificate")
    check(key_table == (BASE / "amota/keys.ini").resolve(),
          "OTA references OEM key table")

    expected = {
        "chip": "apollo4b",
        "image_type": "secure-firmware"
    }
    for name, value in expected.items():
        check(fwcfg.get(s, name) == value,
              f"OTA {name}")

    numeric = {
        "load_address": 0x18000,
        "enc_algo": 1,
        "auth_algo": 1,
        "auth_key": 2,
        "kek_index": 0x80,
        "ota": 1,
        "sbl_ota": 0
    }
    for name, value in numeric.items():
        check(int(fwcfg.get(s, name), 0) == value,
              f"OTA {name}")

    for name in ("kcp", "kce", "kb0", "kb1", "kb2", "kb3"):
        path = BASE / keys.get("Symmetric Keys", name)
        data = load(path)
        check(len(data) >= 16,
              f"{name}: AES key material available")

    ota_path = BASE / fwcfg.get(s, "output")
    ota = load(ota_path)
    check(0 < len(ota) <= 0xE8000,
          "Secure OTA package fits staging MRAM")

    print()
    print("Certificate-chain and host-input checks: PASS")

except Exception as exc:
    print("[FAIL]", exc)
    sys.exit(1)
PY
)
apollo4_verify_crypto
```

The next chapter describes the generation of INFO0, the certificate pointer table, and the OTA descriptor required for device provisioning.



## 9. INFO0 and Secure Boot Metadata Generation

This chapter describes the generation of the INFO0 configuration image, OEM Certificate Chain Pointer Table, and OTA Descriptor required for Apollo4 Plus Secure Boot provisioning.

The memory addresses and binary metadata are based on the MRAM organization defined in Table 1.

All generated files will be used during the device provisioning procedure.



### 9.1. Generate the INFO0 Configuration Image

INFO0 contains the device configuration parameters used by the Ambiq Secure Bootloader, including:

- Secure Boot configuration and application start address.

- Wired update interface configuration.

- UART pin assignments and communication parameters.

- Boot override configuration.

- OEM Certificate Chain Pointer Table address.

- Secure Debug Certificate address.

- Wired update timeout.

The INFO0 image is generated using the AmbiqSuite SDK R4.5.0 utility:

```bash
create_info0.py
```

Remain in the provisioning working directory:

```bash
~/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov
```

Execute:

```bash
python3 ./create_info0.py --valid 1 --u0 0x1C200C0 --u1 0xFFFF3C2F --u2 0x4 --u3 0x4 --u4 0x0 --u5 0x0 --main 0x18000 --version 1 --wTO 5000 --sdcert 0x1FC000 --cchain 0x1EC000 info0
```

The generated binary file is:

```bash
./info0.bin
```

The following parameters establish the memory locations required by this procedure.

| Parameter | Value | Description |
| --- | --- | --- |
| valid | 1 | Marks the INFO0 configuration as valid. |
| u0 | 0x01C200C0 | UART configuration word 0. Specifies 115200 baud and the selected UART communication settings. |
| u1 | 0xFFFF3C2F | UART configuration word 1. Selects GPIO47 (RX) and GPIO60 (TX); unused hardware flow-control pins are set to 0xFF. |
| u2 | 0x4 | UART RX pin function selection (GPIO47, function 4). |
| u3 | 0x4 | UART TX pin function selection (GPIO60, function 4). |
| u4 | 0x0 | Additional UART configuration word 4, set to zero. |
| u5 | 0x0 | Additional UART configuration word 5, set to zero. |
| main | 0x00018000 | Main application start address in MRAM. |
| version | 1 | INFO0 configuration version. |
| wTO | 5000 | Wired update interface timeout in milliseconds (5 seconds). |
| sdcert | 0x001FC000 | Secure Debug Certificate location in MRAM. |
| cchain | 0x001EC000 | OEM Certificate Chain Pointer Table location in MRAM. |
| info0 | info0 | Output filename prefix. The utility generates info0.bin. |

The u0 through u5 parameters configure the wired bootloader interface and associated device settings.

Their values correspond to the selected UART and Boot Override configuration.

Verify that the INFO0 image has been generated:

```bash
ls -l ./info0.bin
```

Check the file size:

```bash
stat -c '%n: %s bytes' ./info0.bin
```

The expected INFO0 image size is 2048 bytes.



### 9.2. Generate the Certificate Chain Pointer Table

The OEM Certificate Chain Pointer Table contains the MRAM addresses of the three certificates used by the Secure Boot authentication mechanism.

The table is located at:

```bash
0x001EC000
```

It contains three consecutive 32-bit addresses in little-endian format.

| Offset | Certificate | Address |
| --- | --- | --- |
| 0x00 | OEM Root Certificate | 0x001F0000 |
| 0x04 | OEM Key Certificate | 0x001F4000 |
| 0x08 | OEM Content Certificate | 0x001F8000 |

The pointer table is generated as a binary file using Python.

Remain in the provisioning working directory.

Execute:

```bash
python3 -c "from pathlib import Path; import struct; Path('amota/cert_pointers.bin').write_bytes(struct.pack('<III',0x001F0000,0x001F4000,0x001F8000))"
```

The generated file is:

```bash
./amota/cert_pointers.bin
```

Verify the file size:

```bash
stat -c '%n: %s bytes' ./amota/cert_pointers.bin
```

The expected size is 12 bytes.

Display its contents:

```bash
xxd -g4 -e ./amota/cert_pointers.bin
```

The three addresses must correspond to the certificate locations defined in Table 1.

The INFO0 configuration generated in Section 9.1 references this table through the cchain parameter.



### 9.3. Generate the OTA Descriptor

The OTA Descriptor provides the information required by the Ambiq Secure Bootloader to locate and process the staged firmware update package.

According to the memory map defined in Table 1, the OTA Descriptor is stored at:

```bash
0x00100000
```

The Secure OTA package generated in Chapter 8 is stored separately at:

```bash
0x00104000
```

The descriptor consists of two 32-bit words stored in little-endian format.

| Offset | Value | Description |
| --- | --- | --- |
| 0x00 | 0x00104003 | OTA package address with SBL control bits |
| 0x04 | 0xFFFFFFFF | Second descriptor word |

Generate the descriptor binary.

Remain in the provisioning working directory.

Execute:

```bash
python3 -c "from pathlib import Path; import struct; Path('amota/ota_descriptor.bin').write_bytes(struct.pack('<II',0x00104003,0xFFFFFFFF))"
```

The generated file is:

```bash
./amota/ota_descriptor.bin
```

Verify its size:

```bash
stat -c '%n: %s bytes' ./amota/ota_descriptor.bin
```

The expected size is 8 bytes.

Display the binary contents:

```bash
xxd -g4 -e ./amota/ota_descriptor.bin
```

The descriptor must reference the Secure OTA package stored at 0x00104000.



## 10. OEM Provisioning and Transition to Secure LCS

This chapter describes the OEM provisioning procedure used to transition an Apollo4 Plus device from Device Manufacturer (DM) LCS to Secure Enabled (SE) LCS.

All required cryptographic assets, OEM certificates, INFO0 configuration, Secure OTA package, and binary metadata have been prepared in the preceding chapters.

The provisioning procedure uses a single SEGGER J-Link Commander script to install the security-related data, execute the Ambiq OEM Provisioning Tool (OPT), and initiate the transition to Secure Enabled LCS.



### 10.1. Single-Pass J-Link Provisioning Script

The provisioning sequence is based on the single-pass procedure successfully used during the Apollo4 Plus laboratory test, with MRAM addresses adapted to the memory organization defined in Table 1.

Change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Create the following script:

```bash
nano ./jlink-provision.txt
```

Insert the following content:

```text
//
// Apollo4 Plus - Secure LCS Provisioning
// Single-Pass Provisioning Procedure
//

usb
device AMAP42KP-KBR
si SWD
speed 4000
con
eoe 1
h

//
// STEP 1 - Load INFO0 image into SRAM
//

loadbin ./info0.bin 0x10001000 noreset
verifybin ./info0.bin 0x10001000

//
// STEP 2 - Configure Boot Override
//

w4 0x400C3850 0x00000112
w4 0x400C3854 0x00000001
mem32 0x400C3850 1
mem32 0x400C3854 1

//
// STEP 3 - Program INFO0 using Boot ROM
//

wreg MSP, 0x10000100
setPC 0x08000061
w4 0x10000000 0x00000000 0x00000200 0xD894E09E 0xFFFFFFFF
g
sleep 250
h
mem32 0x1000000C 1

//
// STEP 4 - Install OEM Certificate Chain
//

loadbin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_key_util_output/oem_root_cert_hbk1.bin 0x001F0000 noreset
loadbin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_key_util_output/oem_key_cert.bin 0x001F4000 noreset
loadbin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_content_util_output/content_cert.bin 0x001F8000 noreset
loadbin ./amota/cert_pointers.bin 0x001EC000 noreset

//
// STEP 5 - Stage Secure OTA Package and Descriptor
//

loadbin ./amota/main_firmware_ota.bin 0x00104000 noreset
loadbin ./amota/ota_descriptor.bin 0x00100000 noreset

//
// STEP 6 - Verify Programmed MRAM Contents
//

verifybin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_key_util_output/oem_root_cert_hbk1.bin 0x001F0000
verifybin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_key_util_output/oem_key_cert.bin 0x001F4000
verifybin ./oem_tools_pkg/cert_utils/cert_gen_utils/am_cert_content_util_output/content_cert.bin 0x001F8000
verifybin ./amota/cert_pointers.bin 0x001EqcC000
verifybin ./amota/main_firmware_ota.bin 0x00104000
verifybin ./amota/ota_descriptor.bin 0x00100000

//
// STEP 7 - Load OEM Provisioning Assets into SRAM
//

loadbin ./oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/dmpu_prov_data_blob.bin 0x10037000 noreset
loadbin ./oem_tools_pkg/oem_prov_tool/opt_image_pkg.bin 0x10030000 noreset

//
// STEP 8 - Verify SRAM Contents
//

verifybin ./oem_tools_pkg/oem_asset_prov_utils/oem_asset_package/dmpu_prov_data_blob.bin 0x10037000
verifybin ./oem_tools_pkg/oem_prov_tool/opt_image_pkg.bin 0x10030000

//
// STEP 9 - Activate OTA and Initiate Provisioning
//

w4 0x40020264 0x00100001
mem32 0x40020264 1

//
// STEP 10 - Enable Secure Boot and Secure Boot on Reset in DM LCS
//
w4 0x400C209C 0x20000200
mem32 0x400C209C 1

exec SetRestartOnClose = 0
w4 0x40000008 0x000000D4
sleep 1000
exit
```

#### Register Readback and Verification

The J-Link provisioning script uses mem32 commands to read back selected registers and operation status values after programming. These readbacks provide diagnostic information in the J-Link execution log but are not automatically compared against expected values.

The verifybin commands perform binary comparisons of the programmed memory contents, while eoe 1 terminates script execution if a J-Link command reports an error.

> [!NOTE]
> **Important:** The script does not automatically validate the values returned by mem32. In particular, the Secure Boot configuration stored in OTP_SECURITY must be reviewed to confirm that the required enable bits have been programmed.

For automated production provisioning, additional validation should be implemented to prevent the final lifecycle transition if a critical security configuration check fails.

#### Boot Override Configuration

Step 2 of the above script configures the Boot Override mechanism and wired update interface:

| Register | Value | Description |
| --- | --- | --- |
| 0x400C3850 | 0x00000112 | Configures Boot Override using GPIO18 (0x12), active LOW. |
| 0x400C3854 | 0x00000001 | Enables UART0 as the wired update interface. |

When GPIO18 is held LOW during reset, the Secure Bootloader enters its wired update mode instead of proceeding with the normal application boot sequence.

This mechanism provides access to the Secure Bootloader independently of the customer application. These settings are distinct from the UART pin assignments and communication parameters stored in INFO0.

#### INFO0 Programming

Step 3 invokes the device Boot ROM procedure to program the INFO0 configuration image previously loaded into SRAM.

The sequence initializes the stack pointer, selects the Boot ROM entry point, prepares the operation parameters, and executes the corresponding ROM routine.

The final mem32 command reads the operation status.

#### Certificate and Secure OTA Installation

Steps 4 and 5 install the OEM Certificate Chain, Certificate Chain Pointer Table, Secure OTA package, and OTA Descriptor into their designated MRAM regions.

All addresses correspond to the memory organization defined in Table 1.

Step 6 uses verifybin commands to compare the programmed memory contents against the generated binary files.

#### OEM Provisioning Tool

Steps 7 and 8 load and verify the encrypted DMPU provisioning data and Ambiq OEM Provisioning Tool in SRAM.

| Component | SRAM address |
| --- | --- |
| OEM Provisioning Tool | 0x10030000 |
| DMPU Provisioning Data Blob | 0x10037000 |

The OPT processes the encrypted provisioning data to establish the device security configuration and perform the required lifecycle transition.

#### OTA Activation and Reset

Step 9 configures the Secure Bootloader OTA pointer: 0x40020264

The value references the OTA Descriptor at 0x00100000, with the corresponding control bit enabled.

The final sequence:

```bash
exec SetRestartOnClose = 0
w4 0x40000008 0x000000D4
sleep 1000
exit
```

disables automatic restart on J-Link connection closure and initiates a software power-on reset.

This allows the device to continue the provisioning and Secure Boot sequence.



### 10.2. Execute OEM Provisioning

Connect the Apollo4 Plus device to SEGGER J-Link through the SWD interface.

The target device must initially operate in Device Manufacturer (DM) LCS.

Ensure that the target power supply remains stable throughout the provisioning operation.

Remain in the provisioning working directory.

Execute:

```bash
JLinkExe -CommandFile ./jlink-provision.txt
```

The script performs the provisioning sequence in a single J-Link Commander session.

The J-Link log displays the programming operations, binary verification results, Boot ROM status, and final reset sequence.

The eoe 1 setting stops script execution if a J-Link command reports an error.

After successful OEM provisioning, the device transitions to Secure Enabled LCS.

Following reset, the Secure Bootloader processes the staged Secure OTA package, authenticates and decrypts the firmware update, and installs the application with its associated OEM Content Certificate.

The application is installed at:

```bash
0x00018000
```

Once the device has entered Secure Enabled LCS, unrestricted SWD debugging is disabled according to the programmed security policy.



### 10.3. Verify Secure LCS and Secure Boot Configuration Using UART

After completing OEM provisioning, verify the device security configuration using the Secure Bootloader UART wired interface.

The Boot Override mechanism allows communication with the Secure Bootloader independently of the installed customer application.

The INFO0 configuration prepared in Chapter 9 specifies a wired update timeout of 5000 ms. The initial HELLO command must therefore be sent within this interval after the Secure Bootloader enters wired update mode.

> [!NOTE]
> **Note:** In Secure LCS, SWD and SWO access are disabled by default. Therefore, Secure Bootloader diagnostic output over SWO cannot be assumed to be available.

The UART Boot Override interface provides access to device status and security configuration independently of the customer application.

Verification of firmware authentication and execution requires an additional observation mechanism, such as application-level diagnostics or authorized debug access.



#### 10.3.1. Connect the UART Interface

Connect an external USB-to-UART adapter to the Apollo4 Plus board.

The UART interface uses the pin configuration defined in INFO0.

| Signal | MCU pin | Connection |
| --- | --- | --- |
| UART0 RX | GPIO47 | USB-UART adapter TX |
| UART0 TX | GPIO60 | USB-UART adapter RX |
| Ground | GND | USB-UART adapter GND |
| Boot Override | GPIO18 | Pull LOW during reset |

Use a USB-UART adapter compatible with the target GPIO voltage levels (1.8 V).

The UART communication parameters are:

| Parameter | Value |
| --- | --- |
| Interface | UART0 |
| Baud rate | 115200 |
| Data bits | 8 |
| Parity | None |
| Stop bits | 1 |
| Hardware flow control | Disabled |
| Boot Override | GPIO18, active LOW |
| Wired update timeout | 5000 ms |



#### 10.3.2. Prepare the UART Communication

Activate the Python virtual environment:

```bash
source "$HOME/apollo4-env/bin/activate"
```

Change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Identify the connected USB-UART adapter:

```bash
python3 -m serial.tools.list_ports -v
```

For the laboratory configuration described in this guide, the external FTDI adapter is connected through:

```bash
/dev/ttyUSB0
```

The actual serial device name may differ depending on the Ubuntu configuration.

Prepare the following command in the terminal, but do not execute it yet:

```bash
python3 uart_wired_update.py -b 115200 -r 0 -o 0xFFFFFFFF /dev/ttyUSB0
```

The command requests Secure Bootloader status information without downloading firmware, programming an OTA descriptor, or requesting a device reset.

The command uses the following parameters:

| Parameter | Value | Description |
| --- | --- | --- |
| -b | 115200 | UART baud rate |
| -r | 0 | Disables software reset by the utility |
| -o | 0xFFFFFFFF | No OTA descriptor address specified |
| Serial port | /dev/ttyUSB0 | External USB-UART adapter |

No firmware image is specified. The utility is used to establish communication with the Secure Bootloader and retrieve device status information.



#### 10.3.3. Enter Boot Override Mode

The INFO0 configuration specifies:

```bash
--wTO 5000
```

This configures a 5000 ms timeout for wired update communication.

Perform the following procedure:

1. Prepare the UART command in the Ubuntu terminal.

2. Pull GPIO18 LOW to assert Boot Override.

3. Reset the Apollo4 Plus device while GPIO18 remains LOW.

4. Immediately execute the prepared UART command by pressing Enter.

5. Confirm reception of the HELLO response and STATUS message.

The initial HELLO message must reach the Secure Bootloader within the configured 5-second communication window. If communication is not established, repeat the Boot Override entry sequence.



#### 10.3.4. Execute HELLO and STATUS Commands

The uart_wired_update.py utility communicates with the Secure Bootloader using the Ambiq wired update protocol.

When the Secure Bootloader accepts the request, it responds with a STATUS message containing information about the bootloader, security configuration, and device identification.

A successful communication sequence produces:

```text
Connecting with Apollo4 over serial port /dev/ttyUSB0...

Sending Hello.
Received response for Hello
Received Status

length = 0x88
version = 0x56a80000
Max Storage = 0x4fe30
Status = 0x91ee32b7
State = 0xa

chipId0 = [device-specific value]
chipId1 = [device-specific value]

SDCert = 0x1fc000
Resv = 0x0

SOCId =
[eight device-specific 32-bit words]

AMInfo =
[device security information]
```

Done.

The example illustrates the STATUS response format observed during laboratory verification. Device identifiers and security-related values may differ between devices and bootloader versions.

The response fields have the following meanings:

| Field | Description |
| --- | --- |
| length | STATUS response length in bytes. |
| version | Raw Secure Bootloader version field. Its encoded value should not be treated as a conventional software version without decoding. |
| Max Storage | Maximum temporary storage available for wired update operations. |
| Status | Raw Secure Bootloader status field. |
| State | Current Secure Bootloader protocol state. This is not the device Life Cycle State. |
| chipId0, chipId1 | Device identification words. |
| chipId1 | Device identification words. |
| SDCert | Configured Secure Debug Certificate memory address. |
| Resv | Reserved field. |
| SOCId | Device-specific 256-bit System-on-Chip Identifier. |
| AMInfo | Additional device and security configuration information. |



#### 10.3.5. Verify the Reported Security Configuration

The Secure Bootloader STATUS response provides information about the device configuration and identity.

Verify the following fields:

| Parameter | Expected result | Description |
| --- | --- | --- |
| HELLO response | Received response for Hello | Confirms successful communication with the Secure Bootloader. |
| STATUS response | Received Status | Confirms that the Secure Bootloader returned its status information. |
| Secure Debug Certificate address | SDCert = 0x1FC000 | Matches the address configured in INFO0 and the memory map defined in Table 1. |
| SOC-ID | Eight 32-bit words | Provides the 256-bit device identifier required for Secure Debug certificate generation. |
| Communication result | Done. | Confirms successful completion of the UART utility. |

The reported SDCert value identifies the configured storage location of the Secure Debug Certificate. It does not indicate that a certificate is already installed.

Secure LCS indication: In the verified Apollo4 Plus provisioning flow, the device returned an unpopulated SOC-ID (0xFFFFFFFF) before the transition to Secure LCS. After provisioning, the UART STATUS response returned the device-specific 256-bit SOC-ID. This provides an additional indication that Secure Enabled LCS is active.

Successful HELLO and STATUS communication confirms that the Secure Bootloader wired interface is operational. Firmware authentication and application execution are verified separately.



#### 10.3.6. Retrieve and Record the Device SOC-ID

The STATUS response contains the device-specific System-on-Chip Identifier (SOC-ID).

The SOC-ID consists of eight 32-bit words, representing a total of 256 bits.

The identifier is displayed in the following format:

```text
SOCId =
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
0xXXXXXXXX
```

The SOC-ID is required to generate the device-specific OEM Secure Debug Developer Certificate. The UART STATUS response should be saved to a local text file. Remain in the provisioning directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

To capture the output automatically, use the following command during the UART communication procedure described in Step 3:

```bash
python3 uart_wired_update.py -b 115200 -r 0 -o 0xFFFFFFFF /dev/ttyUSB0 | tee ./amota/uart_status.log
```

The command saves the complete UART output to:

```bash
./amota/uart_status.log
```

The saved log must contain the SOCId and AMInfo fields, with the eight SOC-ID words between them.

The following function extracts the SOC-ID from the saved STATUS response and prepares the text file required for certificate generation.

Copy the entire code block below and paste it into the Ubuntu terminal using Ctrl+Shift+V. Press Enter to execute the function.

```bash
apollo4_extract_socid() {
python3 - <<'PY'
from pathlib import Path
import re

source = Path("amota/uart_status.log")
output = Path("oem_tools_pkg/cert_utils/cert_gen_utils/inputData/socid.txt")

if not source.is_file():
    raise SystemExit("FAIL: UART STATUS log not found")

text = source.read_text(encoding="utf-8", errors="replace")

match = re.search(
    r"(?ms)^[ \t]*SOCId[ \t]*=[ \t]*\r?\n(.*?)^[ \t]*AMInfo[ \t]*=",
    text
)

if match is None:
    raise SystemExit("FAIL: SOC-ID section not found")

lines = [
    line.strip()
    for line in match.group(1).splitlines()
    if line.strip()
]

if len(lines) != 8:
    raise SystemExit("FAIL: Expected exactly eight SOC-ID words")

if not all(re.fullmatch(r"0x[0-9A-Fa-f]{1,8}", line) for line in lines):
    raise SystemExit("FAIL: Invalid SOC-ID word format")

words = [int(line, 16) for line in lines]

if all(word == 0 for word in words):
    raise SystemExit("FAIL: Uninitialized SOC-ID (all zeros)")

if all(word == 0xFFFFFFFF for word in words):
    raise SystemExit("FAIL: Uninitialized SOC-ID (all ones)")

formatted = " : ".join(f"0x{word:08X}" for word in words)

output.write_text(formatted + "\n", encoding="ascii")

print("PASS: Eight 32-bit SOC-ID words extracted")
print("PASS: SOC-ID text file generated")
print("Output:", output)
PY
}
apollo4_extract_socid
```

Expected confirmation:

```bash
PASS: Eight 32-bit SOC-ID words extracted
PASS: SOC-ID text file generated
```

The generated file is:

```bash
./oem_tools_pkg/cert_utils/cert_gen_utils/inputData/socid.txt
```

It contains eight hexadecimal words separated by colons, following the format used by the Apollo4 SOC-ID conversion utility.

Data format:

```bash
0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX : 0xXXXXXXXX
```

The word order is preserved exactly as reported by the Secure Bootloader.

Change to the certificate generation directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/cert_utils/cert_gen_utils"
```

Use the socid_to_bin.py conversion utility from the Apollo4 Secure Debug certificate preparation procedure.

Execute:

```bash
python3 socid_to_bin.py ./inputData/socid.txt -o ./inputData/soc_id1.bin
```

The generated output file is:

```bash
./inputData/soc_id1.bin
```

The conversion utility prepares the binary SOC-ID representation used by the OEM Debug Developer Certificate generator.

> [!NOTE]
> **Note:** If the utility socid_to_bin.py is missing in the AmbiqSuite_R4.5.0 it can be also obtained from AmbiqSuite_5.2.0 Apollo5 SDK.

Check the file size:

```bash
stat -c '%n: %s bytes' ./inputData/soc_id1.bin
```

The expected size is:

```bash
32 bytes
```

The generated binary file is now available for the OEM Debug Developer Certificate generation procedure in Chapter 11.

#### Preserve the Device Identification Record

Keep the following files together with the device provisioning record:

| File | Description |
| --- | --- |
| uart_status.log | Original Secure Bootloader UART STATUS output |
| socid.txt | Extracted SOC-ID in hexadecimal text format |
| soc_id1.bin | Binary SOC-ID input for Secure Debug certificate generation |

The SOC-ID must be associated with the correct physical device.

A Secure Debug Developer Certificate generated for another SOC-ID cannot authorize debugging of another device.



## 11. Secure Debug Certificate Generation and Installation

This chapter describes the generation and installation of a device-specific Secure Debug Certificate for an Apollo4 Plus device operating in Secure Enabled Life Cycle State (SE LCS).

Secure Debug allows selected debugging capabilities to be restored through authenticated certificates, according to the device security configuration.

The procedure uses the OEM cryptographic keys prepared in Chapter 6 and the device SOC-ID obtained through the Secure Bootloader UART interface in Chapter 10.3.6.

The Secure Debug Certificate is installed through the Secure Bootloader wired update interface, independently of the customer application.



### 11.1. Secure Debug Certificate Structure

Apollo4 Plus supports a three-level Secure Debug Certificate chain consisting of:

1. OEM Debug-Key Certificate — establishes authentication of the debug certificate hierarchy using the OEM Root of Trust.

2. OEM Debug Enabler Certificate — defines the authorized Debug Control Unit (DCU) permissions and authenticates the Developer Certificate public key.

3. OEM Debug Developer Certificate — specifies the requested debug permissions and binds the authorization to the device-specific SOC-ID.



### 11.2. Prepare the Certificate Generation Environment

Activate the Python virtual environment:

```bash
source "$HOME/apollo4-env/bin/activate"
```

Configure access to the legacy OpenSSL installation:

```bash
export PATH="$HOME/ambiq-legacy/openssl-install/bin:$PATH"
export LD_LIBRARY_PATH="$HOME/ambiq-legacy/openssl-install/lib${LD_LIBRARY_PATH:+:$LD_LIBRARY_PATH}"
```

Change to the certificate generation directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov/oem_tools_pkg/cert_utils/cert_gen_utils"
```

The procedure uses the OEM RSA keys previously selected for device provisioning.

The same OEM Root key hierarchy must be used for generating the Secure Debug Certificate chain.



### 11.3. Generate the OEM Debug-Key Certificate

The OEM Debug-Key Certificate establishes the first level of the Secure Debug authentication chain.

The certificate is generated using:

```bash
am_cert_key_util.py
```

Configuration file:

```bash
./am_config/am_oem_dbg_key_cert.cfg
```

Execute:

```bash
python3 am_cert_key_util.py ./am_config/am_oem_dbg_key_cert.cfg
```

The generated files are:

| File | Description |
| --- | --- |
| am_cert_key_util_output/debug_oem_key_cert.bin | Binary OEM Debug-Key Certificate |
| am_cert_key_util_output/debug_oem_key_cert_Cert.txt | Text representation of the certificate |

The certificate authenticates the public key used for the next level of the Secure Debug hierarchy.



### 11.4. Generate the OEM Debug Enabler Certificate

The OEM Debug Enabler Certificate defines the maximum debugging permissions that may be granted to the Developer Certificate.

The configuration includes DCU enable and lock masks, together with the public key used to authenticate the Developer Certificate.

The certificate must authorize the debugging capabilities required for the target device.

Configuration file:

```bash
./am_config/am_oem_dbg_enabler_cert.cfg
```

The utility is:

```bash
am_cert_dbg_enabler_util.py
```

Execute:

```bash
python3 am_cert_dbg_enabler_util.py ./am_config/am_oem_dbg_enabler_cert.cfg
```

The generated binary file is:

```bash
./am_debug_cert_output/oem_dbg_cert_enabler_pkg.bin
```

The Enabler Certificate incorporates the Debug-Key Certificate generated in Section 11.3.



### 11.5. Generate the Device-Specific OEM Debug Developer Certificate

The OEM Debug Developer Certificate is the final certificate in the Secure Debug authentication chain. It contains the device-specific SOC-ID and the selected DCU debug permissions. The SOC-ID was obtained from the Secure Bootloader STATUS response in Section 10.3.6. It consists of eight 32-bit words, representing a 256-bit device identifier. The Developer Certificate configuration must use the SOC-ID of the target device, preserving the required word and byte ordering.

Configuration file:

```bash
./am_config/am_oem_dbg_developer_cert.cfg
```

The utility is:

```bash
am_cert_dbg_developer_util.py
```

The configuration references the Debug Enabler Certificate generated in Section 11.4 and the corresponding OEM Developer signing key.

Execute:

```bash
python3 am_cert_dbg_developer_util.py ./am_config/am_oem_dbg_developer_cert.cfg
```

The generated files are:

| File | Description |
| --- | --- |
| am_debug_cert_output/oem_dbg_cert_developer_pkg.bin | Complete OEM Secure Debug Certificate package |
| am_debug_cert_output/oemDeveloperCert.h | C header representation of the Developer Certificate |

The binary package contains the certificate chain required by the device security subsystem. The Secure Debug Certificate is device-specific and must correspond to the SOC-ID of the target Apollo4 Plus.



### 11.6. Secure Debug Certificate Installation Through UART

The Apollo4 Plus Secure Bootloader supports installation of an OEM Secure Debug Certificate through its UART wired update interface. This mechanism allows a device operating in Secure Enabled LCS to receive a device-specific Secure Debug Certificate without requiring an active SWD connection.

The procedure uses the OEM Debug Developer Certificate generated in the preceding sections. The certificate contains the complete Secure Debug Certificate chain and is associated with the target device SOC-ID. The Secure Debug Certificate will be installed in the dedicated MRAM region defined in Table 1.



#### 11.6.1. Create the Secure Debug Wired Download Configuration

The Secure Debug Certificate must be packaged into the wired download format supported by the Ambiq Secure Bootloader.

The AmbiqSuite create_cust_image_blob.py utility is used to generate this package.

Change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Create the configuration file:

```bash
nano ./amota/wired_debug_cert.ini
```

Insert the following configuration:

```ini
[Settings]
ambiq_owned = 0x0
chip = apollo4b

app_file = ./oem_tools_pkg/cert_utils/cert_gen_utils/am_debug_cert_output/oem_dbg_cert_developer_pkg.bin
load_address = 0x001FC000

enc_algo = 0x0
auth_algo = 0x0
kek_index = 0x80

image_type = wired
ota = 0x0
sbl_ota = 0x0
offset = 0x0

certificate = None

output = ./amota/debug_cert_wired.bin
key_table = ./amota/keys.ini

loglevel = 0x2
wired_chunk_size = 0x40000
```

Save the file.

The configuration parameters are described below.

| Parameter | Value | Description |
| --- | --- | --- |
| chip | apollo4b | Apollo4 Plus image generation configuration |
| app_file | oem_dbg_cert_developer_pkg.bin | Complete OEM Secure Debug Certificate package |
| load_address | 0x001FC000 | Destination address in MRAM |
| enc_algo | 0 | No additional wired transport encryption |
| auth_algo | 0 | No additional wired transport RSA authentication |
| kek_index | 0x80 | Key encryption key index; not used when encryption is disabled |
| image_type | wired | Secure Bootloader wired download format |
| ota | 0 | Direct wired download without OTA staging |
| sbl_ota | 0 | SBL firmware update disabled |
| certificate | None | No separate firmware Content Certificate |
| output | debug_cert_wired.bin | Generated wired download package |
| key_table | ./amota/keys.ini | Existing OEM cryptographic key configuration |
| wired_chunk_size | 0x40000 | Maximum wired image chunk size, 256 KiB |

The transport image does not use additional encryption or RSA authentication. The Secure Debug Certificate itself contains the cryptographic signatures required for certificate-chain authentication by the device security subsystem.

The existing keys.ini file is reused without modification.



#### 11.6.2. Generate the Secure Debug Wired Package

Remain in the provisioning working directory:

```bash
cd ~/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov
```

Execute:

```bash
python3 create_cust_image_blob.py -c ./amota/wired_debug_cert.ini
```

The utility processes the complete Secure Debug Developer Certificate and generates the wired download image.

The resulting file is:

```bash
./amota/debug_cert_wired.bin
```

Verify that the file has been generated:

```bash
ls -l ./amota/debug_cert_wired.bin
```

Display its size:

```bash
stat -c '%n: %s bytes' ./amota/debug_cert_wired.bin
```

The generated file contains the Secure Debug Certificate and the wired download metadata required by the Secure Bootloader.

The binary package is now ready for transfer through UART.



#### 11.6.3. Install the Certificate Using Boot Override UART

Use the external USB-UART adapter is described in Chapter 10.

The INFO0 configuration specifies a wired update timeout of 5000 ms.

The debug certificate installation command must be be executed before this timeout expires.

Prepare the following command in the terminal, but do not execute it yet:

```bash
python3 uart_wired_update.py -b 115200 /dev/ttyUSB0 -r 1 -o 0xFFFFFFFF -f ./amota/debug_cert_wired.bin
```

The command parameters are:

| Parameter | Description |
| --- | --- |
| -b 115200 | UART communication speed |
| /dev/ttyUSB0 | External USB-UART adapter |
| -r 1 | Request POI reset after image download |
| -o 0xFFFFFFFF | Skip OTA Descriptor transmission |
| -f | Specifies the Secure Debug wired download package |

Perform the following sequence:

1. Ensure that the USB-UART adapter is connected and the prepared UART command is ready in the

Ubuntu terminal.

2. Assert Boot Override by pulling GPIO18 LOW.

3. Reset the Apollo4 Plus device while GPIO18 remains LOW.

4. Immediately press Enter to execute the prepared UART command.

5. Confirm that the Secure Bootloader responds to the HELLO request.

6. Allow the utility to transfer the complete Secure Debug wired image and process the received

acknowledgements.

The expected communication begins with:

```text
Connecting with Apollo4 over serial port /dev/ttyUSB0...

Sending Hello.
Received response for Hello
Received Status
```

After the STATUS response, the utility transmits the wired image using the Secure Bootloader update protocol. The transfer consists of an UPDATE command followed by DATA messages containing the prepared image. The Secure Bootloader acknowledges successfully received messages and processes the wired image according to the programmed device security policy. After completing the download, the utility sends the reset command selected by -r 1.

A successful transmission must finish without NACK responses, communication timeouts, or other reported errors.


> [!NOTE]
> **Important:** Successful UART transmission confirms that the wired download operation completed. Activation of the requested Secure Debug permissions must be verified separately after reset.



#### 11.6.4. Activate the Secure Debug Certificate

The installed Secure Debug Certificate is evaluated during device initialization.

After completing the UART download:

1. Release GPIO18 to deactivate Boot Override.

2. Perform a complete device reset or power cycle with GPIO18 inactive.

3. Allow the device to complete its normal initialization.

The device security subsystem processes the installed Secure Debug Certificate and applies the authorized Debug Control Unit (DCU) permissions.

The resulting debug access is determined by the certificate chain, device-specific SOC-ID, and configured DCU masks.

#### 11.6.5. Verify Debug Access

Connect the SEGGER J-Link debugger to the Apollo4 Plus evaluation board through SWD.

Open an Ubuntu terminal and execute:

```bash
JLinkExe -device AMAP42KP-KBR -if SWD -speed 4000 -autoconnect 1
```

Attempt to establish a debug connection.

If the Secure Debug Certificate has been accepted and the corresponding DCU permissions authorize SWD access, the debugger can connect to the device.

Once SWD communication has been restored, the contents of the protected device memory may be inspected according to the effective security permissions.

The first diagnostic operation should be a read-only examination of the MRAM regions containing the Certificate Chain Pointer Table, OEM certificates, and application firmware.

The Secure Debug Certificate installation does not modify the existing OEM Root, Key, or Content Certificates.

The ability to read individual MRAM regions depends on the effective device protection configuration.

#### 11.6.6. Removing the Secure Debug Certificate Using J-Link

The installed Secure Debug Certificate can be removed by erasing its dedicated 16 KB MRAM region using SEGGER J-Link Commander.

This operation requires an active SWD connection with sufficient memory access permissions.

Open an Ubuntu terminal and execute:

```bash
JLinkExe -device AMAP42KP-KBR -if SWD -speed 4000 -autoconnect 1
```

Erase the Secure Debug Certificate: At the J-Link Commander prompt, execute:

```bash
erase 0x001FC000 0x001FFFFF noreset
```

The command erases the 16 KB MRAM region reserved for the Secure Debug Certificate without initiating a reset before the erase operation.

#### Verify the Erased Memory

Read the beginning of the certificate region:

```bash
mem32 0x001FC000 8
```

Expected result:

```text
001FC000 = FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF
001FC010 = FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF
```

This confirms that the first 32 bytes contain the erased value. The erase operation must also report successful completion for the entire selected region.

#### Alternative Method — Overwriting the Secure Debug Certificate with 0xFF

As an alternative to the J-Link erase command, the Secure Debug Certificate can be removed by overwriting its dedicated 16 KB MRAM region with 0xFF bytes.

**Step 1 — Generate a Blank 16 KB Binary File**

In the Ubuntu terminal, change to the provisioning working directory:

```bash
cd "$HOME/AmbiqSuite_R4.5.0/tools/apollo4b_scripts_prov"
```

Generate a binary file containing 16,384 bytes of 0xFF:

```bash
python3 -c 'from pathlib import Path; Path("blank_cert_16k.bin").write_bytes(b"\xFF" * 16384)'
```

Verify the file size:

```bash
stat -c '%n: %s bytes' ./blank_cert_16k.bin
```

Expected result:

```bash
./blank_cert_16k.bin: 16384 bytes
```

**Step 2 — Overwrite the Secure Debug Certificate**

With an active J-Link SWD connection, execute the following command in J-Link Commander:

```bash
loadbin ./blank_cert_16k.bin 0x001FC000 noreset
```

This operation overwrites the entire Secure Debug Certificate region at 0x001FC000–0x001FFFFF without resetting the MCU before programming.

**Step 3 — Verify the Programmed MRAM Contents**

Execute:

```bash
verifybin ./blank_cert_16k.bin 0x001FC000
```

Expected result:

```bash
Reading 16384 bytes data from target memory @ 0x001FC000.
Verify successful.
```

The verifybin command compares all 16,384 bytes against the generated binary file, confirming that the entire Secure Debug Certificate region contains 0xFF.

> [!NOTE]
> **Important:** Removing the Secure Debug Certificate may cause SWD/SWO access to become unavailable after the next reset or power cycle. Restoring debug access may require installing a new valid, device-specific Secure Debug Certificate through the UART Boot Override interface.

After reset, SWD/SWO access may become unavailable again according to the device security policy. A new valid Secure Debug Certificate may be required to restore debugging capabilities.

#### Secure Debug Certificate Installation and Recovery

On Apollo4 Plus, Secure Debug Certificate installation operates independently of the OEM certificate chain used for Secure Boot.

The OEM Root, Key, and Content Certificates stored in MRAM are not required for Secure Debug Certificate installation or authentication. Secure Debug uses its own certificate chain (Debug-Key → Debug Enabler → Debug Developer), authenticated against the OEM Root of Trust provisioned in OTP.

A valid, device-specific Secure Debug Certificate can be installed through the SBL UART Boot Override interface, even when all three OEM Secure Boot certificates and the previously installed Secure Debug Certificate have been erased.

Following installation and reset, the authorized DCU permissions are restored, allowing SWD access in Secure LCS.

Secure Debug recovery remains available even when Secure Boot firmware validation fails because the OEM certificate chain is missing or invalid.




## References

The following official Ambiq documentation and software resources provide technical background for the security architecture, OEM provisioning, Secure Boot, firmware update mechanisms, and debugging features discussed in this guide.

**[1] Apollo4 Family OEM Provisioning, Update, and Tools User's Guide**

Ambiq Micro, Document A-SOCAP4-UGGA01EN, Revision 1.6, July 2023.

Describes OEM security asset generation, provisioning tools, certificate chains, INFO0 configuration, Secure Debug certificates, and firmware update procedures.

[Ambiq Content Portal — View Document](https://contentportal.ambiq.com/documents/20123/387772/Apollo4-Family-OEM-Provisioning-Update-and-Tools-Users-Guide.pdf)

**[2] Apollo4 Family Secure Update User's Guide**

Ambiq Micro, Document A-SOCAP4-UGGA02EN, Revision 1.4, July 2023.

Describes secure firmware update mechanisms, OTA image formats, authentication, encryption, OTA descriptors, wired update protocols, and recovery procedures.

[Ambiq Content Portal — View Document](https://contentportal.ambiq.com/documents/20123/387772/Apollo4-Family-Secure-Update-Users-Guide.pdf)

**[3] Apollo4 Family Security User's Guide**

Ambiq Micro, Document A-SOCAP4-UGGA05EN, Revision 1.3.

Provides reference information on the Apollo4 security architecture, lifecycle states, security policies, OTP configuration, Secure Boot, and Secure Debug functionality.

[Ambiq Content Portal — View Document](https://contentportal.ambiq.com/documents/20123/387772/Apollo4-Family-Security-Users-Guide.pdf)

**[4] Apollo4 Family Feature Support Restrictions Guide**

Ambiq Micro, Document FR-A4-4p0, Revision 4.0, July 2023.

Defines officially supported functionality and feature restrictions across the Apollo4 family. Restriction SECURITY-01 identifies Secure LCS as an unsupported feature, including on Apollo4 Plus devices.

[Ambiq Content Portal — View Document](https://contentportal.ambiq.com/documents/20123/387742/Apollo4-Family-Feature-Support-Restrictions-Guide.pdf)

**[5] AmbiqSuite SDK R4.5.0**

Ambiq Micro, Apollo4 Family Software Development Kit.

Provides the HAL, device headers, firmware examples, OEM provisioning utilities, certificate generation tools, Secure OTA utilities, and J-Link reference scripts used as the software foundation for the procedures described in this guide.

[Ambiq Content Portal — AmbiqSuite SDK R4.5.0](https://contentportal.ambiq.com/documents/20123/387817/AmbiqSuite-R4-5-0.zip)

---

**Note:** The references above are provided for technical background and further reading. The provisioning workflow, consolidated J-Link procedure, and laboratory validation presented in this guide represent the author's independent engineering work.

All referenced materials remain the property of their respective owners. Access to certain Ambiq resources may require registration or acceptance of the applicable license terms.




## Appendix: J-Link Utilities

SWO console version start:

```bash
JLinkSWOViewerCLExe -cpufreq 96000000 -swofreq 1000000 -device AMAP42KP-KBR -itmport 0 -swoattach 1
```

SWO graphics version start:

```bash
JLinkSWOViewerExe -cpufreq 96000000 -swofreq 1000000 -device AMAP42KP-KBR -itmport 0 -swoattach 1
```

SWD start:

```bash
JLinkExe -device AMAP42KP-KBR -if SWD -speed 4000 -autoconnect 1
```
