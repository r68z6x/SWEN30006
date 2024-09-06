java c
Project   1:   Specification for Automail:   Software   Modelling and   Design   (SWEN30006_2024_SM2)
Project   1: Specification for   Automail
Background: Automail
Delivering Solutions Inc.   (DS)   has   recently developed and   provided a   Robotic   Mail   Delivery system called Automail to the market. Automail is an automated   mail   sorting   and   delivery system designed to operate   in a large   building that   has   a   dedicated   mail   room.   The   system   offers end-to-end receipt and delivery of mail   items within   the   building   and   can   be   tweaked   to   fit   many different installation environments.The current version of the system supports delivery of letters   using   one   specific   delivery   mode      where every robot operates with the same   behaviour. DS would   like the   system to   also   handle   parcel delivery, and to support exploring alternative delivery   modes which   specialize the   robot      behaviour.
The Automail SystemThe   building the system operates   in (see   Figure   1)   will   have a   number of floors each with   the   same   number of rooms. Below the   rooms   is a   mailroom   (row 0),   to   the   left   and   right   are   robot   tracks (column 0 and column roomsperfloor+1)   respectively.   
Figure   1: Automail   Building   Layout (6 floors   by 5   rooms) with   Robots
The mail   items are   letters which are effectively weightless, or   parcels which   have   a   given   weight. All   mail   items are addressed to deliver within the building to   an   room   identified   by combination of Floor and   Room number.    The current   system   handles   only   delivery   of   letters.
The Automail system consists of two key components:
·      A   MailRoom subsystem which   holds   mail   items after their arrival at the building’s   mail   room. The   mail   roomdecides the order in which mail   items should   be   delivered.
·      Delivery   Robots which take   mail   items from the   mail room, or from other robots,   and   deliver   them throughout the   building. The currently used robot   (see   Figure   2)   has   a   backpack-like container for carrying mail items. Carrying capacity   of the   robot   is weight   limited. As   such   the total weight of carried items cannot exceed robot.capacity.    An   installation   of Automail   can manage a team of delivery   robots of configurable   size.


