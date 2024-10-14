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
The **Mock-Seal Project** provides a streamlined and configurable method for generating self-signed certificates that include custom attributes, such as the Organization Identifier. These certificates are useful for digital signatures, encryption, or setting up secure communication between applications. The project leverages OpenSSL and provides an easy-to-use guide for creating and managing certificates in multiple formats. 

The goal of this project is to help developers, DevOps engineers, or system administrators quickly generate certificates with custom attributes for testing and demonstration purposes, as well as support broader digital credential use cases.

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

## About the Project

The **Mock-Seal Project** is part of the larger **BREVET project**, funded by **Erasmus+**, which aims to modernize the credentialing process in the **Vocational Education and Training (VET)** sector. As part of the **BrevApp** system, the Mock-Seal Project helps ensure that digital credentials comply with the **European Digital Credentials Infrastructure (EDCI)** standards. This tool supports VET institutions in issuing credentials that are secure, verifiable, and recognized across Europe.

With the ability to add custom fields such as the **Organization Identifier (OID)**, the Mock-Seal Project allows for realistic simulations of credential issuance scenarios. The integration into **BREVET** and **BrevApp** helps streamline the process of skills recognition in line with European frameworks like **ESCO** (European Skills, Competences, Qualifications, and Occupations) and **Europass**.

For more information about the BREVET project and BrevApp, please visit the [BREVET website](https://brevet.openrecognition.org/).

