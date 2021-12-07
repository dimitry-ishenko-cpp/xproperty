# Debian Packaging for xproperty

This is Debian packaging for the
[xproperty](https://github.com/jupyter-xeus/xproperty) repository.

xproperty along with other supporting packages can be conveniently installed
from
[ppa:ppa-verse/xeus](https://launchpad.net/~ppa-verse/+archive/ubuntu/xeus).

Building instructions:

```bash
p=xproperty b=debian # or focal or impish

# clone this repo
git clone -b ${b} --depth 1 https://github.com/dimitry-ishenko-cpp/${p}.git
r=$(cd ${p} && git log -n1 --oneline --no-decorate `git describe --tags --abbrev=0` | cut -d/ -f2)
v=${r%-*}
v=${v#*:}

# download upstream tarball
wget https://github.com/jupyter-xeus/xproperty/archive/refs/tags/${v}.tar.gz \
    -O ${p}_${v}.orig.tar.gz

# build source package
dpkg-source -b ${p}

# build .deb package locally...
sudo pbuilder build ${p}_${r}.dsc

# ...or generate .changes file and upload it to a PPA
# NB: change -sa to -sd when upgrading
(cd ${p} && dpkg-genchanges -S -sa -O../${p}_${r}_amd64.changes)

debsign ${p}_${r}_amd64.changes
dput ppa:... ${p}_${r}_amd64.changes

# share and enjoy
```
