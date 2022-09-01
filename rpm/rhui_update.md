Updating RHUI keys
---

Extensive guide to updating your RHUI keys. 

Some of the advice in here is less than sound. YMMV

```bash
# Disable GPG signing
# This is (likely) controversial, but from the imaging to
# the moment of instance creation your GPG key may expire
# or be rotated.
#
# Ideally you should have a robust key model to handle this
# maybe your cloud provider does (or doesn't) this method
# works no matter what, hence it is documented here.
sed -i'' 's/\(repo_gpgpcheck=\)1/\10/g' /etc/yum.repos.d/*.repo
# Destroy your yum cache
sed -i'' 's/\(keep_cache\s*=\s*\)1/\10/g' /etc/yum.conf
# Flush any stagnant metadata
yum clean all || true
# Update only with your cloud providers repos (only use 1 of these)
yum update -y --skip-broken --disablerepo='*' --enablerepo='*google*'
yum update -y --skip-broken --disablerepo='*' --enablerepo='*aws*'
yum update -y --skip-broken --disablerepo='*' --enablerepo='*azure*'
# Flush anything that maybe got cached.
yum clean all || true
# Finally update all packages
yum update -y
```
