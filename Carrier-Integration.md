# SIP trunk componenets: 

✅Authentication
✅Registration
✅Address of Record
✅Endpoint
✅Identify

-- > all these compnents are necessary and depends on the trunk registration method, because it's unlike a phone, we will need to identify how the trunk suppose to register. 
-- > We have three cases of a sip trunk registration : 
    1- Carrier registers to Asterisk
    2- Asterisk registers to Carrier
    3- No registration at all -> Just IP Authentication

-------------------------------------------------------
Step 1: Decide the trunk type 

Option B — IP Authentication

Telnyx trusts your Azure VM's public IP.
No SIP registration is required.

If someone calls
+1 XXX XXX XXXX

Telnyx will attempt toward Asterisk
INVITE sip:+13152735797@20.124.106.28
------------------------------------------------------

Step2: Atserisk Config
Since we have decided the trunk to register with IP auth we no longer need Auth or Registration objects. 

✅"The Telnyx SIP trunk."
[Carrier-Telnyx]
type=endpoint

✅"Where do I send SIP requests?"
[Carrier-Telnyx-aor]
type=aor

✅"How do I recognize packets coming from Telnyx?"
[Carrier-Telnyx-identify]
type=identify


PJSIP config: 
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Carrier : Telnyx SIP Trunk
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

[Carrier-Telnyx]
type=endpoint
transport=transport-udp --> reusable object we have already created earlier
context=from-telnyx -- > our CSS which will be configured under Extensions.config file / When Telnyx sends me an incoming call, where should I start processing it?
aors=Carrier-Telnyx-AOR -- > Telnyx address
identify_by=ip --> we are using IP authentication
disallow=all
allow=ulaw
direct_media=no
rtp_symmetric=yes
force_rport=yes
rewrite_contact=yes

[Carrier-Telnyx-AOR]
type=aor
contact=sip:sip.telnyx.com:5060
qualify_frequency=60

[Carrier-Telnyx-Identify]
type=identify
endpoint=Carrier-Telnyx
match=sip.telnyx.com


Extensions config: 
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Inbound PSTN Calls from Telnyx
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

[from-telnyx]
exten => _X.,1,NoOp(===== Incoming Call From Telnyx =====)    -- > Match any extension that starts with a digit and continues with one or more additional digits /// NoOp() This does nothing. It simply writes a message to the Asterisk console.
 same => n,NoOp(CallerID: ${CALLERID(num)}) 
 same => n,NoOp(Dialed Number: ${EXTEN})
 -- > when someone calls, the CLI will show: CallerID: and Dialed Number:
 same => n,Answer()  -- > Asterisk answers the call.
 same => n,Playback(demo-congrats) -- > This is a built-in Asterisk audio file.
 same => n,Hangup() -- > Cleanly terminates the call.


Results on CLI will be as follows: 

===== Incoming Call From Telnyx =====

CallerID: +13155551234

Dialed Number: +13152735797
