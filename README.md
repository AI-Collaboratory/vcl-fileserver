# VCL Datasets Fileserver

## Overview

This packer build creates an Amazon machine image for an NFS fileserver. It
configures the server with a set of EBS-backed volumes that are based on snapshots.
The snapshots are each mounted under /mnt and shared over NFS. The datasets are
referenced in the vcl group_vars, not included in this repository. The snapshots
are mapped to device names in the template.json file.

## Building a new image

To build a new image, you must have installed Packer. You do not need Ansible on
your local system, as it will only running on the Amazon machine. You will need
the folder referenced in template.json, containing the hosts files, AWS certificate
and group_vars. This information is not included in the git repository.

The command for building an image is:

    $ packer build template.json

## Creating the Server Instance

The two key elements to creating the server are the IP address and the security group.
NFS requires 111 and 2049 ports to be open to both UDP and TCP. There is an existing
security group "NFS server" which allows this communication. The IP address for the
fileserver is an elastic IP that is reserved. This is a private IP that must be
associated with the server. That IP is also recorded in the vcl group_vars.
