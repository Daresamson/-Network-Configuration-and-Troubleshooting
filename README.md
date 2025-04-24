# Network Configuration and Troubleshooting

## Project 1: Network Configuration and Troubleshooting

### Objective
In this project, I learned how to configure and troubleshoot network settings in Linux. The goal was to configure a static IP address, modify routing settings, and troubleshoot network connectivity using various networking commands.

---

### Steps

![Screenshot (281)](https://github.com/user-attachments/assets/56d139bd-cdc3-422c-bdd6-0f2bea91e9ed)
![Screenshot (285)](https://github.com/user-attachments/assets/883ed82f-5e4a-497e-bdb2-e4e3f47345e9)



#### 1. **Set a Static IP Address**
The first step was to configure a static IP address for the system. I edited the network configuration file as follows:

- Open the configuration file with the command:
  ```bash
  sudo nano /etc/network/interfaces
  ```

- I added the following lines to set the static IP (adjusted to my network):
  ```bash
  auto eth0
  iface eth0 inet static
      address 192.168.1.100
      netmask 255.255.255.0
      gateway 192.168.1.1
  ```

- I saved and closed the file.

#### 2. **Apply the Configuration**
After modifying the configuration file, I applied the changes with the following command:

```bash
sudo systemctl restart networking
```

However, I encountered an issue because the `networking.service` unit was not found. This led me to check the system for netplan configurations.

![Screenshot (288)](https://github.com/user-attachments/assets/18fe434b-3b1a-43a2-9791-c6282d65af4f)
![Screenshot (288)](https://github.com/user-attachments/assets/c46fe41d-1fc9-4811-9576-c9e2dda1ac93)
![Screenshot (290)](https://github.com/user-attachments/assets/2a05bfd7-443b-4074-8a00-d9b7cba86992)
![Screenshot (291)](https://github.com/user-attachments/assets/a8488228-8da4-4aae-b612-95f459700a20)

#### 3. **Troubleshooting Network Configuration**
Upon reviewing the network configuration files, I found that my system was using **netplan** instead of the older `interfaces` file.

- I checked the existing netplan configuration files in `/etc/netplan/` using the command:
  ```bash
  ls /etc/netplan/
  ```

- I edited the **00-installer-config.yaml** file:
  ```bash
  sudo nano /etc/netplan/00-installer-config.yaml
  ```

- I modified the IP configuration to ensure the correct static settings were applied.

- After saving the changes, I applied the configuration using:
  ```bash
  sudo netplan apply
  ```
![Screenshot (291)](https://github.com/user-attachments/assets/05a20f9f-508e-428e-9951-52cc147160ef)

#### 4. **Verify the IP Address**
To verify the configuration, I used the following command to check the IP address of the interface `enX0`:
```bash
ip addr show enX0
```

This confirmed that the system had correctly obtained the static IP address.

#### 5. **Routing Configuration**
I modified the routing table to ensure proper routing for my network. I checked the routes using:
```bash
ip route show
```

I noticed that there were conflicting default routes. This required me to delete a redundant route:
```bash
sudo ip route del 172.31.80.1 dev enX0
```

I updated the routes to reflect the correct configuration, ensuring no conflicts between interfaces.
![Screenshot (291)](https://github.com/user-attachments/assets/8cf9640f-1c04-4f6b-bdd1-ec345a439a0e)
![Screenshot (292)](https://github.com/user-attachments/assets/c77f1d4c-1a34-4fb0-a2ce-8380b0d9aec2)
![Screenshot (293)](https://github.com/user-attachments/assets/b6f41a2a-0479-47d0-9c53-c58ed1f2e399)

#### 6. **Testing Network Connectivity**
To verify network connectivity, I performed two tests:

- **Ping the gateway (172.31.80.1):**
  ```bash
  ping 172.31.80.1
  ```

  This showed that the connection to the gateway was successful.

- **Ping an external server (Google):**
  ```bash
  ping google.com
  ```

  The ping to Google returned successful results, confirming that the system had internet access.

---

### Troubleshooting Observations

- **Deprecation Warning for `gateway4`:** I encountered warnings indicating that the `gateway4` configuration was deprecated. The recommended approach was to define default routes instead of `gateway4`. I updated my configuration accordingly.
  
- **Conflicting Default Routes:** I had to manually delete conflicting routes, which had been automatically configured. After removing the redundant route, the networking was stable.

---

### Conclusion

Through this project, I successfully configured a static IP address, adjusted network routes, and resolved network configuration conflicts. I also became familiar with the netplan tool, which is now used by default in many modern Linux distributions for network management.

This project helped me develop practical skills in troubleshooting network issues in a Linux environment.

---
