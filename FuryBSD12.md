# install notes

- default root password in live environment is 'furybsd'
- don't expect to be able to effectively use the mouse at least in the live environment.

## in vmware
- had to set hard disk to "sata" instead of "iscsi"
- had to format (newfs -U /dev/<device><partition>)

## disk formatting

for the purposes of example the device name is "ada0"

- Remove Existing partitions
  gpart destroy -F ada0
- Create GPT slice (partition)
  gpart create -s gpt ada0
- Now ada0p1 should exist in /dev/
- CREATE the FFS slice (filling the entire platter)
  gpart add -t freebsd-ufs ada0
- format the slice
  newfs -U /dev/ada0p1
