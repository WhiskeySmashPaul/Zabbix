# Zabbix Template for Quantum Scalar i3/i6 Monitoring

## Introduction

This repository contains a Zabbix template for monitoring Quantum Scalar i3 or i6 tape libraries using SNMP. This template has been tested on Zabbix version 7.0 and supports both SNMPv2 and SNMPv3 protocols.

## Author
WhiskeySmashPaul

## Features

- **SNMP Monitoring**: The template utilizes SNMP to monitor various aspects of the Quantum Scalar i3/i6 tape libraries.
- **Triggers for Alerts**: Includes several triggers to alert on issues such as drive status warnings, power supply count issues, and more.
- **Discovery Rules**: Automatically discovers management interfaces, RAS event tickets, logical libraries, and physical drives.

## Requirements

- Zabbix 7.0 or higher
- Quantum Scalar i3/i6 tape library
- SNMPv2 or SNMPv3 configured on the tape library

## Installation

### Importing the Template

1. Download the `Quantum Scalar i3 - i6.xml` file from this repository.
2. Log in to your Zabbix frontend.
3. Navigate to **Configuration** -> **Templates**.
4. Click on the **Import** button in the top right corner.
5. Choose the `Quantum Scalar i3 - i6.xml` file and click **Import**.

### Configuring SNMP

#### On the Quantum Scalar i3/i6

1. **Access the Management Interface:**
   - Open a web browser and log in to the Quantum Scalar i3/i6 management interface using its IP address.

2. **Navigate to SNMP Settings:**
   - Go to **System** -> **Operations** -> **SNMP**.

3. **Configure SNMP:**
   - **Enable SNMP**: Ensure that SNMP is enabled.
   - **Set Community Strings (for SNMPv2)**: If using SNMPv2, configure the read-only community string (e.g., `public`).
   - **Enable SNMPv3 (if applicable)**: 
     - The username if utilizing SNMPv3 is your admin account for the Quantum.
   - **Save the Configuration**: Ensure all settings are saved.

4. **Verify SNMP Access:**
   - Use an SNMP tool (e.g., snmpwalk) to verify that SNMP is working correctly by querying the Quantum Scalar i3/i6.

#### Setting Up the Host SNMP in Zabbix

1. **Create a New Host in Zabbix:**
   - Go to **Configuration** -> **Hosts**.
   - Click on **Create Host**.

2. **Configure Host Details:**
   - **Host name**: Enter a name for your host (e.g., `Quantum Scalar i6`).
   - **Visible name**: Optional, for display purposes.
   - **Groups**: Select a host group or create a new one.
   - **Agent interfaces**: Remove if not needed.
   - **SNMP interfaces**: Add an SNMP interface with the IP address of the Quantum Scalar i6.

3. **Set Up SNMP Parameters:**
   - **SNMP Version**: Choose the version that matches your Scalar i6 configuration (SNMPv2 or SNMPv3).
   - **SNMP Community (for SNMPv2)**: Enter the community string configured on the Scalar i6.
   - **SNMPv3 Settings (if applicable)**: Enter the username, authentication, and privacy protocols, and their respective passwords.
     - Enter your security name as the admin username for the Quantum Scalar i3/i6
     - Set security level as authNoPriv
     - Set Authentication protocol to MD5
     - Enter you admin password in the Authentication passphrase field.

4. **Link the Zabbix Template:**
   - Go to the **Templates** tab and click on **Link new template**.
   - Search for the "Quantum i6" template and link it to the host.

5. **Apply and Test:**
   - Save the host configuration and monitor the data under **Monitoring** -> **Latest data** to ensure that SNMP data is being correctly gathered.

## Usage

Once the template is linked to a host, Zabbix will start monitoring the Quantum Scalar i3/i6 tape library. You can check the monitoring data under **Monitoring** -> **Latest data**.

## Items Collected

The following table lists the items and item prototypes collected by this template, along with their descriptions, types, and keys.

| Item Name                                | Description                                        | Item Type  | Item Key                                             |
|------------------------------------------|----------------------------------------------------|------------|------------------------------------------------------|
| Quantum_i6_aggregatedIEAreaStatus        | Physical library's overall insert/eject area access status | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.aggregatedIEAreaStatus |
| Quantum_i6_aggregatedMagazineStatus      | Physical library's overall magazine presence status | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.aggregatedMagazineStatus |
| Quantum_i6_driveRASStatus                | Indicates overall library drive status             | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.driveRASStatus      |
| Quantum_i6_libraryCleaningTapeCount      | Total number of library-configured cleaning tapes  | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryCleaningTapeCount |
| Quantum_i6_libraryGlobalStatus           | Current RAS status of the entire library           | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryGlobalStatus |
| Quantum_i6_libraryMediaCount             | Total number of media minus any configured cleaning tapes | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryMediaCount   |
| Quantum_i6_libraryName                   | Host name (DNS alias) of the tape library          | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryName         |
| Quantum_i6_librarySerialNumber           | Library serial number                              | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.librarySerialNumber |
| Quantum_i6_libraryStorageSlotCount       | Number of overall library storage slots            | SNMP_AGENT | QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryStorageSlotCount |

### Item Prototypes

The following table lists the item prototypes discovered by this template.

