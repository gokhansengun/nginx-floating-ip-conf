### About

An example configuration to run two Nginx web servers on two separate VMs in a highly available manner.

Clients reach web servers over a Floating IP which is assigned dynamically to one of the boxes by [keepalived](https://github.com/acassen/keepalived).

**Note:** This is actually an HA setup for Load Balancers but Nginx is used as a web server in order not to complicate the example.

### Steps to Run 

1. Create the VMs using Vagrant

    ```
    $ vagrant up lb-01 lb-02
    ```

2. Open a browser and navigate to the Floating IP `172.29.44.200`. You should see a web page indicating the master Nginx box. Master should be `LB #1` as it has high priority when both of them are up.

3. In order to test the HA setup, halt the first box `lb-01` using below command

    ```
    $ vagrant halt lb-01
    ```

4. After the first box is shut down, navigate to the Floating IP `172.29.44.200` again, now you should see `LB #2` is active and serving the page.

5. As a last test, get the first box `lb-01` up and running again to see if it is taking the mastership back. Use below command.

    ```
    $ vagrant up lb-01
    ```

6. Navigating to the Floating IP should now prove that the `LB #1` took the mastership.

