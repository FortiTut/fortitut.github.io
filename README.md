# FortiTutorials
### Disable SIP ALG
[Technical Tip: Disabling VoIP Inspection](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Disabling-VoIP-Inspection/ta-p/194131)
1. When a firewall policy has a voip-profile applied, SIP-ALG is used over SIP session-helper, even if disabled.
2. Disabling SIP session-helper is only necessary if ALL the SIP inspection must be removed.
The commands associated with the SIP-helper will not be relevant if the FortiGate is using SIP-ALG. Fine-tuning SIP-ALG is done through the voip profile.
3. Multi-vdom considerations: sip-helper is a global setting. Deleting sip-helper from global context, will make it inaccessible for all VDOMs. SIP-ALG is enabled (by default) and can be disabled per-vdom.
#### Disable SIP ALG per vDOM `(vdom0002) #`:
```
config system settings
set default-voip-alg-mode kernel-helper-based
end
``` 
Display the SIP session-helper entry number we need to edit (13 in this example)<br>
This is done from _global config_ mode if vDOMs are present:
```
(global) # config system session-helper
(session-helper) # show | grep -fi sip
```
Look for entry number containing sip/udp 5060:
```
config system session-helper
    edit 13
        set name sip <---
        set protocol 17
        set port 5060
    next
end
```
Delete the entry number:
```
delete 13
end
```
