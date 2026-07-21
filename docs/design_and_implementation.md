[Click here](../README.md) to view the README.

## Design and implementation

This Code example demonstrates how to use cryptography using Trusted Firmware-M (TF-M) on a PSOC&trade; Edge MCU. Trusted Firmware-M (TF-M) implements the secure processing environment (SPE) for Armv8-M, Armv8.1-M architectures (for example, the Cortex&reg;-M33, Cortex&reg;-M23, Cortex&reg;-M55, Cortex&reg;-M85 processors), and dual-core platforms. TF-M serves as the reference implementation of the platform security architecture and aligns with PSA-certified guidelines, enabling chips, real-time operating systems, and devices to become PSA certified. For more details, see the [Trusted Firmware-M documentation](https://tf-m-user-guide.trustedfirmware.org/).

The Extended Boot launches the Edge Protect Bootloader from RRAM. The bootloader authenticates the CM33 secure, CM33 non-secure, and CM55 projects which are stored in external flash, loads them to SRAM, and then launches the CM33 secure application from the external flash. The CM33 project contains TF-M. The TF-M creates an isolated space between the M33 secure and M33 non-secure images. TF-M is available in source code format as a library in *mtb_shared* directory. The CM33 secure application does not contain any source files of its own; instead, it includes the TF-M library from *mtb-shared* to build the TF-M firmware.

After initializing the partitions, TF-M launches the M33 NSPE project from external flash which enables M55 and initializes the M33 NSPE <-> M55 NSPE interface using the Secure Request Framework (SRF). The M33 NSPE project requests TF-M for crypto service. In this example, TF-M uses Secure Enclave Runtime services (SE RT services) for the cryptography implementation. The key lifetime attribute is used to inform TF-M that the SE RT service should be used for the crypto operation.

The M33 NSPE uses PSA API to show SHA256 hashing, ECC signing/verification, and AES AEAD encryption/decryption. All three crypto operations are performed using SE RT services, and the result of these crypto operations are logged to the serial terminal.

**Table 1. Application projects**

Project | Description
--------|------------------------
proj_cm33_s | TF-M (SPE)
proj_cm33_ns | M33 NSPE
proj_cm55 | M55 NSPE

<br>