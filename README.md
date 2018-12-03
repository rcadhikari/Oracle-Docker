### Build the docker image for Oracle in local.
1. Follow the instruction https://sqlmaria.com/2017/04/27/oracle-database-12c-now-available-on-docker/
1. And, finally run the following command on docker enabled terminal:
    ```
    $ cd ~/apps/oracle/docker-images-master/OracleDatabase/SingleInstance/dockerfiles
    $ ./buildDockerImage.sh -v 12.2.0.1 -e

    docker run --name oracle -p 1521:1521 -p 5500:5500
    -v /Users/rc.adhikari/apps/myapp/oradata:/opt/oracle/oradata
    oracle/database:12.2.0.1-ee

    ./buildDockerImage.sh -v 12.2.0.1 -e

    1. /opt/oracle/oraInventory/orainstRoot.sh
    2. /opt/oracle/product/12.2.0.1/dbhome_1/root.sh

    docker run --name oracle-ee -p 1521:1521 -v /Users/rc.adhikari/apps/myapp/oradata:/opt/oracle/oradata oracle/database:12.2.0.1-ee

    docker run --name oracle-ee -p 1521:1521 -v /Users/rc.adhikari/apps/myapp/oradata:/opt/oracle/oradata oracle/database:12.2.0.1-ee
    ```

### To resize the virtual box hard drive size.
1. Go to the virtual machine location:
    ```
    $ cd ~/.docker/machine/machines/<machine_name>
    e.g. ~/.docker/machine/machines/myapp
    ```
And, you should be able to see `disk.vmdk` file on the list.

2. To check the size:
    ```
    $ VBoxManage showhdinfo disk.vmdk

    UUID:           bff17e4f-9f0a-40df-9b2c-3e1c18da1d8c
    Parent UUID:    base
    State:          created
    Type:           normal (base)
    Location:       /Users/rc.adhikari/.docker/machine/machines/myapp/disk.vmdk
    Storage format: vdi
    Format variant: dynamic default
    Capacity:       20000 MBytes
    Size on disk:   17212 MBytes
    Encryption:     disabled
    ```

3. Resize the disk as you need.
    ```
    VBoxManage clonehd disk.vmdk disk_resized.vdi --format VDI --variant Standard
    VBoxManage modifyhd disk_resized.vdi --resize 51200
    VBoxManage clonehd disk_resized.vdi resized_disk.vmdk --format vmdk
    ```

4. To check for resized disk space:
    ```
    $ vboxmanage showhdinfo resized_disk.vmdk
    UUID:           a12a3eac-d41f-4e29-84bb-3964a143edbb
    Parent UUID:    base
    State:          created
    Type:           normal (base)
    Location:       /Users/rc.adhikari/.docker/machine/machines/myapp/resized_disk.vmdk
    Storage format: vmdk
    Format variant: dynamic default
    Capacity:       51200 MBytes
    Size on disk:   17034 MBytes
    Encryption:     disabled
    ```

Sources:
1. http://osxdaily.com/2015/04/07/how-to-resize-a-virtualbox-vdi-or-vhd-file-on-mac-os-x/
2. https://stackoverflow.com/questions/11659005/how-to-resize-a-virtualbox-vmdk-file
