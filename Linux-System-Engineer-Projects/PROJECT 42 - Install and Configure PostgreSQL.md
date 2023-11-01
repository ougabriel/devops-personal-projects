**INSTALL AND CONFIGURE POSTGRESQL**

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

a. Install and configure PostgreSQL database on Nautilus database server.

b. Create a database user kodekloud_top and set its password to Rc5C9EyvbU.

c. Create a database kodekloud_db1 and grant full permissions to user kodekloud_top on this database.

d. Make appropriate settings to allow all local clients (local socket connections) to connect to the kodekloud_db1 database through kodekloud_top user using md5 method (Please do not try to encrypt password with md5sum).

e. At the end its good to test the db connection using these new credentials from root user or server's sudo user.#


**1. Install PostgreSQL:**
   ```
   sudo yum install postgresql-server
   ```

**2. Initialize the database:**
   ```
   sudo postgresql-setup initdb
   ```

**3. Start the PostgreSQL service:**
   ```
   sudo systemctl start postgresql
   ```

**4. Enable the PostgreSQL service to start on boot:**
   ```
   sudo systemctl enable postgresql
   ```

**5. Switch to the PostgreSQL user:**
   ```
   sudo -i -u postgres
   ```

**6. Set the password for the "postgres" user:**
   ```
   psql -c "ALTER USER postgres WITH PASSWORD 'Rc5C9EyvbU';"
   ```

**7. Create a new database user:**
   ```
   createuser kodekloud_top --createdb --pwprompt
   ```

   Enter the password for the user when prompted.

**8. Create a new database:**
   ```
   createdb kodekloud_db1 -O kodekloud_top
   ```

**9. Exit the PostgreSQL user shell:**
   ```
   exit
   ```

**10. Edit the PostgreSQL configuration file to allow local connections using md5 authentication:**
    ```
    sudo vi /var/lib/pgsql/data/pg_hba.conf
    ```

    Find the line that starts with `local` and ends with `peer` or `ident`, and change it to:
    ```
    local   all             all                                     md5
    ```

    Save the file and exit.

**11. Restart the PostgreSQL service:**
    ```
    sudo systemctl restart postgresql
    ```

**12. Test the database connection:**
    ```
    psql -U kodekloud_top -d kodekloud_db1 -h localhost
    ```

    Entered the password for the user when prompted. We should now be able to connect to the database.

