# Network Namespace with Open vSwitch

This project demonstrates how to set up a virtual switch using Open vSwitch (OVS) and configure network namespaces in Linux. It includes steps to create a virtual switch, set up two network namespaces, connect them to the switch, and assign IP addresses.

## Prerequisites

Ensure you have the following installed on your Linux system:

- Open vSwitch (OVS)
- `iproute2` package

You can install these packages using your package manager. For example, on Ubuntu, you can use:

```bash
sudo apt update
sudo apt install openvswitch-switch iproute2
```

Steps

1. Create a Virtual Switch using OVS
```bash
# Create a new OVS bridge
sudo ovs-vsctl add-br ovs-br0
```

2. Create Network Namespaces
Next, we will create two network namespaces.

```bash
# Create two network namespaces
sudo ip netns add ns1
sudo ip netns add ns2
```

3. Create Virtual Ethernet Pairs
Create virtual Ethernet pairs to connect the namespaces to the OVS bridge.

```bash
# Create veth pairs
sudo ip link add veth1 type veth peer name veth1-br
sudo ip link add veth2 type veth peer name veth2-br
```

4. Connect Veth Pairs to Namespaces and OVS
   
```bash
# Move veth1 to namespace ns1
sudo ip link set veth1 netns ns1

# Move veth2 to namespace ns2
sudo ip link set veth2 netns ns2

# Add veth1-br and veth2-br to OVS bridge
sudo ovs-vsctl add-port ovs-br0 veth1-br
sudo ovs-vsctl add-port ovs-br0 veth2-br
```
5. Assign IP Addresses and Bring Up Interfaces
Assign IP addresses to the interfaces inside the namespaces and bring them up.

```bash
# Assign IP to veth1 and bring it up inside ns1
sudo ip netns exec ns1 ip addr add 192.168.1.1/24 dev veth1
sudo ip netns exec ns1 ip link set dev veth1 up
sudo ip netns exec ns1 ip link set lo up

# Assign IP to veth2 and bring it up inside ns2
sudo ip netns exec ns2 ip addr add 192.168.1.2/24 dev veth2
sudo ip netns exec ns2 ip link set dev veth2 up
sudo ip netns exec ns2 ip link set lo up

# Bring up veth interfaces connected to OVS bridge
sudo ip link set dev veth1-br up
sudo ip link set dev veth2-br up
```

6. Test Connectivity
Test the connectivity between the two namespaces.
```bash
# Ping from ns1 to ns2
sudo ip netns exec ns1 ping 192.168.1.2
```
Conclusion

You've successfully set up a virtual switch using OVS and connected two network namespaces to it. This setup can be extended further to simulate more complex network topologies or to test network configurations in an isolated environment.
