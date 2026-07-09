# Asterisk-pbx-from-scratch
# Overview 
This project documents the design and implementation of a production-style Asterisk PBX hosted on Microsoft Azure.  The objective is not only to install Asterisk, but to understand how enterprise voice platforms communicate with SIP carriers and process inbound and outbound calls.

# Technologies
- [x] Microsoft Azure
- [x] Ubuntu Server
- [x] Asterisk 20
- [x] PJSIP
- [x] SIP
- [x] RTP
- [x] Telnyx SIP Trunk
- [x] Linux
- [x] Networking

# Architecture

                    PSTN
                      │
                 Telnyx SIP
                      │
            Enterprise SIP Trunk
                      │
          Azure Ubuntu Virtual Machine
                      │
                 Asterisk PBX
              ┌────────┴────────┐
              │                 │
                Internal Users  



# What makes this project different?
This project focuses on understanding enterprise telephony architecture rather than simply installing Asterisk. 
Every configuration is manually created and documented to explain why each object exists, how SIP signaling works, and how the PBX interacts with carriers.

