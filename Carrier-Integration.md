# SIP trunk componenets: 

- [x] Authentication
- [x] Registration
- [x] Address of Record
- [x] Endpoint
- [x] Identify

- [x] -- > all these compnents are necessary and depends on the trunk registration method, because it's unlike a phone, we will need to identify how the trunk suppose to register. 
- [x] -- > We have three cases of a sip trunk registration : 
    - [x]  Carrier registers to Asterisk
    - [x]  Asterisk registers to Carrier
    - [x]  No registration at all -> Just IP Authentication

-------------------------------------------------------
- [x] Step 1: Decide the trunk type 

Option B — IP Authentication

- [x] Telnyx trusts your Azure VM's public IP.
- [x] No SIP registration is required.

If someone calls
+1 XXX XXX XXXX

Telnyx will attempt toward Asterisk
INVITE sip:+13152735797@20.124.106.28

------------------------------------------------------

- [x] Step2: Atserisk Config
Since we have decided the trunk to register with IP auth we no longer need Auth or Registration objects. 

- [x] "The Telnyx SIP trunk."
[Carrier-Telnyx]
type=endpoint

- [x] "Where do I send SIP requests?"
[Carrier-Telnyx-aor]
type=aor

- [x] "How do I recognize packets coming from Telnyx?"
[Carrier-Telnyx-identify]
type=identify


- [x] PJSIP config: 
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Carrier : Telnyx SIP Trunk
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

- [x]- [x]  [Carrier-Telnyx]
- [x] type=endpoint
- [x] transport=transport-udp --> reusable object we have already created earlier
- [x] context=from-telnyx -- > our CSS which will be configured under Extensions.config file / When Telnyx sends me an incoming call, where should I start processing it?
- [x] aors=Carrier-Telnyx-AOR -- > Telnyx address
- [x] identify_by=ip --> we are using IP authentication
- [x] disallow=all
- [x] allow=ulaw
- [x] direct_media=no
- [x] rtp_symmetric=yes
- [x] force_rport=yes
- [x] rewrite_contact=yes

- [x]- [x]  [Carrier-Telnyx-AOR]
- [x] type=aor
- [x] contact=sip:sip.telnyx.com:5060
- [x] qualify_frequency=60

- [x] - [x] [Carrier-Telnyx-Identify]
- [x] type=identify
- [x] endpoint=Carrier-Telnyx
- [x] match=sip.telnyx.com


- [x] Extensions config: 
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Inbound PSTN Calls from Telnyx
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

- [x] - [x] [from-telnyx]
- [x] exten => _X.,1,NoOp(===== Incoming Call From Telnyx =====)    -- > Match any extension that starts with a digit and continues with one or more additional digits /// NoOp() This does nothing. It simply writes a message to the Asterisk console.
- [x]  same => n,NoOp(CallerID: ${CALLERID(num)}) 
- [x]  same => n,NoOp(Dialed Number: ${EXTEN})
 -- > when someone calls, the CLI will show: CallerID: and Dialed Number:
- [x]  same => n,Answer()  -- > Asterisk answers the call.
- [x]  same => n,Playback(demo-congrats) -- > This is a built-in Asterisk audio file.
- [x]  same => n,Hangup() -- > Cleanly terminates the call.


- [x] - [x] Results on CLI will be as follows: 

- [x] ===== Incoming Call From Telnyx =====
- [x] CallerID: +13155551234
- [x] Dialed Number: +13152735797
