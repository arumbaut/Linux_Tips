ejemplo resize:
 
for i in sdb sdt sde sdw sdi sdh; do echo 1 > /sys/block/$i/device/rescan; done
multipathd resize map mpathai
pvresize /dev/mapper/mpathai
 