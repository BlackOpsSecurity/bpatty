##egresscheck-framework
This is a nice tool for checking what ports are open between two network segments.  So lets say you have PC1 on 192.168.0.10 and PC2 on a different subnet at 192.168.1.222..0

* First [get the framework](https://github.com/stufus/egresscheck-framework).
* On PC1, run `python ecf.py`
* Set your source IP by typing `set SOURCEIP 192.168.0.10`
* Then set your target IP by typing `set TARGETIP 192.168.1.222`
* Set your port range (I like testing everything) by typing `set ports 1-65535`
* Test both TCP and UDP by typing `set PROTOCOL all`
* Type `get` and hit Enter to confirm your selections on PC1.
* Now type `generate tcpdump` - you will be given a command to run on PC2 starting with `tcpdump -n -U -w /tmp/egress...` - run that command on PC2 now.
* Back at PC1, type `generate python-cmd` to generate a Python one-liner to run on PC1 to pass traffic to PC2.  
* Type `exit` to leave the tool, then run the Python one-liner on PC1.
* When the Python one-liner completes, go to PC2 and *Ctrl+C* the tcpdump job.
* Then, run the two *tshark* commands (generated when you ran *generate tcpdump*) on PC2 in order to parse the TCP/UDP ports open between the two hosts.  For example, my commands were:

---

    tshark -r 03-18-16.pcap -Tfields -eip.proto -eip.src -etcp.dstport tcp | sort -u #For received TCP

    tshark -r 03-18-16.pcap -Tfields -eip.proto -eip.src -eudp.dstport udp | sort -u #For received UDP