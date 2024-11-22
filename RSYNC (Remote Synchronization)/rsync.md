### rsync

<ins>Feature:</ins> Optimize file transfers by sending only the changes made and ensure that file ``Permissions``, ``Ownership``, and ``Timestamps`` are preserved.

``rsync [options] source destination``

- ``-a`` or --archive for archiving files
- ``-v`` or --verbose for verbose output
- ``-z`` or --compress for compression
- ``--progress`` to show progress during trransfer