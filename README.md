# DDos-with-SDN-controller
early detection and mitigation of DDos using centralized SDN controller POX

========
			Implementation steps

Basically, an Openflow controller is connected to a network. We then observe the entropy of the traffic related to the controller under normal and attack conditions.

We used the POX controller for this project, because it runs on Python.

The network emulator used is Mininet for creation of network topology.

Packet generation is done with the help of Scapy. Where Scapy is used for generation of packets, sniffing, scanning, forging of packet and attacking. Scapy is used for generation of UDP packets and spoofing the source IP address of the packets.

Here first we create a Mininet topology of 9 switches and 64 hosts 

We the starts pox controller from another terminal and then we can see 8 openflow links are connected.

Now the project is divided into 2 parts ---1 packet generation and detection 
					---2 attack and detection & preventing

1.	packet generation: we then run the packet generation program from one of the host which generates random source ip and send the packets to random destinations.  we then able to see the entropy in controller terminal.

2.	Attack: Here we run the attack from few selected hosts to a selected target. Now the entropy value in the controller decreases.

3.	After that for every 5 sets of process i.e. 250 packets entropy is below the threshold value then ddos is detected and those ip are blocked.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
							

							Manual

1. Installation of Scapy:
sudo apt-get install python-scapy
------------------------------------------------------------------------------------------------------------
2. Test environment:
a. Creation of a new python script for normal traffic generation in the folder mininet/custom
$ cd mininet/custom
$ vim launchTraffic.py

b. create a new python script for attack traffic  in same folder of mininet as above
$ vim launchAttack.py

c. create the new python script for detection in folder pox/pox/forwarding 
$ cd pox/pox/forwarding
$ vim detection.py

d. edit the l3_learning file in the forwarding folder of pox so that it will import entropy from detection script and changes to detect ddos Or replace the l3_learning file with the editted l3 file . 

-------------------------------------------------------------------------------------------------------------

3.Steps for performing the project task:


Finding the threshold of the usual traffic:

a. Creating a Mininet topology by entering the following command:

$ sudo mn --switch ovsk --topo tree,depth=2,fanout=8 --controller=remote,ip=127.0.0.1,port=6633


b. In another terminal  enter the following command for running the pox controller:

$ cd pox
$ python ./pox.py  forwarding.l3_edited

c. Now opening xterm for a host by typing the following command:
mininet>xterm h1 h2 h3 h64

d. In the xterm window of host h1 running the following command:
# python launchTraffic.py –f 2 –e 65
                  where f = first value and e = last value of the number of host to which we want to send packets.

------------------------------------------------------------------------------------------------------------------------------
Detection of DDoS threat using the value of Entropy:

1.On xterm window of h64 enter the following commands:

# script h64.txt
# tcpdump –v	


--------------------------------------------------------------------------------------------------------------------------------

To attack a host

1.  on h1 and parallelly entering the following commands on h2 and h3 xterm windows to launch a attack :

# python launchAttack.py 10.0.0.[number of host in mininet which you want to attack]
             for example 10.0.0.64 if we want to attack 64th host.


----------------------------------------------------------------------------------------------------------------------------------

you can see the entropy values on pox controller terminal 

after successful completion of the above steps, 

terminating tcpdump on h64 and Stop Mininet and pox controller.