| Item Prototype Name                        | Description                                        | Item Type  | Item Key                                                  |
|--------------------------------------------|----------------------------------------------------|------------|-----------------------------------------------------------|
| managementInterfaceAccessType[{#SNMPINDEX}]| Management interface protocol access type          | SNMP_AGENT | managementInterfaceAccessType.[{#SNMPINDEX}]              |
| managementInterfaceAddress[{#SNMPINDEX}]   | Management interface IP address                    | SNMP_AGENT | managementInterfaceAddress.[{#SNMPINDEX}]                 |
| managementInterfaceProtocol[{#SNMPINDEX}]  | Management interface protocol and version          | SNMP_AGENT | managementInterfaceProtocol.[{#SNMPINDEX}]                |
| logicalLibraryAutoClean[{#SNMPINDEX}]      | Partition's automatic drive cleaning support configuration | SNMP_AGENT | logicalLibraryAutoClean.[{#SNMPINDEX}]                   |
| logicalLibraryChangerDeviceAddr[{#SNMPINDEX}]| First partition medium transport SCSI element address | SNMP_AGENT | logicalLibraryChangerDeviceAddr.[{#SNMPINDEX}]            |
| logicalLibraryControl[{#SNMPINDEX}]        | Partition control path configuration               | SNMP_AGENT | logicalLibraryControl.[{#SNMPINDEX}]                     |
| logicalLibraryInterface[{#SNMPINDEX}]      | Partition control interface method                 | SNMP_AGENT | logicalLibraryInterface.[{#SNMPINDEX}]                   |
| logicalLibraryMode[{#SNMPINDEX}]           | Partition online/offline mode                      | SNMP_AGENT | logicalLibraryMode.[{#SNMPINDEX}]                        |
| logicalLibrarySerialNumber[{#SNMPINDEX}]   | Partition serial number                            | SNMP_AGENT | logicalLibrarySerialNumber.[{#SNMPINDEX}]                |
| logicalLibraryState[{#SNMPINDEX}]          | Partition ready/not-ready status                   | SNMP_AGENT | logicalLibraryState.[{#SNMPINDEX}]                       |

## Triggers

The following table lists the triggers included in this template, along with their corresponding expressions.

| Name                                      | Expression                                                                                 |
|-------------------------------------------|-------------------------------------------------------------------------------------------|
| IE Area Access Issue Detected             | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.aggregatedIEAreaStatus)=2`            |
| Magazine Access Issue Detected            | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.aggregatedMagazineStatus)=2`          |
| Drive RAS Status Warning Detected         | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.driveRASStatus)=3`                    |
| Library Global Status Issue Detected      | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryGlobalStatus)=1`               |
| Low Cleaning Tape Count Detected          | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryCleaningTapeCount)<"{$MinCleaningTapeCount}"` |
| Power Supply Count Issue Detected         | `last(/Quantum i6/QUANTUM-MIDRANGE-TAPE-LIBRARY-MIB.libraryPSCount)<"{$MinPSCount}"`      |

### Trigger Prototypes

The following table lists the trigger prototypes discovered by this template.

| Name                                      | Expression                                                                                 |
|-------------------------------------------|-------------------------------------------------------------------------------------------|
| Management Interface Access Type [{#SNMPINDEX}]: IP Address Change | `last(/Quantum i6/managementInterfaceAccessType.[{#SNMPINDEX}])<>"{$ExpectedAccessType}"` |
| Management Interface Access Type [{#SNMPINDEX}]: Unexpected Access Type | `last(/Quantum i6/managementInterfaceAccessType.[{#SNMPINDEX}])<>"{$ExpectedAccessType}"` |
| Management Interface Access Type [{#SNMPINDEX}]: Protocol Mismatch | `last(/Quantum i6/managementInterfaceProtocol.[{#SNMPINDEX}])<>"{$ExpectedProtocol}"`      |
| Logical Library Degraded on Partition [{#SNMPINDEX}] | `last(/Quantum i6/logicalLibraryControl.[{#SNMPINDEX}])=3`                                |
| Logical Library Path Failover on Partition [{#SNMPINDEX}] | `last(/Quantum i6/logicalLibraryControl.[{#SNMPINDEX}])=4`                                |
| Logical Library Offline on Partition [{#SNMPINDEX}] | `last(/Quantum i6/logicalLibraryMode.[{#SNMPINDEX}])=2`                                   |
| Logical Library Not Ready on Partition [{#SNMPINDEX}] | `last(/Quantum i6/logicalLibraryState.[{#SNMPINDEX}])=2`                                  |

## Macros Used

The following table lists the macros used in this template, along with their descriptions and values.

| Name                       | Description                                      | Value       |
|----------------------------|--------------------------------------------------|-------------|
| {$MinCleaningTapeCount}     | Minimum number of cleaning tapes allowed         | 2 (example) |
| {$MinPSCount}               | Minimum number of power supplies required        | 1 (example) |
| {$ExpectedAccessType}       | Expected management interface access type        | HTTPS       |
| {$ExpectedProtocol}         | Expected management interface protocol           | IPv4        |

## Value Mapping

The following table provides the value mappings used in this template.

| Name                       | Description                              | Mapped Values                               |
|----------------------------|------------------------------------------|---------------------------------------------|
| RASSubSystemStatus          | Status of the RAS subsystem              | 0: OK, 1: Warning, 2: Critical              |
| DisabledEnabled             | Indicates whether a feature is enabled   | 0: Disabled, 1: Enabled                     |
| DeviceMode                  | Mode of the device                       | 0: Offline, 1: Online                       |
| LibraryState                | Ready/not-ready status of the library    | 0: Not Ready, 1: Ready                      |
| PartitionType               | Type of partition configuration          | 1: Redundant, 2: Non-redundant              |
| NetworkProtocol             | Network protocol version                 | 1: IPv4, 2: IPv6                            |

## Contributing

If you would like to contribute to this project, please fork the repository and submit a pull request. You can also open an issue if you encounter any problems or have suggestions for improvements.

## License

This project is licensed under the GNU General Public License v3.0. See the [LICENSE](LICENSE) file for details.

## Contact

For any questions or support, please reach out to the repository maintainer.

- **GitHub**: [WhiskeySmashPaul](https://github.com/WhiskeySmashPaul)
