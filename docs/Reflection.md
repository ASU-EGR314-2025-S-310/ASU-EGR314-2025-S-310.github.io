
## Reflection

1. There are many secrets in engineering that no one will tell you, you need to ask questions and search to find them out.

2. When soldering, make sure to not bridge pins and test, test, test

3. When your designing a pcb make sure that your grounds are grounded and what should be getting power IS getting power, and what shouldn't ISN'T. 

4. A big thing with working in a group is that everyone should be comfortable. Its a hard balancing act but it can be done even if you don't know anyone in your group. If you stay within your own bubble you'll never leave it. Having good culture within a group will lead success, being able to have that strife within a group AND it not resort to a pink slip or an anonymous review on one of the CATMEs is the backbone to a strong group. Having a bond that deep has helped our team achieve the highest highs and bear through the lowest lows.

5. Teamwork makes the dream work, it also makes the prototype functional. The main reason our team was able to achieve a fully functional prototype and it look professional was due to the teamwork ethic we had. Everyone knew exactly what needed to be done and when someone had issues others would be there to help them get through it. This was evident throughout our project as when someone would get done with their part of the project they would go back to help out and as a result our submissions would be polished and quality.

6. In engineering, there are many ways to skin a cat. No two people are the same and their approaches may be wildly different. Having an unbiased view when deciding the best course of action is a very important part of 314 as it can be the difference between having a functional board and failure.

7. Trust within the team is very important. This allows teammates to work independently and rely on each other to do their work. Trust contributes greatly to team morale by lowering the minor stress levels of individual assignments and responsibilities.

8. Communication is the cornerstone of a good team. Being in a team that allows each member to contribute unique ideas without fear of judgement leads to a much more creative project. Open communication also prevents members from hiding failures and allows the team to work together much more effectively.

9. Defining a project and starting as early as possible is extremely important for success. Every project has issues and bugs to solve. Sometimes these problems require ordering parts, which leads to waiting on shipping times. This especially can bog down progress on the project and force teams to the deadline.

10. Sometimes, it's helpful to get away from project work and just spend time with your group in a casual setting. This will build morale and a sense of cohesiveness in the team, leading to improvements across the board. 


## Recommendations for Future Students

1. If at all possible, build a team with other students that you know. This class is extremely fast paced and stressful, and it would be very difficult to bond as a cohesive team without prior experience working together.

2. Don't get stuck in decision paralysis about the project. Even if the project you choose isn't a perfect idea, starting early gives you the opportunity to have a project that works by the end of the semester. 

3. Communicate with your team. Good or bad, having a team that understands your struggles and successes allows that team to support you as needed.

4. Get work done early. Waiting until the day before or the day of to get labs done holds your entire team back because it robs valuable class time that could be used to work on your project. 

5. Have backup solutions. Any problem has multiple solutions, so be ready is your first solution doesn't work out as well as you expect. 

## Changes for Version 2

For our team's version 2.0, 

For our team's version 2.0, the first change would be to add two more coils and only have one sensor per coil. This change would allow for faster acceleration of the marble and would preserve the speed calculations / the switching timing. A hardware improvement would also be made to the sensors as rather than having a separate pcb a smaller module would make a better mounting apparatus that would look better, be adjustable, and more stable. A main issue we ran into was how hot the coils would get. With our final switching timing and with 2amps going through each coil they would get up to 200 degreed Fahrenheit in under a minute. In a second version we would tune the switching timing so each coil was on for the minimum amount of time possibly removing the need for a fan system, allow for a longer runtime, and reduce power consumption. A huge change would be adding four relays on to the actuator pcb so that each driver output controlled one. If possible given enough freedom, replacing the SPI drivers entirely would be ideal. We had a lot of issues with the HMI refresh rates so there is potential for optimization on the software end to increase responsiveness.