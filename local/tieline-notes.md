# 94560 - 50 Line PAX, Tie line with preference access & preference discrimination

Rough diagram notes compiled by Paul Seward (Feb 2020) as part of an ongoing project
to build a more minimal "outgoing only, no preference services" version for the Lotts Rd PAX.

For more details, see http://paulseward.com/downloads/Telephony_PAXes/Ericsson/Ericsson-PAX50

## Outgoing Call

(Calls from PAX, going out the Tie Line to a distant exchange)

Each tie line has a pair of consecutive outlets.  The one in use is determined by the state of the NM and PM marking wires coming from the PAX.

As far as I can tell, when tie lines are configured as a group - Priority callers take precidence over the first caller in the group, even if there are free tie lines later in the group.

```
Initial Conditions:

- Bank: 200ohm battery (via TA(de))
+ Bank: 200ohm battery (via TA(ab))
P Bank: 3000ohm battery (via TL)
M Bank: Connected to "NM" marking wire (via TS4, TG3) if call is "normal"
M Bank: Connected to "PM" marking wire (via TS4, TG3) if call is "priority"

TL siezed (Normal Call)

* FS steps to TL outlet, marked by NM outlet on bank M.
* TA is not operated, as opposing windings are both powered
* TL operates to Eth from FS (via U13)
  * SR operates via TL1
    * TS operates via SR1
      * TS1 (Connects PM wire to U41)
      * TS2 Connects outgoing line (removes incomming facility)
      * TS3 (Disconnects PM wire from U28)
      * TS4 (Disconnects M bank contact from NM/U3)
      * TS5 (Connects NM(U3) to U16)
      * TS6 (Routes P wire from TN to TG)
      * TS7 (Removes ETH from outboing line (removes incomming facility)
      * TS8 (Disconnects TU)
    * (SR2 prepares a path for TV to operate - incoming preference discrimination)
  * TL2 prepares a 100R battery for TC
  * TD is connected accross outgoing line (via TL3 and TL4)
    (Caller receives dialtone from distant exchange)
* ETH pulse on incomming A or B wire
  * TA operates to imbalance
    * TA1 repeats pulse to outgoing line as a disconnect
    * TA2 Operates TC
      * TC1 shorts out TD(ab) to provide clean pulse
  * TA releases
    * TA1 reconnects outgoing line
    * TA2 removes ETH from TC1
      TC1 slow to release, as it's shorted via TA2

TL siezed (Priority Call)
* FS steps to TL outlet, marked by PM outlet on bank M.
* TA is not operated, as opposing windings are both powered
* TN operates via TS6 and the P bank
  * TN1 bridges the - leg to the in-progress call
  * TN2 bridges the + leg to the in-progress call
  * TN3 provides an ETH to operate TG
    * TG1 connects TD(de) to TERM IT (Misc Strip Conn) and provides interruption tone to in-progress call
    * TG2 disconnects PM from Bank M outlet, and passes it on to the next TL in the group
    * TG3 removes short from MR2, putting it in circuit with the outgoing line
    * TG4 disconnects NM from Bank M outlet
  * TN4 provides a holding path for TN
  * TN5 provides a holding ETH to TL

At this stage, the in-progress callers have been alerted to priority interruption, and clear down their call, leaving TL,SR,TS operated.

The priority caller receives dialtone from the distant exchange and dialing can commence as normal.
```


## Incoming Call - TODO

* BAT provided via N(ab), TU3, MR1, TS2, B-wire
* ETH provided via TU1, TS7, A-Wire
* 100R battery fed to M (BAT, R1, TL2, TV2, 25)
* P wire Dis
* Relay N operates
  * N1 operates TU (via ETH, N1, TS8, TU)
    * TU1 operates LS
      * LS1
      * LS2
      * LS3
      * LS4
      * LS5
      * LS6
      * LS7
    * TU2
    * TU3
    * TU4
  * N2 holds N via BAT, N(ed), N2, K2

