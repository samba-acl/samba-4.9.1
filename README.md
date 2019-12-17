# samba-4.9.1

This Repository holds NFS4ACL_XATTR Plugin changes on Samba 4.9.1 which can be summarized as:

* Implemented another set of XDR structure and APIs which are compliant with NFSv4 ACL Format prescribed in RFC 7530. These encodings can be used with encoding=xdr40
* Changed default XDR attribute name to "system.nfs4_acl" which works with native NFSv4 and nfs-ganesha.
* Also added validate_mode flag which is false by default for xdr40. This skips any RWX mode checks for files and folders during validate blob functionality
* All owners with Special IDs along with OWNER@, GROUP@, EVERYONE@ are encoded/decoded now.

The patch file can be used in following steps:

Download Samba 4.9.1 with:
		
		wget https://download.samba.org/pub/samba/stable/samba-4.9.1.tar.gz  

untar with:
		
		gunzip samba-4.9.1.tar.gz 
		tar -xf samba-4.9.1.tar

Apply the patch with:

		cd samba-4.9.1
  		patch -p4 < ../nfs4acl_xattr.patch

Configure and Build samba using steps listed on:

		https://wiki.samba.org/index.php/Build_Samba_from_Source

The plugin will be available at bin/default/modules/vfs/nfs4acl_xattr.so
Place the nfs4acl_xattr.so in modules/vfs/ directory of your Samba. You can obtain modules/ directory using

		<path to your smbd> -b | grep MODULESDIR

While creating the share, use following options
 
		vfs objects = nfs4acl_xattr

		nfs4acl_xattr:xattr_name = system.nfs4_acl

		nfs4acl_xattr:version = 40

		nfs4acl_xattr:encoding = xdr40
	
Reload the configuration if you happen to change any of the share parameters

		smbcontrol all reload-config

* nfs4acl_xattr.so compiled with Samba-4.9.1 is also uploaded, which can be used with your currently running Samba 4.9.1 by following steps 6 to 8.
