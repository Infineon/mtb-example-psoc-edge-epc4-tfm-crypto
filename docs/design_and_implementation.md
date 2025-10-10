[Click here](../README.md) to view the README.

## Design and implementation

This Code example demonstrates how to use cryptography using Trusted Firmware-M (TF-M) with PSOC&trade; Edge MCU. Trusted Firmware-M (TF-M) implements the Secure Processing Environment (SPE) for Armv8-M, Armv8.1-M architectures (e.g., the Cortex&reg;-M33, Cortex&reg;-M23, Cortex&reg;-M55, Cortex&reg;-M85 processors) and dual-core platforms. It is the platform security architecture reference implementation aligning with PSA-certified guidelines, enabling chips,real time operating systems and devices to become PSA certified. For more details, see the [Trusted Firmware-M Documentation](https://tf-m-user-guide.trustedfirmware.org/).

The Extended boot launches the Edge Protect Bootloader from RRAM. The bootloader authenticates the CM33 secure, CM33 non-secure, and CM55 projects which are placed in External Flash and SRAM loads and launches the CM33 secure application from external Flash. The CM33 project contains the TF-M. The TF-M creates an isolated space between the M33 secure and M33 non-secure images. TF-M is available in source code format as a library in mtb_shared directory. The CM33 secure application does not have any source files instead includes this TF-M library from mtb-shared for building TF-M firmware.

After initializing the partitions, TF-M launches the M33 NSPE project from external flash which enables M55 and initializes  M33 NSPE <-> M55 NSPE interface using secure Request Framework (SRF). The M33 NSPE project requests TF-M for crypto service. In this example, TF-M uses Secure Enclave Runtime services (SE RT services) for cryptography implementation. The key lifitime attribute is used for informing TF-M that SE RT service should be used for crypto operation.

The M33 NSPE uses PSA API to show SHA256 hashing, EC signing/verification and AES AEAD encryption/decryption. All three crypto operations are done with SE RT services and the result of these crypto operations are logged on serial terminal.

**Table 1. Application projects**

Project | Description
--------|------------------------
proj_cm33_s | TF-M (SPE)
proj_cm33_ns | M33 NSPE
proj_cm55 | M55 NSPE

<br>