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
