# Mock-Seal Project

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
  - [Generating Certificates](#generating-certificates)
  - [Adding Organization Identifier (OID)](#adding-organization-identifier-oid)
  - [Converting to .p12 and .pem Formats](#converting-to-p12-and-pem-formats)
  - [Viewing Certificate Details](#viewing-certificate-details)
- [Common Use Cases](#common-use-cases)
- [Author](#author)
- [Contact](#contact)

## Overview
This code is part of the **BrevApp** system, developed under the **BREVET project**, funded by **Erasmus+**. BrevApp is a digital credentialing tool designed to assist **Vocational Education and Training (VET)** institutions in issuing credentials that comply with the **European Digital Credentials Infrastructure (EDCI)**. The purpose of BrevApp is to provide a streamlined, user-friendly solution for issuing digital credentials recognized across Europe.

This specific section of the code includes functionality for generating **mock-seals** and custom attributes, which help simulate real-world credentialing scenarios. It plays a critical role in ensuring that BrevApp can generate certificates with necessary fields like the **Organization Identifier (OID)** and export them in various formats (`.crt`, `.p12`, `.pem`). The generated credentials align with European frameworks such as **ESCO** (European Skills, Competences, Qualifications, and Occupations) and **Europass**, ensuring portability and recognition across the EU.

By making this open-source, we aim to contribute to the wider adoption and development of digital credentialing systems across educational institutions and organizations.

For more details about the BREVET project and BrevApp, please visit the [BREVET website](https://brevet.openrecognition.org/).


## Features
- **Custom Certificate Attributes**: Support for adding custom X.509 fields such as the organization identifier, along with standard fields (Country, State, Organization, etc.).
- **Multiple Certificate Formats**: Ability to export certificates to `.crt`, `.p12`, and `.pem` formats for various uses.
- **OpenSSL Configuration**: Provides an easily configurable OpenSSL setup for fast certificate generation.
- **Realistic Mock Data**: Includes mock data in certificates to simulate real-world scenarios for testing environments.
- **Extensible**: The project can be easily modified to generate certificates with additional fields and extensions.
- **Cross-Platform Compatibility**: Works on Linux, macOS, and any system with OpenSSL installed.

## Requirements
To use the Mock-Seal Project, you will need the following:
- **OpenSSL**: Version 1.1.1 or later installed on your system.
- **UNIX-based OS**: Works best on Linux or macOS, but can also be run on a Linux VM like VirtualBox.
- **Basic command-line knowledge**: Some familiarity with running commands in a terminal.

## Installation
1. **Verify OpenSSL Installation**: 
   Ensure that OpenSSL is installed on your system. You can verify this by running the command:
   
   openssl version
If OpenSSL is not installed, you can install it using your package manager:

On Ubuntu/Debian:


    sudo apt-get update
    sudo apt-get install openssl

On macOS:

    brew install openssl

    Set Up OpenSSL Configuration: The repository includes an openssl.cnf configuration file that defines the certificate details, including custom attributes. Modify the configuration file if needed.

## Usage
### Generating Certificates

    Generate Private Key and CSR (Certificate Signing Request): Use the following command to generate a private key and CSR:


    openssl req -new -nodes -out mycsr.csr -newkey rsa:2048 -keyout mykey.key -config openssl.cnf

### Generate a Self-Signed Certificate: Create the self-signed certificate using the CSR:


    openssl x509 -signkey mykey.key -in mycsr.csr -req -days 365 -out mycert.crt -extfile openssl.cnf -extensions v3_req

### Adding Organization Identifier (OID)

To include an Organization Identifier in your certificate:

Modify the OpenSSL Configuration File: Open openssl.cnf and add the organizationIdentifier field under the [ req_distinguished_name ] section:


    organizationIdentifier = ASN1:PRINTABLESTRING:123456789

Generate the Private Key and CSR: Run the following command:


    openssl req -new -nodes -out mycsr.csr -newkey rsa:2048 -keyout mykey.key -config openssl.cnf

Generate the Self-Signed Certificate:


    openssl x509 -signkey mykey.key -in mycsr.csr -req -days 365 -out mycert.crt -extfile openssl.cnf -extensions v3_req

### Converting to .p12 and .pem Formats

Convert to .p12 (PKCS#12) Format:


    openssl pkcs12 -export -out mycert.p12 -inkey mykey.key -in mycert.crt -certfile mycert.crt

Convert to .pem (PEM) Format:


    openssl pkcs12 -in mycert.p12 -out mycert.pem -nodes

### Viewing Certificate Details

To verify the details of the generated certificate:


    openssl x509 -in mycert.crt -text -noout

## Common Use Cases

    Digital Signatures: Use the certificate to digitally sign documents or software artifacts.
    Secure Communication: The .p12 format can be used for configuring secure communications in web servers, VPNs, or email systems.
    Testing and Development: Mock certificates with realistic data are useful in test environments without needing a CA-issued certificate.
    Learning and Demos: Perfect for demonstrating certificate creation and management.

## Author

Joseph Terzis
Web2Learn | Thessaloniki, Greece

## Contact

For any inquiries or further assistance, feel free to reach out:

    Email: joshterzis@gmail.com





