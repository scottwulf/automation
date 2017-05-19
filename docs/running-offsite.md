# running mkcloud outside Nuremberg

This file describes how remote people can improve their experience with mkcloud

Currently, mkcloud works best out-of-the-box when you have a good network connection to Nuremberg and you can reach `clouddata.nue.suse.com.` and `download.nue.suse.com.` - that is you are in the MicroFocus corporate network.

However, it can be made to work even outside of SUSE with no access to any company network.

For that you want to have an SMT setup to mirror the relevant repositories,
e.g. SLES-12-SP2, SUSE-OpenStack-Cloud-7, SLE12-SP2-HA, SUSE-Enterprise-Storage-4 (Pool and Updates repo for each of them)

And then you adapt and set

```
export smturl=http://smt-scc.nue.suse.com/repo
```

There are some other parts fetched over NFS, rsync and HTTP from `clouddata.nue.suse.com.`
You can override this host with

```
export reposerver=yourmirror.yourdomain
```

As a SUSE employee you can get files mirrored using

```
rsync -a clouddata.nue.suse.com::cloud/repos/x86_64/... TARGET_DIRECTORY/
rsync -a clouddata.nue.suse.com::cloud/images/x86_64/... TARGET_DIRECTORY/
```

You can browse that directory structure at
http://clouddata.nue.suse.com/

