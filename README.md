# myss7
* This project id for a mm3ua implmentation protocol for ussd-c to ussd-g
* from: 
  * https://www.rfc-editor.org/rfc/rfc4666.html#page-11
  * https://github.com/RestComm/jss7/releases/latest
* 
## MTP3 Feature 
* Larger information blocks can be accommodated directly by M3UA/SCTP, without the need for an upper layer segmentation/ re-assembly procedure as specified in recent SCCP or ISUP versions. However, in the context of an SG, the maximum 272-octet block size must be followed when interworking to a SS7 network that does not support the transfer of larger information blocks to the final destination.
* The distribution of SS7 messages between the SGP and the Application Servers is determined by the Routing Keys and their associated Routing Contexts.  A Routing Key is essentially a set of SS7 parameters used to filter SS7 messages, whereas the Routing Context parameter is a 4-octet value (integer) that is associated to that Routing Key in a 1:1 relationship.  The Routing Context therefore can be viewed as an index into a sending node's Message Distribution Table containing the Routing Key entries.
* Possible SS7 address/routing information that comprise a Routing Key entry includes, for example, the OPC, DPC, and SIO found in the MTP3 routing label.  Some example Routing Keys are: the DPC alone, the DPC/OPC combination, or the DPC/OPC/SI combination.  The particular information used to define an M3UA Routing Key is application and network dependent, and none of the above examples are mandated.
*  An Application Server Process may be configured to process signalling traffic related to more than one Application Server, over a single SCTP Association.  In ASP Active and ASP Inactive management messages, the signalling traffic to be started or stopped is discriminated by the Routing Context parameter.  At an ASP, the Routing Context parameter uniquely identifies the range of signalling traffic associated with each Application Server that the ASP is configured to receive.
*  Routing Keys SHOULD be unique in the sense that each received SS7 signalling message SHOULD have a full or partial match to a single routing result.
*  There are two ways to provision a Routing Key at an SGP.  A Routing Key may be configured statically using an implementation dependent management interface, or dynamically using the M3UA Routing Key registration procedure.
*  Message Distribution at the ASP The ASP must choose an SGP to direct a message to the SS7 network.
*  Signalling Gateway SS7 Layers The SG is responsible for terminating MTP Level 3 of the SS7 protocol, and offering an IP-based extension to its users.
*  The SGP provides a functional interworking of transport functions
   between the SS7 network and the IP network by also supporting the M3UA adaptation layer.  It allows the transfer of MTP3-User signalling messages to and from an IP-based Application Server Process where the peer MTP3-User protocol layer exists.
*  The failover model supports an "n+k" redundancy model, where "n" ASPs
   is the minimum number of redundant ASPs required to handle traffic and "k" ASPs are available to take over for a failed or unavailable
   ASP.  Traffic SHOULD be sent after "n" ASPs are active.  "k" ASPs MAY be either active at the same time as "n" or kept inactive until
   needed due to a failed or unavailable ASP.
* A "1+1" active/backup redundancy is a subset of this model.  A simplex "1+0" model is also supported as a subset, with no ASP redundancy.
* The SCTP and TCP Registered User Port Number Assignment for M3UA is 2905.
* Example 1: ISUP Message Transport

      ********   SS7   *****************   IP   ********
      * SEP  *---------*      SGP      *--------* ASP  *
      ********         *****************        ********

      +------+         +---------------+        +------+
      | ISUP |         |     (NIF)     |        | ISUP |
      +------+         +------+ +------+        +------+
      | MTP3 |         | MTP3 | | M3UA |        | M3UA |
      +------|         +------+-+------+        +------+
      | MTP2 |         | MTP2 | | SCTP |        | SCTP |
      +------+         +------+ +------+        +------+
      |  L1  |         |  L1  | |  IP  |        |  IP  |
      +------+         +------+ +------+        +------+
          |_______________|         |______________|

      SEP - SS7 Signalling End Point
      SCTP - Stream Control Transmission Protocol
      NIF - Nodal Interworking Function
      
 * nodal    interworking function (NIF) that allows the MGC to exchange SS7    signalling messages with the SS7-based SEP.  The NIF within the SGP
   serves as the interface within the SGP between the MTP3 and M3UA
 * SCCP Transport between IPSPs

               ********    IP    ********
               * IPSP *          * IPSP *
               ********          ********

               +------+          +------+
               |SCCP- |          |SCCP- |
               | User |          | User |
               +------+          +------+
               | SCCP |          | SCCP |
               +------+          +------+
               | M3UA |          | M3UA |
               +------+          +------+
               | SCTP |          | SCTP |
               +------+          +------+
               |  IP  |          |  IP  |
               +------+          +------+
                   |________________|
** M3UA Protocol Elements
 * Common Message Header
 *    The protocol messages for MTP3-User Adaptation require a message
   header that contains the adaptation layer version, the message type,
   and message length.

      0                   1                   2                   3
      0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |    Version    |   Reserved    | Message Class | Message Type  |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |                        Message Length                         |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
* The version field contains the version of the M3UA adaptation layer.


# Transfer Messages
* Payload Data Message (DATA)
*       ** Network Appearance       Optional
*        *** The Network Appearance parameter value is of local significance only, coordinated between the SGP and ASP.  Therefore, in the case where an ASP is connected to more than one SGP, the same SS7 network context may be identified by different Network Appearance values, depending on which SGP a message is being transmitted/ received.
*        
        ** Routing Context          Conditional
        ** Protocol Data            Mandatory
        ** Correlation Id           Optional


*        0                   1                   2                   3
       0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |        Tag = 0x0200           |          Length = 8           |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |                       Network Appearance                      |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |        Tag = 0x0006           |          Length = 8           |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |                        Routing Context                        |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |        Tag = 0x0210           |             Length            |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      \                                                               \
      /                        Protocol Data                          /
      \                                                               \
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |        Tag = 0x0013           |          Length = 8           |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      |                        Correlation Id                         |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+


* Where multiple Routing Keys and Routing Contexts are used across a common association, the Routing Context MUST be sent to identify the traffic flow, assisting in the internal distribution of Data messages.
* Protocol Data: variable length

     ** The Protocol Data parameter contains the original SS7 MTP3
      message, including the Service Information Octet and Routing
      Label.
* The Protocol Data parameter contains the following fields:

      **  Service Indicator
      **   Network Indicator
      **    Message Priority

      **   Destination Point Code
      **   Originating Point Code

      **   Signalling Link Selection Code (SLS)

      **   User Protocol Data, which includes

      **      MTP3-User protocol elements (e.g., ISUP, SCCP, or TUP parameters
