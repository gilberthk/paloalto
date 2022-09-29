<link href="style.css" rel="stylesheet"></link>

<a class="top-link hide" href="#top">↑</a>
<a name="top"></a>

# Palo Alto initialise
> ### **Clear default setting**
1. Connect PA with MGT port;
2. Set local host ip under the same subnet with PA (default 192.168.1.1/24);
3. Delete **POLICIES** -> **Security** -> **rule** (if exits);
4. Delete all rules in **POLICIES** -> **NAT**;
5. Delete **NETWORK** -> **Virtual Routers** -> **Default**;
6. Delete **NETWORK** -> **Virtual Wires** -> **default**;
7. Delete **NETWORK** -> **Zones** -> **trust** and **untrust**,
8. Delete **NETWORK** -> **Interfaces** -> **ethernet1/1** and **ethernet1/3**.

> ### **Configuration**
1. In **NETWORK** -> **Interfaces**, **Add** two interfaces, for example, **WAN** and **lan**;

![WAN](/Assets/WAN_interface.png)
<details>

> Both Interface type: Layer 3;\
> set up security zone for each interface (layer 3)\
> WAN_security_zone:\
![WAN_security_zone](/Assets/WAN.png)
> lan_security_zone:\
![lan_security_zone](/Assets/lan.png)
<g>//just click Security zone and clike OK to create</g>
![create_zone](/Assets/create_zone.png)
</details>

1. In **NETWORK** -> **Virtual Routers**, **Add** a route for WAN infertace.

![route_config](/Assets/Route_set_1.png)
Gateway as Next Hop:
![route_config](/Assets/Route_set_2.png)


2. In **POLICIES** -> **NAT**, **Add** a new NAT policy.  Point lan interface to WAN.

![nat](/Assets/NAT.png)
![nat](/Assets/NAT_1.png)
3. In **POLICIES** -> **Security**, **Add** a new security rule. Point lan to WAN.  

![security_rule](/Assets/Security_rule_1.png)
![security_rule]/Assets/Security_rule_2.png)
![security_rule](/Assets/Security_rule_3.png)
![security_rule](/Assets/Security_rule_4.png)
![security_rule](/Assets/Security_rule_5.png)

4. DHCP configuration
![DHCP](Assets/DHCP.png)

<g>// Check 'Ping IP when allocating new one' may lead to unidentified network.  Why?

<br></br>

# Partner License Active #

Go to [**Partner Relationship Management**](https://paloaltonetworkssupport.force.com/NextWavePartnerProgram/s/eval-request/a3N4u000006BV8wEAG/e342127) 
portal 

Click **Evaluations** -> **Evaluation Inventory**, add the hardware you want to active licenses for
![add_inventory](Assets/Add_Inventory.png) 
![inventory_example](Assets/inventory_example.png) 

<g>// you can update the inventory details through **Bulk Update**</g> 

Click **Evaluations** -> **Evaluation Request** to active the devices
![eva_request](Assets/Eval%20Request.png) 

Select **Eval Type** -> **Customer Eval**, **Opportunity** -> only one that can be selected
<g> // why not other? what is opportunity?  what is CSP account?  </g> 

Click **Add Product**, select the model of the devices/hardware, choose the number you want to active in **Quantity**
![add_product](Assets/add_product.png)
![1](Assets/new_request_1.png)
![2](Assets/new_request_2.png)
![3](Assets/new_request_3.png)

Click **Save**, return to the Request list.
![request_list](Assets/request_list.png)

Click the new request you just added, then click **Fulfillment**\
![fulfillment](Assets/Fulfillment.png)

Add the serial number of the device you wanna active.
![add_serial_number](Assets/add_serial_number.png)

Go back to [**CUSTOMER SUPPORT PORTAL**](https://support.paloaltonetworks.com/), register the device through serial number.

Click **Assets** -> **Devices**
![register](Assets/Register.png)

Back to the firewall GUI, retrieve the license.

![retrivev_license](Assets/retrieve_license.png)

<br></br>

# Firewall configure 

## [HA](https://docs.paloaltonetworks.com/pan-os/9-1/pan-os-web-interface-help/device/device-high-availability/configure-ha-settings) 

## Active/Passive ##

- go to **Network** -> **Interfaces**, config two interface as the **Interface Type** are **HA**
- Two interface: one for control link, one for data link
![HA_setting](Assets/HA/HA1_interface.png)

- Peer address: control link of HA2
- device priority: higher index = lower priority = passive
- Preemptive: allow higer priority PA back to service after revocery immediately
<g> // heartbeat backup is not required.

![HA_setting](Assets/HA/HA1_setting.png)
<g>// it seems that Control, data, and Backup link requires different interface.

<g>// suggest to use different subnet.


![HA_setting](Assets/HA/HA1_setting_2.png)

![HA_setting](Assets/HA/HA1_setting_3.png)

- go back to **Dashboard**, add HA widget
- Click **Sync to peer HA**

## HA - Active/Active - the first HA ##
- set WAN to static instead of DHCP
- enble a new HA interface as **packet forwarding**

![HA1_interface](Assets/HA/HA1_AA_interface.png)

- go to **Devices** -> **High Availability**, enable **Active/Active**
- **Device ID** represents the priority (0 as active primary, higher than 1)

![HA1_setting](Assets/HA/HA1_AA_setting1.png)

- define different interface as control, data, and packet forwarding

![HA1_setting](Assets/HA/HA1_AA_setting2.png)
![HA1_setting](Assets/HA/HA1_AA_setting3.png)

- in **Active/Active config**, add a floating ip
- recommend that the floating ips of different HA are set under different subnet
  
![HA1_setting](Assets/HA/HA1_AA_setting4.png)

## the second HA

- set the HA inerface on different PA

![HA2_interface](Assets/HA/HA2_AA_interface.png)
![HA2_setting](Assets/HA/HA2_AA_setting1.png)
![HA2_setting](Assets/HA/HA2_AA_setting2.png)
![HA2_setting](Assets/HA/HA2_AA_setting3.png)

- go back to dashboard, click sync to peer, then manually sync the version

![status](Assets/HA/HA_AA_status.png)

<br></br>

