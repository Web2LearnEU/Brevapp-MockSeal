Table of Contents
Overview
Features
Requirements
Installation
Usage
Generating Certificates
Adding Organization Identifier (OID)
Converting to .p12 and .pem Formats
Viewing Certificate Details
Common Use Cases
Author

Overview
The Mock-Seal Project provides a streamlined and configurable method for generating self-signed certificates that include custom attributes, such as the Organization Identifier. These certificates are useful for digital signatures, encryption, or setting up secure communication between applications. The project leverages OpenSSL and provides an easy-to-use guide for creating and managing certificates in multiple formats.
The goal of this project is to help developers, DevOps engineers, or system administrators quickly generate certificates with custom attributes for testing and demonstration purposes, as well as support broader digital credential use cases.

Features
Custom Certificate Attributes: Support for adding custom X.509 fields such as the organization identifier, along with standard fields (Country, State, Organization, etc.).
Multiple Certificate Formats: Ability to export certificates to .crt, .p12, and .pem formats for various uses.
OpenSSL Configuration: Provides an easily configurable OpenSSL setup for fast certificate generation.
Realistic Mock Data: Includes mock data in certificates to simulate real-world scenarios for testing environments.
Extensible: The project can be easily modified to generate certificates with additional fields and extensions.
Cross-Platform Compatibility: Works on Linux, macOS, and any system with OpenSSL installed.

Requirements
To use the Mock-Seal Project, you will need the following:
OpenSSL: Version 1.1.1 or later installed on your system.
UNIX-based OS: Works best on Linux or macOS, but can also be run on a Linux VM like VirtualBox.
Basic command-line knowledge: Some familiarity with running commands in a terminal.

Installation
Verify OpenSSL Installation: Ensure that OpenSSL is installed on your system. You can verify this by running:
openssl version
If OpenSSL is not installed, you can install it using your package manager:
On Ubuntu/Debian:
sudo apt-get update
sudo apt-get install openssl
On macOS:
brew install openssl
Set Up OpenSSL Configuration: The repository includes an openssl.cnf configuration file that defines the certificate details, including custom attributes. Modify the configuration file if needed.

Usage
Generating Certificates
Generate Private Key and CSR (Certificate Signing Request):
Use the following command to generate a private key and CSR. This CSR will be used to create the self-signed certificate.
openssl req -new -nodes -out mycsr.csr -newkey rsa:2048 -keyout mykey.key -config openssl.cnf
Generate a Self-Signed Certificate:
Create the self-signed certificate using the CSR generated in the previous step:
openssl x509 -signkey mykey.key -in mycsr.csr -req -days 365 -out mycert.crt -extfile openssl.cnf -extensions v3_req
The certificate (mycert.crt) is now ready for use and is valid for 365 days.

Adding Organization Identifier (OID)
If you want to include an Organization Identifier in your certificate, you can use OpenSSL's built-in support for the organizationIdentifier OID. Follow these steps:
Modify the OpenSSL Configuration File:
Open the openssl.cnf file in a text editor:
nano openssl.cnf
Add the organizationIdentifier field under the [ req_distinguished_name ] section, like this:
[ req ]
default_bits = 2048
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no
[ req_distinguished_name ]
C = GR
ST = Thessaloniki
L = Thessaloniki
O = MockOrganization
OU = MockOrganizationalUnit
emailAddress = mockemail@mockdomain.com
CN = www.mockwebsite.gr
organizationIdentifier = ASN1:PRINTABLESTRING
[ v3_req ]
basicConstraints = CA

keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
[ alt_names ]
DNS.1 = www.mockwebsite.gr
DNS.2 = mockwebsite.gr
Generate the Private Key and CSR:
Use the following command to generate a new private key and CSR with the updated configuration:
openssl req -new -nodes -out mycsr.csr -newkey rsa:2048 -keyout mykey.key -config openssl.cnf
Generate the Self-Signed Certificate:
Create the self-signed certificate using the CSR:
openssl x509 -signkey mykey.key -in mycsr.csr -req -days 365 -out mycert.crt -extfile openssl.cnf -extensions v3_req
Verify the Certificate:
Check the details of your new certificate:
openssl x509 -in mycert.crt -text -noout
This process should place the organizationIdentifier in the subject name of your certificate.

Converting to .p12 and .pem Formats
You can convert your certificate and private key to different formats for various applications:
Convert to .p12 (PKCS#12) Format:
openssl pkcs12 -export -out mycert.p12 -inkey mykey.key -in mycert.crt -certfile mycert.crt
Convert to .pem (PEM) Format:
openssl pkcs12 -in mycert.p12 -out mycert.pem -nodes

Viewing Certificate Details
To verify the details of the generated certificate:
openssl x509 -in mycert.crt -text -noout
This command will display the certificate details, including the custom attributes like organizationIdentifier, country, state, etc.

Common Use Cases
Digital Signatures: The certificate generated can be used to digitally sign documents or software artifacts, ensuring authenticity.
Secure Communication: The .p12 format can be used for configuring secure communications in web servers, VPNs, or email systems.
Testing and Development: Mock certificates with realistic data can be useful in test environments without needing a CA-issued certificate.
Learning and Demos: A perfect tool for demonstrating certificate creation, management, and usage in workshops or educational settings.


Author
Joseph Terzis
Web2Learn | Thessaloniki, Greece

Contact
For any inquiries or further assistance, you can reach me at:
Email: joshterzis@gmail.com
