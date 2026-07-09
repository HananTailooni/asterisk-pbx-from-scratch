# Asterisk Architecture:
                   Caller

                      │
               SIP / PSTN Trunk

                      │
                +-------------+
                |  Asterisk   |
                | PBX Server  |
                +-------------+

          ┌─────────┼───────────┐
          │         │           │
      Extension   IVR       Voicemail
        1001                  Boxes
          │
      Softphone
	  
Where does the configurations live: 

Ubuntu
↓

/etc/asterisk/

↓
extensions.conf
pjsip.conf
voicemail.conf
queues.conf
...
