                         PSTN
                           │
                           ▼
                    Telnyx SIP Cloud
                           │
                    Enterprise SIP Trunk
                           │
          ┌───────────────────────────────────┐
          │      Azure Ubuntu VM (Azure)      │
          │                                   │
          │         Asterisk PBX              │
          │                                   │
          │  • PJSIP                          │
          │  • Dialplan                       │
          │  • SIP Routing                    │
          └──────────────┬────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
    Extension    Extension    Microsoft Teams
      (MicroSIP)      (MicroSIP)      (Operator Connect)
           │                               ▲
           └──────── SIP Calls ────────────┘

──────────────────────────────────────────────────────

                🚧 Next Project Phase

        • IVR
        • Voicemail
        • Call Queues
        • AI Voice Agent
        • Speech-to-Text
        • CRM Integration
