# How to decrypt LUKS1 with key in memory


# How to decrypt LUKS1 with key in memory
---

LUKS is a standard for Linux disk encryption, and exists in two version. The latest version LUKS2 can be strong and a trusted solution when paired with strong password and algorithm, whereas LUKS1 suffers from a severe **vulnerability**. The master key, used to encrypt and decrypt the whole disk, **can be retrieved in the memory** of the running machine.
This article will show you how to ___recover that key___, and then ___decrypt the disk___.

{{< admonition warning "Educational purpose" >}}
This is for educational purpose only, do not try this on systems that you do now own, or have been allowed to do so.
{{< /admonition >}}

### {{< fa-icon  solid list >}} Requirements

This can only be done if you have access to :

- The encrypted disk (.img, .vmdk, .qemu, physical one, etc)
- The memory of the decrypted machine.
- A Linux machine.

The disk can be a raw type, as a virtual or a physical one.

The memory can be extracted with tools like [avml](https://github.com/microsoft/avml), or from a snapshot (eg. .vmem)

The Linux machine will be the support for decryption and then access the data.

### {{< fa-icon solid key >}} Key recovering

LUKS1 uses **AES algorithm** for encrypting data. The vulnerability of LUKS1 is that this key is kept in plain text forever in the memory, while the machine is running and decrypted.
The idea here, is to find the key in the memory. 

I personally used a great tool called **[findaes](https://sourceforge.net/projects/findaes/)**, but others exist with the relative same name.

We just need to use in our memory/snapshot like this :

```pwsh
./findaes.exe path/to/memory.vmem
```

Result looks like this :

{{< image src="/LUKS/aes.png" height="50%" caption="Findaes result" title="Findaes result" alt="Findaes result" >}}

```
78 ac 5a e3 9b b5 d5 96 fe 69 6f ee 2a 13 17 12 49 69 ce 92 46 56 53 45 02 fa fb 6e 16 43 a0 c9
ff e7 b8 81 a3 31 40 05 6a 04 0e 9f 93 ed ba e8 56 b8 b5 b9 18 de 4e bb d4 1d 1e 94 ce d8 66 93
```
Save your result somewhere.

{{< admonition question "Why two keys ?" >}}
Here we have two AES keys found. We'll get back to it later. 
{{< /admonition >}}

### {{< fa-icon solid lock-open >}} Decrypt 

Next step is to attach and mount the disk. I personally used the .vmdk version in a Kali VM.

{{< admonition tip "Raw disk" >}}
If you use a raw disk (.raw, .img or equivalent) you can install ``kpartx`` and run this command :

````shell
$ sudo kpartx -av <disk-file>
````
{{< /admonition >}}

You should then see a result equivalent for the next screenshot, but in loop device.
Using the `lsblk` command, you should see your disk, and some lvm partition : 

{{< image src="/LUKS/lvm.png" height="50%" caption="Mounted disk" title="Mounted disk" alt="Mounted disk" >}}

As you can see, this machine is a centos, but whatever.

We'll then use the `cryptsetup` command to gather information and decrypt the disk. This is the common and standard way to encrypt a isk with LUKS.

We then need the key in binary format. I used `xxd` for that part.
First paste them in a txt file 

{{< image src="/LUKS/keytxt.png" height="50%" caption="Paste keys" title="Paste keys" alt="Paste keys" >}}

Using ``xxd -r -p file.txt > file.bin`` we can convert the text file into binary. We'll use this format for decryption.

{{< image src="/LUKS/keybin.png" height="50%" caption="Convert keys in binary" title="Convert keys in binary" alt="Convert keys in binary" >}}

We now can open our LUKS encrypted disk with this command :

````shell
$ sudo cryptsetup luksOpen /dev/my-device device_name --master-key-file key.bin
````

{{< admonition tip "device_name" >}}
I chose ``LUKS`` as my device_name, this will be available at `/dev/mapper/LUKS`
{{< /admonition >}}

{{< image src="/LUKS/failed.png" height="50%" caption="Trying to open disk" title="Trying to open disk" alt="Trying to open disk" >}}

Either it works fine for you, or this prompts you some kind a key size error.

It asks us for a **64 bytes** key. Or, if we check, both our keys are **32 bytes** keys.

{{< image src="/LUKS/size.png" height="50%" caption="32 bytes key" title="32 bytes key" alt="32 bytes key" >}}


What happens here, is that LUKS uses a special cipher mode of AES. We can dump the cipher info with the following command : 

{{< image src="/LUKS/xts.png" height="50%" caption="XTS Cipher mode" title="XTS Cipher mode" alt="XTS Cipher mode" >}}

The mode used here is **XTS**, not the common GCM, CBC or ECB. It is a stronger algorithm than the other, because it needs **2 keys**.

{{< image src="/LUKS/scheme.png" height="50%" caption="Because an image is worth a thousand words" title="Because an image is worth a thousand words" alt="Because an image is worth a thousand words" >}}

I will not go deep in the theory, but that explains the fact that the first key is following the second one in memory.

The master key file does need both keys. You have to **concatenate both your keys**.
By doing so, you can try your news keys.  

{{< image src="/LUKS/decrypt.png" height="50%" caption="Decrypt with new key" title="Decrypt with new key" alt="Decrypt with new key" >}}

The second key appears to have worked correctly. Let's mount our device and check the content of our mounting point `mntLUKS` :

Let's create a folder in your common location. This will be our mounting point.
I called it `mntLUKS`.

{{< image src="/LUKS/mount.png" height="50%" caption="Disk mounted" title="Disk mounted" alt="Disk mounted" >}}

{{< admonition success "Done" >}}
We did recover our LUKS1 encrypted disk !
{{< /admonition >}}

---

#### Tags
``LUKS``  ``LUKS1``  ``LUKS decrypt``  ``LUKS decipher``  ``LUKS open``  ``LUKS key memory``  ``LUKS memory``  ``LUKS decrypt with memory``  ``LUKS open with memory``  ``recover LUKS key``  ``memory``


