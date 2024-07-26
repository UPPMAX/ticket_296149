# ticket_296149

## Solution

Suggest to create a Rackham Small project and do the transfer with its
computational resources

## Problem

> I did write previously about problems when trying to transfer files from rackham to dardel (NAISS support #294917). 
> I got a link (https://docs.uppmax.uu.se/cluster_guides/dardel_migration/) and have followed the info there. 
> I have managed to create the slurm script for transferring my data from rackham to dardel, but when running the script, I get an error message:
> sbatch: error: Batch job submission failed: Invalid account or account/partition combination specified
>
> Why is that? I suppose that my new compute project (naiss2024-22-760) should work also on rackham?

I am unsure why it does not work on Rackham. I am sure there is no `naiss2024-22-760`, 
as when I search the `proj` folder for any project starting with `naiss2024-22-76`,
I do not see yours:

```bash
[richel@rackham3 proj]$ find . | grep naiss2024-22-76
./naiss2024-22-769
```

I assume you see the same, when (following the documentation at https://docs.uppmax.uu.se/getting_started/project/#view-your-uppmax-projects )
you visit https://supr.naiss.se .


> Also, when I do a check on wether the generated script works (as instructed when the darsync gen script is 
> finished: bash /home/emmajo/darsync_Microcystis.slurm) the file transfer actually starts! 
> And I find it a bit odd that running the script with bash works but not when submitting it with 
> sbatch. (I did terminate the transfer though, since there are quite many files, and since I 
> also need to transfer other folders from rackham to dardel I would want a script that actually works!)

It is expected that one can run scripts directly, without using `sbatch`:
it means the script is run on the shared login node. Maybe the node has been
empty enough, maybe the job is light enough, maybe you've gotten an email
from my colleagues that you're overusing the login node :-) . 


> This is how my generated script looks like:

```
#!/bin/bash -l
#SBATCH -A naiss2024-22-760
#SBATCH -M rackham
#SBATCH -t 10-00:00:00
#SBATCH -p core
#SBATCH -n 1
#SBATCH -J darsync_Microcystis
#SBATCH --output=/home/emmajo/darsync_Microcystis.out
#SBATCH --error=/home/emmajo/darsync_Microcystis.err

rsync -e "ssh -i /home/emmajo/id_ed25519 -o StrictHostKeyChecking=no" -acPuv /proj/uppstore2017173/Microcystis/ emjoha2@dardel.pdc.kth.se:/cfs/klemming/projects/snic/naiss2024-23-352
```

Looks great! And, as stated before, it will not work, because `naiss2024-22-760` is not a Rackham project.


> Should also say that both the storage (uppstore2017173) on rackham, and the compute (naiss2023-22-475) projects has expired, 
> which might also cause some problems? 
> These projects belong to my former PhD supervisor (Karin Rengefors), 
> and since I haven't finished all bioinformatics after my defense, 
> she and I decided that I would work with the data on my own 
> storage/compute projects (naiss2024-22-760 and naiss2024-23-352, respectively).

Sure, that can work perfectly fine :-)

I did check if the other project is a Rackham project, but also here, I found nothing on Rackham:


```bash
[richel@rackham3 proj]$ find . | grep naiss2024-23-35.
./naiss2024-23-354
./naiss2024-23-359
./naiss2024-23-358
```

