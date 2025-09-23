**[⬅ Back to README](/README.md)**

So for hyper-v i had a massive issue to solve, Proxmox solved much of the problems i had to find an answer to out of the box, thinks like incremental backups.. optimizing storage efficiency.. or even whole functions such as backup and restore and the file recover function. Hyper-V, lacks all of that out of the box.

After a prompt of research, i was recommended a Powershell script that would "Export" the vms or take snapshots as Hyper-V has some amount of API Accessible functionality, however that would be far from optimal and wouldn't have fixed my storage issue. Even Windows Server Backup function which is a built in feature for Windows Server machines, had me be talked out of it by my supervisor who said back in the day you'd immediately discard it.

However i was lucky enough that ChatGPT found me a perfect Match, Veeam Backup & Replication tool. Tends to be free, i immediately downloaded the Community Edition and began setting it up.

A key differentor of Veeam to say WSB (Windows Server Backup) is that Veeam actually uses the backend services that manage Hyper-V out of the box, so it is downright aware of what the VM is doing, much like Proxmox.. I didn't waste time and immediately installed it.


Once installed i created two backup repositories, these are where our backups will reside in. Again we're using 2 to apply the 3-2-1 rule.

![alt text](image-19.png)

With the Repos in place, i created the backup job which runs daily at 4 am.

![alt text](image-21.png)

![alt text](image-22.png)

Also specified i wanted to backup any and all VMs hosted, with exception our PBS vm.

![alt text](image-23.png)

![alt text](image-24.png)

After that, we created something similar to a Sync job, that ensures that everything on the default repo is copied over to our secondary repo for it to be the 2nd medium. It's important to note the Repo's are on two different physical disks.

![alt text](image-25.png)

And since that was all we had to do to get started, we'll start our first backup job.

![alt text](image-26.png)

No errors occcured during the backup.

So we'll conclude to attempting our restore. this time in an effort to try and save time, we will just delete a vm entirely and pray that the restore works, it's more relaistic if we think about it.

<img width="916" height="478" alt="image" src="https://github.com/user-attachments/assets/e7e6d8c1-a74f-448f-89b6-0edae3874513" />

First i'll attempt to verify that the backup image exists, then i'll remove the specified VM below, and attempt a full VM Restore.

<img width="714" height="608" alt="image" src="https://github.com/user-attachments/assets/9a73a10d-ac18-4215-9499-8972b90ff329" />

I must mention there's a exciting amount of options with Veeam. We'll go for Entire VM Restore. We'll however proceed to select the service-zomobid game server vm.

<img width="1334" height="581" alt="image" src="https://github.com/user-attachments/assets/d8bc454f-ed5c-4af7-bd5c-b43d475dc208" />

We can see, that it detects both our Repo's (cloned after eachother) and gives us the option to search our vm from our backups.

<img width="700" height="226" alt="image" src="https://github.com/user-attachments/assets/da2b04b7-1dae-45de-ac84-92e8ef8aabc5" />
<img width="462" height="77" alt="image" src="https://github.com/user-attachments/assets/c872be58-e674-4b0b-b94c-4372ed181e00" />

After the VM was deleted, and the Harddisk manually aswell.. we can proceed with our Restore test.

<img width="564" height="174" alt="image" src="https://github.com/user-attachments/assets/b16eb0ce-f1d0-415e-97ea-8dc4e479acbb" />

<img width="751" height="538" alt="image" src="https://github.com/user-attachments/assets/535558b8-85af-40fb-bd3f-03c9c1b6a1b2" />

We'll restore it to the original location, in an attempt to get a 1:1 as it was restoration.

<img width="603" height="460" alt="image" src="https://github.com/user-attachments/assets/680957aa-0022-4896-aae0-fed121be8bda" />

And that easily it's already installing. And after about 3 minutes, we have our VM back.

<img width="706" height="578" alt="image" src="https://github.com/user-attachments/assets/e89743a5-6116-4812-a315-3972d3851438" />

<img width="1025" height="852" alt="image" src="https://github.com/user-attachments/assets/3f21bc78-e818-4c70-904c-6f38f2c485a2" />

Genuienly shocking how easy that was.

Let's see weather recovering singular files from a backup is just as easy, for that we'll just choose guest file restore when the option arises.

<img width="410" height="67" alt="image" src="https://github.com/user-attachments/assets/3db8dfcb-5de1-4f2c-b8c2-9349ed58ed21" />

This time it asks us whithin the backup weather we wish to restore a file from the incremental one, or the last full backup.

<img width="780" height="553" alt="image" src="https://github.com/user-attachments/assets/36080441-8662-4a8d-b15a-4a82e95105b9" />

<img width="777" height="552" alt="image" src="https://github.com/user-attachments/assets/0e8473f8-9ae2-4fc3-b714-51cef1314dc1" />

I selected the Incremental one, since it's the newest, hoping it smartly accessed the full backup behind the szenes to allow me to effortlessly browse the entirety of the system. We're also using service-zomboid for this.

<img width="1024" height="687" alt="image" src="https://github.com/user-attachments/assets/3a6d72d2-d5fa-4189-9bec-5cd184bb4deb" />

Incredible.. We have all our files to our disposal and, unlike proxmox we have the option to restore the file(s) itself directly to the productive VM, without having to download it ourselves and post it into the original place..

<img width="1033" height="691" alt="image" src="https://github.com/user-attachments/assets/27bef9db-9934-42a3-9ade-6a7871c28fc9" />

<img width="325" height="232" alt="image" src="https://github.com/user-attachments/assets/b73a15ce-5d9e-4adb-857b-634bf52edc6f" />

This concludes our File Restoration Test.. which allows us to move on to the last part of our concept.. the Cloud backup.

And veeam got us covered here, however instead of using AWS, Google Cloud or Azure.. Which i could no doubts do, and super cool veeam actually supports pretty much out of the box, and even offers extentions for applications not offered out of the box like in this case AWS..

But i chose to doube down on security, reliability.. and by all means ease of access.. So i choose to use my Encrypted Proton Cloud.. My backups sum up to around 600-800gb which is wildely below my estimates and calculations within the concept, thanks to proxmox's and veeams incredible storage efficiency.. however.. this gives me just enough breathing room to push the updates using veeam straight to my proton encrypted secure cloud.

The implementation is quite easy, we just add another Backup Repo.. and because this is a Windows System running Veeam it simply integrates into the C Drive, and the Proton Application automatically Syncs changes up to the cloud.

<img width="188" height="68" alt="image" src="https://github.com/user-attachments/assets/c15b3afb-0711-4d7b-912b-8a94bff028c3" />

<img width="1207" height="117" alt="image" src="https://github.com/user-attachments/assets/177490a0-3626-42ec-9ba9-6ee7d49a3bfd" />

The data will temporarily be stored on the C Drive, which we can thankfully handle without making a big huss out of it.

<img width="262" height="57" alt="image" src="https://github.com/user-attachments/assets/c8e36230-1e31-48b4-9545-32c999cf591d" />













