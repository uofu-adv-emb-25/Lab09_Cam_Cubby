# FSM
## Invariants
1 Approach signal precede depart signal
2 The power never interrupted
3 Trains only run on each track in one direction
4 System timer is consistent
5 Alarm always on while train present
6 Saftey hazards always trigger alarms and arms, assume both trains present (* or safe state)
7 Train cannot depart w/o being present
8 Sensors don't break
9 If arms are down, alarm is on
10 Alarm off while train not present
~~11 Arms always down while train is present~~ -- I don't think this works actually because we have to go train approach -> present & alarm (10 sec) -> arms lower

| number | arms_down | alarm_on | northbound_present | southbound_present | north_approach | south_approach | north_depart | south_depart | timer | safety_hazard |
|--------|-----------|----------|--------------------|--------------------|----------------|----------------|--------------|--------------|---------|---------------|
| 0      | 0         | 0        | 0                  | 0                  | 6              | 5              | X            | X            |         |               |
| 1      | 0         | 0        | 0                  | 1                  | *              | *              | *            | *            |         | 5             |
| 2      | 0         | 0        | 1                  | 0                  | *              | *              | *            | *            |         | 5             |
| 3      | 0         | 0        | 1                  | 1                  | *              | *              | *            | *            |         | 5             |
| 4      | 0         | 1        | 0                  | 0                  | 6              | 5              | X            | X            | 0       |               | &larr Should go to 0, if no app
| 5      | 0         | 1        | 0                  | 1                  | 15             | 13             | X            | X            |         |               | &larr Last 2 shouldn't happen
| 6      | 0         | 1        | 1                  | 0                  | 14             | 15             | X            | X            |         |               |       Should trigger arm first
| 7      | 0         | 1        | 1                  | 1                  | 15             | 15             | 13?          | 14?          |         |               |
| 8      | 1         | 0        | 0                  | 0                  | *              | 8              | *            | *            |         | 9             | &larr Not really a safety
| 9      | 1         | 0        | 0                  | 1                  | *              | *              | *            | *            |         | 9             |       hazard, but all 4     
| 10     | 1         | 0        | 1                  | 0                  | *              | *              | *            | *            |         | 9             |       violate #9
| 11     | 1         | 0        | 1                  | 1                  | *              | *              | *            | *            |         | 9             |
| 12     | 1         | 1        | 0                  | 0                  | X              | X              | X            | X            |         |               |
| 13     | 1         | 1        | 0                  | 1                  | 15             | 13             | X            | 4            |         |               |
| 14     | 1         | 1        | 1                  | 0                  | 14             | 15             | 4            | X            |         |               |
| 15     | 1         | 1        | 1                  | 1                  | 15             | 15             | 13           | 14           |         |               |

I left X's on states that shouldn't happen, they could all probably be zeroes. I don't think state 12 in general should happen, arms should always raise when last train departs.
I assumed approach always triggered present & alarm, and that present & alarm always transitioned to arm down. Going the other way, last depart always triggers arm up. I changed the "ringing" column to timer like the lab instructions mentioned. I think that only affects state 4, that's like the alarm cooldown and then resets back to zero. I don't think this really works without a
train count variable like in the fsm diagram anyway. Also I realized we were supposed to put the invariants in the second table, I don't think it really matters tho

| number | invariant |
|--------|-----------|
| 16     |           |
