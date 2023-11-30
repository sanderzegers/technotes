# DHCP Snooping

DHCP snooping inspects DHCP packets and makes sure, that no rogues DHCP server, can hand out malicious or errornous DHCP Settings.

DHCP Server must sit behind trusted ports:

DHCP OFFER and ACK packets are only allowed on trusted ports.

DHCP snooping is a per-vlan setting.

```
config system interface
  edit "VLAN200"
     set ip 10.1.5.1 255.255.255.0
     set switch-controller-dhcp-snooping enable
  next
 end
```

DHCP Server should either be behind a trusted port, or defined in the Trusted DHCP Server access list.

Trusted Port can be assigned via the GUI:

<figure><img src="../.gitbook/assets/grafik (3).png" alt=""><figcaption></figcaption></figure>

```
config switch-controller managed-switch
   edit "S448EFTF01234556"
      config ports
         edit "port1"
            set dhcp-snooping trusted
         next
      end
   end
end
```

When an untrusted DHCP server is detected or when OFFER or ACK DHCP packets are detected on an untrusted port, following Log message will be created and the packet will be blocked:

<figure><img src="../.gitbook/assets/grafik (22).png" alt=""><figcaption></figcaption></figure>

Optionally, it's also possible to not forward any DHCP DISCOVER and REQUEST packets to untrusted ports. This is to prevent information leakage to non dhcp-servers. This is a FortiSwitch only setting. By default it is configured as 'forwarded-untrusted.

```
SW-1# config system global
SW-1 (global)# set dhcp-snoop-client-req drop-untrusted
```

To display the DHCP snooping status:

```
FG60E_FG1 # diagnose switch-controller switch-info dhcp-snooping status S448EFTF23000000:
Vdom: root

S448EFTF23000000:

Client db: 

Server db: 

      mac        vlan          ip            interface          status           svr-state          last-seen-time             expiry-time         OFFER/ACK/NAK/OTHER
00:09:0f:09:00:08  200       10.1.5.1      GT60E4Q16013456       trusted          disabled         2023-11-03 15:30:07       2023-11-04 15:30:07        6/5/2/0      
00:0c:29:00:11:22  200      10.1.5.101          port1           untrusted         disabled         2023-11-03 15:30:07       2023-11-04 15:30:07        4/4/0/0      

Client6 db: 

Server6 db: 


```

To view the DHCP snooping database:

```
FG60E_FG1 # diagnose switch-controller switch-info dhcp-snooping database S448EFTF23000000:
Vdom: root

S448EFTF23011843:

snoop-enabled-vlans             : 200
verifysrcmac-enabled-vlans      :  
option82-enabled-vlans          :  
option82-trust-enabled-intfs    :
trusted ports    : _FlInK1_ICL0_ SW3 SW2 GT60E4Q16013456
untrusted ports  : port1 port2 port3 port4 port5 port6 port7 port8 port9 port10
                  port11 port12 port13 port14 port15 port16 port17 port18 port19 port24
                  port25 port26 port27 port28 port29 port30 port31 port32 port33 port34
                  port35 port36 port37 port38 port39 port40 port41 port42 port43 port44
                  port45 port46 port49 port50 port51 port52
Max Client Database Entries      : 8000
        Client Database          : 0
        Client6 Database         : 0
Max Server Database Entries      : 1024
        Server Database          : 2
        Server6 Database         : 0 
Limit Database           : 2 / 256

DHCP Global Configuration: 
========================== 
DHCP Broadcast Mode              : Trusted 
DHCP Allowed Server List         : Disable 
Add hostname in Option82         : Disable 

```

DHCP Broadcast mode is the status of the previously described dhcp-snoop-client-req setting. Max Client Database differs per switch model. If this limit is reached, no additional DHCP leases are allowed!

### DHCP Option 82

DHCP Option 82 also known as DHCP Relay Agent Information, is used to provide additional information about the client's physical connection to the network. The Switch adds information such as switch mac address, Port ID and VLAN ID to the DHCP request. Now the DHCP server knowns exactly where the request is coming from.\
This allows network administrator to apply policies and control IP address allocation based on the location of the DHCP client in the network.

It protects the DHCP server from DHCP exhaustion attacks and spoofed DHCP requests.\
This feauture can be enabled on top of the default DHCP Snooping.

Following information is included in DHCP Option 82:

* Circuit ID
  * Client port ID
  * Client VLAN ID
  * DHCP mode: dhcp-s (snooping) / dhcp-r (relay)
* Remote ID
  * FortiSwitch MAC Address (internal interface)

Packet flow:

1. A client device connects to a switch and sends out a DHCP request.
2. The switch, acting as a relay agent, adds Option 82 to the request, including details about which port the client is connected to (Circuit ID) and the switch's identifier (Remote ID).
3. The relay agent forwards this DHCP request to the DHCP server.
4. The DHCP server, seeing the Option 82 information, can decide to assign an IP address based on the specific switch and port the client is connected to.
5. The DHCP server sends back the offer, which the relay agent then forwards to the client.

Because FortiSwitch is adding this details, a dhcp exhaustion attack is not possible anymore.

Option 82 is enabled per VLAN on top of dhcp-snooping setting.

```
config system interface
   edit VLAN200
      set switch-controller-dhcp-snooping enable
      set switch-controller-dhcp-snooping-option82 enable
```

By default packets with option 82 are dropped on the FortiSwitch if they are not coming through a FortiLink-ISL.&#x20;

So in case a downstream switch outside of the FortiLink fabric is adding port 82, the switch port on the root switch must be explictly allowed to accept port82 packets with following options:

<pre><code>config switch-controller managed-switch
    edit rootswitch
        config ports
            set dhcp-snoop-option82-trust enable
<strong>        next
</strong>    end
end
</code></pre>

### MAC Verification

Instructs FortiSwitch to verify that the source MAC address in the Ethernet Header and the client hardware address in the DHCP header match.

```
config system interface
   edit VLAN200
      set switch-controller-dhcp-snooping enable
      set switch-controller-dhcp-snooping-option82 enable
      set switch-controller-dhcp-snooping-verify-mac enable
```

## Logs / Diagnoses

