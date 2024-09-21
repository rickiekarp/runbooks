# README #

## Creating a luks keyfile volume

# Install the "uuid" tool
sudo apt install uuid

# Create a 256 byte key file with random data (.lek = LUKS Encryption Key)
dd if=/dev/urandom bs=1 count=256 > $(uuid).lek

Example uuid: a289ded6-6acd-11ee-afa3-9ff3f60d2f53

# Copy the key file to the USB drive:
cp a289ded6-6acd-11ee-afa3-9ff3f60d2f53.lek /media/maurits/B54D-B744

# Find the encrypted volume:
sudo blkid --match-token TYPE=crypto_LUKS -o device

# Add the key file to the LUKS volume:
sudo cryptsetup luksAddKey /dev/sda3 a289ded6-6acd-11ee-afa3-9ff3f60d2f53.lek

# Edit /etc/crypttab. You should see a line like:
sda3_crypt UUID=b9570e0f-3bd3-40b0-801f-ee20ac460207 none luks,discard

Modify it to:

sda3_crypt UUID=b9570e0f-3bd3-40b0-801f-ee20ac460207 a289ded6-6acd-11ee-afa3-9ff3f60d2f53 luks,discard,keyscript=/bin/luksunlockusb


# Add a script that will search for the key on USB drives during boot:

cat << "END" > luksunlockusb
#!/bin/sh
set -e
if [ ! -e /mnt ]; then
    mkdir -p /mnt
    sleep 3
fi
for usbpartition in /dev/disk/by-id/usb-*-part1; do
    usbdevice=$(readlink -f $usbpartition)
    if mount -t vfat $usbdevice /mnt 2>/dev/null; then
        if [ -e /mnt/$CRYPTTAB_KEY.lek ]; then
            cat /mnt/$CRYPTTAB_KEY.lek
            umount $usbdevice
            exit
        fi
        umount $usbdevice
    fi
done
/lib/cryptsetup/askpass "Insert USB key and press ENTER: "
END

# Make the script executable and move it to the right location:
chmod 755 luksunlockusb
sudo mv luksunlockusb /bin/luksunlockusb

# Include the "luksunlockusb" script (and modules) in the initial ram file system using:
```
sudo update-initramfs -u
reboot
```

## Set up configuration to unlock keyring automatically

Extract the `keyring.tar.xz` config from the Password Manager `hosts` to the users home directory.