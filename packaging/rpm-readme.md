<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [About](#about)
- [CentOS-specific](#centos-specific)
- [Non-RHEL distros and chroots/mock](#non-rhel-distros-and-chrootsmock)
- [Open Build System](#open-build-system)
- [Misc](#misc)
- [RPM package quick start:](#rpm-package-quick-start)
  - [Generating spec file templates](#generating-spec-file-templates)
  - [Sources](#sources)
  - [Building](#building)
    - [Building locally:](#building-locally)
  - [Building with mock](#building-with-mock)
  - [Changelog](#changelog)
  - [Review](#review)
- [GPG signing](#gpg-signing)
- [Links](#links)
  - [General](#general)
  - [Man pages](#man-pages)
  - [spec files](#spec-files)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# About
Info for items specific to The Linux distirbution, Fedora.

# CentOS-specific

* [centpkg (early stages)](https://wiki.centos.org/HowTos/Centpkg)

# Non-RHEL distros and chroots/mock

Mock often doesn't quick work to initalize the base chroot on non-RHEL related distros. In that case, you can make use of the [build-fedora-chroot](https://github.com/ProfessorKaos64/LibreGeek-Packaging/blob/master/setup-files/build-fedora-chroot.sh) script to create the chroot.

# Open Build System

* [Main site](https://build.opensuse.org/)
* [Tutorial](https://en.opensuse.org/openSUSE:Build_Service_Tutorial)

# Misc

* [Working with Kodi and RPM fusion](http://kodi.wiki/view/HOW-TO:Install_Kodi_on_Fedora_23_using_RPMFusion_packages#Configuring_Fedora_.2F_Installing_Dependencies)

# RPM package quick start:

## Generating spec file templates

Generating a clean spec file template:

```
rpmdev-newspec <PKG_NAME>
```

## Sources

It is good practice to download sources properly with `spectool`:
```
spectool -R -g *spec
```

`-g` - gets the sources/patches that are listed with a URL
`-R` - download into rpm's %{_sourcedir}.

## Building

### Building locally:

rpmbuild
```
rpmbuild --rebuild /tmp/SRPM_NAME
```

Fedora
```
fedpkg --dist f24 local
```

## Building with mock

* See [documents/mock/readme.md](https://github.com/ProfessorKaos64/documents/blob/master/mock/readme.md)

## Changelog

Manual:
```
* date Packager's Name <packager's_email> version-release_ver
- Summary of changes
```

Automatic:
```
rpmdev-bumpspec --comment=summary of changes --userstring=Packager's Name <packager's_email> spec file 
```

## Review

Lint the package:

Fedora
```
fedpkg --dist f24 lint
```

CentOS/RHEL
```
rpmlint -i <PACKAGE>.spec
```

Using fedora-revew:

```
rm -rf *rpm results*
fedpkg --dist f24 <PKG_NAME>
fedora-review -n <PKG_NAME>
```

Unpacking an RPM package:

```
 rpm2cpio <PACKAGE> | cpio -id 
 ```
 
 # GPG signing
 
The following commands will initiate GPG key creation and export it in a format suitable for distributing to client systems. The created key should be stored safely and backed up, and its passphrase should be known only by trusted administrators.

```
mkdir -p ~/.gnupg
gpg --gen-key
gpg --list-keys --fingerprint
gpg --export --armor "rpmbuild <rpmbuild@example.com>" > EXAMPLE-RPM-GPG-KEY
```

To import this key to the RPM database to allow RPM origin and integrity verification, the following command must be run as root on all target systems (naturally this should happen automatically during client installations):

```
rpm --import EXAMPLE-RPM-GPG-KEY
```

Once an RPM has been created it must be signed with the GPG key and uploaded to a correct channel:

```
rpm --resign package.rpm
rhnpush --server=http[s]://satellite.server/APP package.rpm --channel=custom-channel-name
```

The following commands will verify an RPM package located in the current directory:

```
rpm –qip pakcage.rpm
rpm -K package.rpm
``` 
See: [Red Hat Satellite Documentation](https://access.redhat.com/documentation/en-US/Red_Hat_Network_Satellite/5.3/html/Deployment_Guide/satops-rpm-building.html)

# Links

## General

* [How to create an RPM pacakge (fedora)](https://fedoraproject.org/wiki/How_to_create_an_RPM_package)
* [How to create an RPM package (tldp)](http://www.tldp.org/HOWTO/RPM-HOWTO/build.html)
* [Fedora maintainers resources](https://fedoraproject.org/wiki/Category:Package_Maintainers?rd=PackageMaintainers#Procedures.2C_Policies_and_Guides)
* [Man page rpmbuild](http://www.rpm.org/max-rpm-snapshot/rpmbuild.8.html)
* [OBS (Open Build System)](https://build.opensuse.org/)
* [Packager's Guide](https://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/Packagers_Guide/)
* [RPM Fusion](http://rpmfusion.org/)
* [Setup packaging environment (CentOS)](https://wiki.centos.org/HowTos/SetupRpmBuildEnvironment)

## Man pages

* [spectool](http://www.unix.com/man-page/centos/1/spectool/)

## spec files

 * [Macros (Fedora docs)](https://fedoraproject.org/wiki/Packaging:RPMMacros?rd=Packaging/RPMMacros)
 * [Macros (rpm.org)](http://www.rpm.org/wiki/PackagerDocs/Macros)
 * [Spec file macros](http://www.rpm.org/max-rpm/s1-rpm-specref-macros.html)
 * [Spec file tags (rpm.org)](http://rpm.org/api/4.4.2.2/specfile.html)
 * [Sources](https://fedoraproject.org/wiki/Packaging:SourceURL?rd=Packaging/SourceURL)
