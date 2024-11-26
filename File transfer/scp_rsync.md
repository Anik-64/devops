### SCP (Scure Copy Protocol)

SCP is a straightforward, secure method for transferring files between hosts on a network. It leverages SSH (Secure Shell) for data transfer, ensuring that the files are encrypted during transit. SCP is widely used due to its simplicity and security.

- Features:
 - Uses SSH for encryption.
 - Simple and easy to use.
 - Good for quick, one-time file transfers.
- Limitations:
 - No incremental file transfer: If you re-run scp for a large file, it retransfers the entire file, even if parts of it are already present on the destination.
 - Less control over synchronization.


1. Linux server 1 to server 2

   Send local file to remote 
    ```
    scp file1.txt anik@192.168.40.128:/home/anik/Documents/
    ```

   ``
   file1.txt -> Local server file, anik@192.168.40.128 -> username@ip (remote server), /home/anik/Documents/ -> Directory for remote server where the file1.txt is copied
   ``
   
   Receive remote file 
   ```
   scp anik@192.168.40.128:/home/anik/Documents/remote_file.txt /tmp/
   ```

2. Windows to linnux server

   Send local file to remote
    ```
    scp -v C:\Users\HP\Documents\local_windows_file.pdf anik@192.168.40.128:/home/anik/Documents/
    ```

   Receive remote file
    ```
    scp -v anik@192.168.40.128:/home/anik/Documents/remote_ubuntu.txt C:\Users\HP\Documents\
    ```

###

### RSYNC (Remote Synchronization)

rsync is a versatile tool for efficiently transferring and synchronizing files between two locations. Unlike SCP, rsync only transfers the differences between the source and the destination, making it highly efficient, especially for large datasets.

- Features:
 - Incremental transfers: Only changes in files are transferred, saving bandwidth and time.
 - Can preserve file attributes (permissions, ownership, timestamps).
 - Compression option (-z) reduces the amount of data sent over the network.
 - Extensive options for synchronization (--delete to remove files from the destination not present in the source).
- Limitations:
 - Slightly more complex syntax compared to scp.
 - Requires rsync to be installed on both the source and destination systems.


``rsync [options] source user@remote_host:/destination_path``

- ``-a`` or --archive for archiving files
- ``-v`` or --verbose for verbose output
- ``-z`` or --compress for compression
- ``--progress`` to show progress during trransfer

1. Linux server 1 to server 2

    ```
    rsync file.txt anik@192.168.40.128:/home/anik/Documents/
    ```

   It will comprase the file so that file size will be minimized and file transfer will fast

    ```
    rsync -vz file.txt anik@192.168.40.128:/home/anik/Documents/
    ```
   
   It will show progress

    ```
    rsync -vz --progress file.txt anik@192.168.40.128:/home/anik/Documents/
    ```

   Create a backup file in the same remote location

    ```
    rsync -vzb --progress file.txt anik@192.168.40.128:/home/anik/Documents/
    ```
   
   Upper command will create a backup file but it has disadvantages. When you will make some changes in that file locally then again run the upper command it will replace the backup file in remote, so you cann't track what is changed. Below I am providing a special command that helps you on inductry to track backup file.
   First create a backup directory in your remote server then assign the directory in the ``--backup-dir``. When you run the command it will create a backup file every time with file name format date and time. This will help to find out only which is changed.

    ```
    rsync -vzb --progress --backup-dir=/tmp/rsync/archive/$(date +%Y%m%d-%H%M%S) file.txt anik@192.168.40.128:/home/anik/Documents/
    ```


### SCP vs RSYNC