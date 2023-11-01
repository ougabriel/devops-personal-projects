The Nautilus system admins team recently deployed a web UI application for their backup utility running on the Nautilus backup server in Stratos Datacenter. The application is running on port 5000. They have firewalld installed on that server. The requirements that have come up include the following:



Open all incoming connection on 5000/tcp port. Zone should be public.


----------------

To open incoming connections on port 5000/tcp for a web UI application running on the Nautilus backup server using firewalld with the "public" zone, you can follow these steps:

1. **Check Firewalld Status:**

   Verify that firewalld is running on the server:

   ```bash
   sudo systemctl status firewalld
   ```

   If it's not running, you can start it with:

   ```bash
   sudo systemctl start firewalld
   ```

2. **Add Firewall Rule:**

   Add a rule to allow incoming connections on port 5000/tcp in the "public" zone:

   ```bash
   sudo firewall-cmd --zone=public --add-port=5000/tcp --permanent
   ```

   - `--zone=public`: Specifies the zone where the rule will be applied.
   - `--add-port=5000/tcp`: Adds a rule to allow incoming connections on port 5000 using TCP protocol.
   - `--permanent`: Makes the rule permanent so that it will be applied even after firewall reloads.

3. **Reload Firewalld:**

   After adding the rule, reload firewalld to apply the changes:

   ```bash
   sudo firewall-cmd --reload
   ```

4. **Verify Rule:**

   Confirm that the rule has been added:

   ```bash
   sudo firewall-cmd --list-all
   ```

   Check the output to ensure that `ports: 5000/tcp` is listed under the "public" zone.

