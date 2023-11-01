We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 6100 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:



Install iptables and all its dependencies on each app host.

Block incoming port 6100 on all apps for everyone except for LBR host.

Make sure the rules remain, even after system reboot.





--------

To fulfill the security requirements for your Nautilus infrastructure in Stratos DC and block incoming port 6100 while allowing access only from the LBR host, you can follow the steps below:

1. Connect to each app host in your infrastructure.

2. Install iptables and its dependencies. Depending on your operating system, you can use the appropriate package manager. For example, on CentOS/RHEL, you can use the following command:
   
   ```
   sudo yum install iptables
   ```

3. Configure iptables rules to block incoming traffic on port 6100, except for the LBR host. Run the following commands on each app host:

   ```
   sudo iptables -A INPUT -p tcp --dport 6100 -s <LBR_IP> -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 6100 -j DROP
   ```

   Replaced `<LBR_IP>` with the IP address of your LBR host.

4. Save the iptables rules so that they persist after system reboots. Execute the following command:

   ```
   sudo iptables-save > /etc/sysconfig/iptables

   ```

   This command saves the current iptables rules to a file that will be loaded automatically upon system reboot.

By following these steps, I have installed iptables, block incoming traffic on port 6100 for all hosts except the LBR, and ensured that the rules are maintained even after system reboots. This will add a security layer to the infrastructure by restricting access to the Apache port.