Figure 2: Artistic representation of one of the   DS   robots
DS provides a simulation subsystem to show that Automail   can   operate to   deliver   mail   items   within the building. The subsystem runs a   simulation   based   on   a   property file,   generates   an output trace of execution and outputs the average time to   deliver a   mail   item.
The simulation subsystem uses a clock to   simulate   operations   of the   mail   room and   robot   subsystems. Broadly speaking, for each tick of the clock   (i.e.   one   unit   of time), the   mail   room   subsystem will load   items to a robot   if there are   robots   available   at the   mailroom;   and   the   robots   will either move to deliver an   item (if there   are   items   in   their   backpack),   deliver   an   item,   or   move   to   return to the   mailroom (if all   items are delivered). Currently,   the   robots   offered   by   DS will   take   one unit of   time   when
·      moving one step   (i.e.,   moving   up or down one floor in   a   building,   or   left   or   right   one   across   the   building).·      delivering items at one apartment once   there.·      transferring items from one robot   to   another.·      being loaded and moved out of   the mailroom.
The simulation completes only after all items   have   been   delivered.
Unfortunately, the staff who designed and implemented the Automail simulation did   not   consider maintainability and future   enhancements.
Current operating mode:   Cycling
This   mode   has   been   implemented   in the current simulation.    It   has the   robots   moving   clockwise through the building delivering mail items, as illustrated in this video (https://canvas.lms.unimelb.edu.au/media_attachments_iframe/20429677? type=videoembedded=true) .
The Cycling mode (MODE=cycling) has every robot operating with the same behaviour. All items a robot carries for delivery will only ever be for a single floor at a time.
Initially: All robots (robot.number>0) start in the MailRoom.




Ongoing   (each timestep):
1.   If there are any   items and   robots   in the   mailroom, the   mailroom will   Load a   robot with   items   for one floor (the floor with the earliest remaining delivered   item),   and   move   it   to   the   bottom   left   (0,   0).
2.   If a   robot   has   items to deliver and   has   not   reached   its target floor,   it will Step   up.
3.   If a   robot   has an   item to deliver it will Step towards   the   left-most   location   to which   its   items   are addressed.
4.   If a   robot   has   reached   its delivery   location and   not yet delivered,   it will   Deliver.
5.   If a   robot   has   no   items to deliver,   it will Step towards the   bottom   right (0,   building.roomsperfloor+1).
6.   If there   is a   robot at   bottom   right (0, building.roomsperfloor+1),   it will   Return to the   mailroom.
You can assume that the Automail Cycling mode   has   been well   tested   and   performs   reasonably   well,though   only   for   letters.
Proposed operating mode:   Flooring
This   mode   has not been   implemented   in the current simulation; you and   your team   need   to   add   this.    It   involves   having one robot   per floor delivering   on that floor,   with   two   robots   (one   at   each   end) bringing mail items up to the other robots for delivery, as per this video   (https://canvas.lms.unimelb.edu.au/media_attachments_iframe/20429779?   type=videoembedded=true) .
The   Levels mode (mode=FLOORING) always involves   exactly   building.floors+2   robots
·      one for each floor, each of which exhibits floor behaviour, and
·      one for each of the leftmost and rightmost columns,   each   which   exhibits   column   behaviour.
Floor robots only ever move on their floor   and   column   robots   only   ever   move   on their   column   and in/out   of the   mailroom.
Floor Behaviour:
·      Initially: the   robot   is at   Room   1 on their floor.   ·      Ongoing   (each timestep):
1.   If the   robot   has   mail   items, continue delivering them (ignore column robots).
2.   If the   robot   is   next to a waiting column   robot (i.e. with   items for this floor), Transfer   them   from the column robot to this robot   and   start   deliver代 写SWEN30006 Project 1: Specification for Automail: Software Modelling and Design 2024 SM2R
代做程序编程语言ing from   that   end   of the floor towards   the other end. (Note: as all   robots   have the same   capacity   and   the   floor   robot   is   carrying   nothing, all items will   be transferable.)
3.   If the   robot   is   heading for a waiting column   robot,   continue   moving towards that robot.
4.   If a column   robot   is   newly waiting, start   heading towards   it.    If two column   robots   are            newly waiting,   move towards the one with the earliest arrival item,   or   the   left   one   if   the   and   Design   (SWEN30006_2024_SM2)   arrival time   is the same.
5.   If the   robot   has   no   items and   no column   robots are waiting for this floor, then do   nothing.   Column behaviour:
·      Initially: the   robot   is   in the   mailroom and   is assigned to the   left or   right column   (one   robot   to   each).
·      Ongoing   (each timestep):
1.   If the   robot   is   in the   mailroom and the   mailroom   has   items,   Load the   robot (as   per   Cycling) and   move   it to   its floor 0, otherwise do   nothing.
2.   If the   robot   is   loaded for delivery, and   not at the destination floor move towards   the   destination floor.
3.   If the   robot   is at the destination floor, and the floor   robot   is   adjacent,   Transfer from this   robot to the floor   robot.
4.   If the robot has transferred,   head towards floor   0.
5.   If the   robot arrived   back at floor 0, enter the   mail   room.
All   items a robot carries for delivery will only   ever   be for a   single   floor   at   a   time.   Robot Operations:
·      Step:   move one floor or apartment towards the destination.    Only   one   robot   can   be   on   a   square at a time. If the destination square   is   blocked,   the   robot   does   nothing.
·      Load: the load destination   is the floor of the item   in the   mailroom   with   the   earliest   arrive
time.   Load the robot with all items destined for that floor which   can   be   transferred   to   the   destination robot, subject to load.    All letters   are   transferred,   and   parcels   are   transferred   in   order of "earliest arrival time package which will fit within weight capacity".
·      Transfer: all   items which can be transferred are   move from the   source   robot   to   the
destination robot, subject to load.    All letters   are   transferred,   and   parcels   are   transferred   in   order of "earliest arrival time package which will fit within weight capacity".
·      Return: all   items are transferred from the robot   back to the   mailroom.
·      Deliver: the   item   is   removed from the   robot and the elapsed time since arrival   used   in   the   item statistics.
Your Task
To expand the usage and trial different   modes of operation,   DS wants   to   update   their Automail   to support (1) parcels, and   (2) the   FLOORING   operating   mode.
The   Base   Package
You   have   been   provided with a zip file containing source code for    the current   version   of   the system,   including an example property file.
(https://canvas.lms.unimelb.edu.au/courses/187398/files/20444343?wrap=1)    
(https://canvas.lms.unimelb.edu.au/courses/187398/files/20444343/download?download_frd=1)
This provides the basis for you to implement   the   additions   described   above.
Please carefully study the provided code and   ensure that   you   are   confident   you   understand      how   it   is set   up and functions   before continuing.   Note that you do   not   need to   understand   all   aspects, just those relevant to the changes you need to   make. If you   have any   questions,   please   make use of the discussion   board.
Note: The simulation will run and generate   mail   items at   random   times   and with   random weights,   based on a seed. You can configure this   in the   property file   (test.properties   by   default).   Any integer value will be   accepted,   e.g.   30006.
Configuration and   Project   Deliverables
(1)   Extended Automail: As discussed above, and for the users of Automail to   have confidence   that changes   have   been   made   in a controlled   manner, you are required   to   preserve   the   Automail simulation’s existing behaviour. Your extended design and   implementation   must   account for the following:
·      Preserve the existing behaviour of the system for configurations where the additional
capabilities are turned off in the   configuration file (properties),   i.e.   mail.parcels=0   and
MODE=cycling.    Note that “preserve” implies   identical output. We   will   use   a   file   comparison   tool to check   this.
·      Add the handling and delivering behaviour for   parcels   (including   robot   capacity   limitation).   ·    Add   the   new   FLOORS   mode   of delivery.
·      Configurable building size and number of robots   (robot.number   for   cycling   mode   or   building.floors+2 for   flooring   mode).It's recommended that you understand the high-level   design   of current   system   so   that   you   can   effectively   identify and update relevant parts. You   don't   need   to   refactor the whole   system, just   those   parts   necessary or helpful to making the   required changes.
(2)   Report:   In addition to the extended Automail, DS also wants you   to   provide   a   report   to   document your design changes and justification of your design. You should also comment on   how easy your changes   make   it to add further mail   items   (beyond   letters and   parcels),   or   further delivery   models (beyond cycling and flooring) in the   future.      Your   report   should   include:
·      a design class diagram which shows all of the changed   design   elements   in your   submission   (at least -   it can show   more than just the changes   but doesn't   need   to   show   all   unchanged elements).
·      a design sequence diagram which illustrates the behaviour   of a   floor-assigned   robot   in   FLOORING mode, for appropriate scenario of your choosing.
More detail of the report   is available on the   LMS   submission   page.
Note: Your implementation   must   not violate the   principle of the simulation by   using   information   that would   not   be available   in the system being simulated.   For example,   it would   not   be appropriate to use   information from the simulation package   (e.g.,   mail   items which   have   not yet   been delivered to the   mail room). We also   reserve the   right to   award   or   deduct   marks for   clever   or very   poor code quality on a case-by-case basis   outside   of the   prescribed   marking   scheme.



         
加QQ：99515681  WX：codinghelp
