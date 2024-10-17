# EXERCISE TITLE 

Based on the following premises, create the (server1/client/server2) schema for DHCP service requests: 

A new user joins a network. We assume the network has 2 DHCP servers. 

    The user sits at their machine and turns on their computer. 
    They work for 1 hour connected to DHCP server #1. 
    The user experiences a 3-minute outage. The user reconnects to the network after 3 minutes. Server #1 is down. 
    The user remains active for 12 hours. 
    The user disconnects for 1 hour. 
    The user reconnects to the network. 
    The user stays on the network for 5 hours and then disconnects permanently 

(*). We assume that our DHCP server operates with average lease times (8 hours).


```mermaid



sequenceDiagram
    participant server1
    participant client
    participant server2
   

    Note over client: The user turns on their computer.

    client->>server1: (Broadcast message) Can you provide me with an ip?. DHCPDISCOVER
    client->>server2: (Broadcast message) Can you provide me with an ip?. DHCPDISCOVER

    server1->>client: You can use x ip x mask for 8 hours DHCPOFFER
    server2->>client: You can use x ip x mask for 8 hours DHCPOFFER

    client->>server1: (Broadcast message) I want the IP of server1 DHCPREQUEST
    client->>server2: (Broadcast message) I want the IP of server1  DHCPREQUEST

    server1->>client: Ok, you can now use your IP. DHCPACK


    note over client: The user work for 1 hour connected to DHCP server1
    note over client: The connection gets interrupted for 3 minutes. server1 is down
    

    client->>server1: Can I still keep using the ip x? DHCPREQUEST

    note over client: There is no answer from server1. A new broadcast message is sent


    client->>server2: (Broadcast message) Can you provide me with an ip?. DHCPDISCOVER
    server2->>client: You can use x ip x mask for 8 hours DHCPOFFE
    client->>server2: (Broadcast message) I want the IP of server2  DHCPREQUEST
    server2->>client: Ok, you can now use your IP. DHCPACK

    note over client: The user want to use the computer for 12 hours
    note over client: After 4 hours the client has to ask again for the availability of the ip as it has reach the 50% of the lease time

    client->>server2: Can I still keep using the ip x?DHCPREQUEST

    server2->>client: Yes, you can DHCPACK


    note over client: As the lease time has end, the client has to start the dhcp proccess again



    client->>server2: (Broadcast message) Can you provide me with an ip?. DHCPDISCOVER
    server2->>client: You can use x ip x mask for 8 hours DHCPOFFE
    client->>server2: (Broadcast message) I want the IP of server2  DHCPREQUEST
    server2->>client: Ok, you can now use your IP. DHCPACK


    note over client: The user keeps using the computer for 4 more hours

    note over client: Before asking again to the server, the client poweroff the computer

    note over client: The client start the computer after 1 hour. 3 lease hours remains


    client->>server2: Can I still keep using the ip x?DHCPREQUEST

    server2->>client: Yes, you can DHCPACK

    note over client:After 3 hours the lease time finish


    client->>server2: (Broadcast message) Can you provide me with an ip?. DHCPDISCOVER
    server2->>client: You can use x ip x mask for 8 hours DHCPOFFE
    client->>server2: (Broadcast message) I want the IP of server2  DHCPREQUEST
    server2->>client: Ok, you can now use your IP. DHCPACK

    note over client: Now, the client has 8 hours remaining

    note over client: After 4 hours the client has to ask again for the availability of the ip as it has reach the 50% of the lease time

    client->>server2: Can I still keep using the ip x?DHCPREQUEST

    server2->>client: Yes, you can DHCPACK

    note over client: 1 hour later the client disconnects permanently


```