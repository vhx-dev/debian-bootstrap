debootstrap --variant=minbase --arch amd64 --include=nano bullseye /mnt http://deb.debian.org/debian/

genfstab -U /mnt/ >> /mnt/etc/fstab

cp /etc/resolv.conf /mnt/etc/resolv.conf

nano /mnt/etc/apt/apt.conf.d/02userconf

APT::Sandbox::User root;

APT::Install-Recommends "0";
APT::Install-Suggests "0";

DPkg::Post-Invoke {"rm -rf /usr/share/locale || true";};
DPkg::Post-Invoke {"rm -rf /usr/share/man || true";};
DPkg::Post-Invoke {"rm -rf /usr/share/help || true";};
DPkg::Post-Invoke {"rm -rf /usr/share/doc || true";};
DPkg::Post-Invoke {"rm -rf /usr/share/info || true";};
DPkg::Post-Invoke {"rm -rf /usr/share/i18n || true";};

Acquire::Languages "none";

nano /mnt/etc/apt/source.list.d/debian.sources

Types: deb
URIs: http://deb.debian.org/debian
Suites: bullseye bullseye-updates
Components: main contrib non-free
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Types: deb
URIs: http://deb.debian.org/debian-security
Suites: bullseye-security
Components: main contrib non-free
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg


arch-chroot /mnt

apt update
