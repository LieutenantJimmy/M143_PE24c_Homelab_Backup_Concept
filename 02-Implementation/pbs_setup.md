**[⬅ Back to README](/README.md)**

# Implementierung – Proxmox Backup Server (PBS) on Hyper‑V

![alt text](image-1.png)

Okay so we've got our Proxmox cluster consisting of two mac-mini nodes.

togehter, they run our service stack, which is for the most part little services i use at home for convenience, random apps i'm looking into or experimenting with or some self-hosted service i provide access too via my website giotech.ch

Today we'll focus on performing a proper backup & restore cycle with a singular VM using Proxmox, purely to test it's functionality.

As of right now we've already calculated RTO and RPO targets.. already implemented a backup scheme following the 7-1-1 principle.. so here's the first test.

![alt text](image-3.png)

in terms of tools we're relying on a 100% Proxmox ecosystem for now, we'll get to explore more tools later on when backing up Hyper-V VMs.

For now we'll be making a backup of service-netvps-1 with but before we do, we'll do a modification that we'll reverse shortly after.

![alt text](image-4.png)

within one of my services in netvps-1 (uses CasaOS & Docker so it runs multiple services within it) i changed the name of a item to this. Then shut down the machine, and performed a backup using Proxmox'x built in backup feature, though i backed up onto the PBS on Hyper-V.

![alt text](image-5.png)

The way i set this up, it not only has TLS (Transit Encryption) but also stores the files in an Encrypted manner on PBS.

![alt text](image-7.png)

As the backup finished, i continued to restore the VM, i was required to delete the previous one to avoid duplicates from a network perspective, but pretty much worked out of the box.

![alt text](image-8.png)


Now that we tested full VM retore, we'll dedicated to the file restore thankfully proxmox also has this built in and handles it for me. Sadly i had to note that my cluster was on version 8.4.0 which had a bug which made the file restore.. unresponsive.

![alt text](image-9.png)

after a quick apt update && apt dist-upgrade we were back in the game with version 8.4.13 (i slept over some updates)


After that i setup a quick test on my personal Domain Controller:

![alt text](image-10.png)

i downloaded the image of the hamster to my pictures folder, and i intended to extract the image through the proxmox webui. or.. in other words.. recover the file.

![alt text](image-11.png)

after a quick backup.. i used the restore tool to quickly find the file within the directory i was already used to on the windows machine.

![alt text](image-12.png)


and here was the hamster..

![alt text](image-13.png)