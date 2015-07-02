# Model Transformation from Sequence Diagrams into State Machines

Implementation of the model transformation procedure described in [Activity-Driven Synthesis of State Machines, Rolf Hennicker and Alexander Knapp](https://www.informatik.uni-augsburg.de/lehrstuehle/swt/sse/veroeffentlichungen/2007-FASE/).


## Short description of the algorithm

Input: Sequence diagrams in UML 2.0 interactions syntax

1) Projection of scenarios: extract the communications an object participates in

This results in a table containing all communications of all objects (an excerpt of such a table for an ATM follows):

| Pre state (rcv.) | Sender | Operation       | Receiver | Return | Post state (rcv.) |
|------------------|--------|-----------------|----------|--------|-------------------|
| WaitCard         | user   | insertCard      | atm      | void   | WaitPassword      |
| -                | atm    | requestPassword | user     | void   | -                 |
| WaitPassword     | user   | enterPassword   | atm      | void   |                   |

2) Generation of behaviours from projects scenarios: restructure the table from step 1 by aggregating operations in the same state for each object (the following table is an excerpt of this table for the behaviour of the object *atm* for the scenario *Bad account*):

| Pre state    | Message in    | Messages out                                                                | Return | Post state   |
|--------------|---------------|-----------------------------------------------------------------------------|--------|--------------|
| WaitCard     | insertCard    | (requestPassword, user, void)                                               | void   | WaitPassword |
| WaitPassword | enterPassword | (verifyAccount, consortium, badAccount), (badAccountMessage, user, void), ... | void   | WaitTakeCard |

3) Integration of behaviours into I/O-automata (states *Z*, input alphabet *In*, output alphabet *Out*, transition relation *δ* ⊆ Z×In×Out×Z): integrate all behaviours of each object

4) Translation of I/O-automata into UML 2.0 state machines with stable and activity states

Output: a state machine in UML 2.0 state machine syntax


## How to load this project into Eclipse

1. Click the right mouse button on the white space (not on one of the projects) in the model explorer. Usually, the project explorer is located at the to the left of the editor area.
2. Choose `Import...` from the popup menu
3. Choose `General -> Existing Projects into workspace`; Press the Next button
4. Press the `Browse...` button next to the label `Select root directory`
5. Select the directory into which you cloned this repository
6. Make sure that the checkbox `Copy projects into workspace` is *not* checked
7. Press the `Finish` button

voilà!

## QVTO References

* QVTo-Übersicht: http://help.eclipse.org/luna/topic/org.eclipse.m2m.qvt.oml.doc/references/M2M-QVTO.pdf
* OCL-Übersicht: http://www.se.eecs.uni-kassel.de/fileadmin/se/courses/GT/download/GraphentechnikSS04.06.30.pdf
