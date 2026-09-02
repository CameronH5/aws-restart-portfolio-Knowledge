# Understanding CIDR Blocks in AWS

In **AWS**, a **CIDR block** is how you reserve a chunk of the cloud to build your own private playground, called a **VPC (Virtual Private Cloud)**. 

Think of AWS as a **giant empty continent**. When you start a project, you use a CIDR block to buy a piece of land and put a fence around it.

---

## 🗺️ The AWS Neighborhood Rules

When you build your playground in AWS, you have to follow a few specific rules:

* **The Big Sandbox (VPC CIDR):** You pick a giant block for your whole project. The most common one you will see is `10.0.0.0/16`. This gives you a massive plot of land with **65,536 addresses** to play with.
* **The Smaller Playgrounds (Subnets):** You can't just throw all your toys in one massive pile. You chop your big land into smaller yards called **Subnets** (like `10.0.1.0/24`). You might have one yard for your public toys (like a website) and a locked backyard for your secret (*hidden*) toys (like a database).

---

## 🤫 The AWS Tax (The 5 Missing Houses)

In a normal neighborhood, a `/24` block gives you 256 houses. But **AWS keeps 5 addresses for itself** in every single subnet to run the neighborhood infrastructure! 

If you buy a block with 256 houses, AWS takes these specific ones:

| IP Address | Role | Description |
| :--- | :--- | :--- |
| `10.0.1.0` | **Network Address** | The neighborhood base. |
| `10.0.1.1` | **VPC Router** | The neighborhood gatekeeper. |
| `10.0.1.2` | **DNS Server** | The neighborhood phonebook. |
| `10.0.1.3` | **AWS Reserved** | Reserved for future AWS space updates. |
| `10.0.1.255` | **Broadcast Address** | The neighborhood loudspeaker. |

Because of this AWS tax, you only get **251 houses** (IP addresses) to actually use for your computers!

import ipaddress

def calculate_cidr(cidr_string):
    try:
        network = ipaddress.ip_network(cidr_string, strict=False)
        total_ips = network.num_addresses
        # Handle /31 and /32 edge cases for usable hosts
        if network.prefixlen == 32:
            usable_ips = 1
        elif network.prefixlen == 31:
            usable_ips = 2
        else:
            usable_ips = total_ips - 2
            
        print(f"CIDR: {network}")
        print(f"Netmask: {network.netmask}")
        print(f"Total Addresses: {total_ips:,}")
        print(f"Usable Hosts: {usable_ips:,}")
    except ValueError as e:
        print(f"Invalid CIDR: {e}")

# Example usage:
calculate_cidr("192.168.1.0/24")
