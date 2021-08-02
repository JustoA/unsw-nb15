# PCAP -> NETFLOW PIPELINE

Unfortunately, getting Netflow data is not as easy as reading Ethernet packets into a Netflow object in dpkt.
Here is the pipeline to properly assemble a netflow data for analysis:

## IF LINK LAYER IS SLL AND NOT ETHERNET:
You've got to rewrite the pcap file to have Ethernet as its link layer so that the tools can read them.

sudo apt install tcpreplay

tcprewrite --dlt=enet --infile=linux_sll.pcap  --outfile=enet.pcap 

## Convert pcap to netflow
git clone https://salsa.debian.org/debian/nfdump.git

sudo apt-get install libtool	

sudo apt-get install dh-autoreconf	

sudo apt-get install libpcap-dev

sudo apt-get install libghc-bzlib-dev

sudo apt-get install flex


cd nfdump

./configure --enable-sflow --enable-readpcap --enable-nfpcapd

make

sudo make install

nfpcapd -e 1000,1000 -r <pcap file> -l <out directory>

nfdump -R <out directory> -o csv > netflow.csv




