## SSH
First set up your ssh login via this

```
Host pace
    User <gt username here>
    Hostname login-phoenix.pace.gatech.edu
```

Connect to GT VPN, and you should be able to run `ssh pace` or connect via vscode.

## Sample Slurm
Sample slurm command on to request 8 L40S gpus.  To request nodes for free we must use PACE's version of "overcap" which they call "embers".  The L40S type is always on the "embers" queue.  However, these jobs have an 8 hour time limit, and can be preempted.  Thus to effectively use it, we should write code which can be preempted and automatically restarted.  See this [guide](https://github.com/erikwijmans/skynet-overcap-example)

```
salloc -A gts-jhoffman68 -GL40S:8 -qembers -Crhel9 -c32 --mem=0
```

Account name list
Judy Hoffman: gts-jhoffman68
Danfei Xu: gts-dxu345

## Sample Submitit
I'll fill this out later, but basically just follow the above command.

## Storage
Each lab has 10T free cedar storage which is mounted on both skynet and pace.  

### Skynet locations
Judy: `/coc/cedarp-jhoffman68-0/<your username here>`

Danfei: `/coc/cedarp-dxu345-0/<your username here>`

On skynet, in your home directory create a symlink for instance for Judy's lab I do `ln -s /coc/cedarp-jhoffman68-0/<my username> jcedar`

### Pace locations
Judy: `/storage/cedar/cedar0/cedarp-jhoffman68-0`
Danfei: `/storage/cedar/cedar0/cedarp-dxu-345`

Do the same style of symlink on pace `ln -s /storage/cedar/cedar0/cedarp-jhoffman68-0/<myusername> jcedar`


## Help
Help email
pace-support@gatech.edu
