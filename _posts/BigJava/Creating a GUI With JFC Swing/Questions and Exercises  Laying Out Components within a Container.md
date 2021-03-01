
# Questions and Exercises: Laying Out Components within a Container

## Questions

In each of the following questions, choose the layout manager(s) most naturally suited for the described layout. Assume that the container controlled by the layout manager is a `JPanel`.

1. The container has one component that should take up as much space as possible<br />
<td width="50%"><img src="../../figures/uiswing/QandE/Layout1-1.png" width="123" height="60" align="bottom" alt="Layout1-1.png" /></td><td width="50%"><img src="../../figures/uiswing/QandE/Layout1-2.png" width="195" height="100" align="bottom" alt="Layout1-2.png" /></td>

a. `BorderLayout`<br />
b. `GridLayout`<br />
c. `GridBagLayout`<br />
d. a and b<br />
e. b and c

2. The container has a row of components that should all be displayed at the same size, filling the container&#226;&#128;&#153;s entire area.

<img src="../../figures/uiswing/QandE/Layout2-1.png" width="348" height="60" align="bottom" alt="Layout2-1.png" /> <img src="../../figures/uiswing/QandE/Layout2-2.png" width="395" height="89" align="bottom" alt="Layout2-2.png" />

a. `FlowLayout`<br />
b. `GridLayout`<br />
c. `BoxLayout`<br />
d. a and b

3. The container displays a number of components in a column, with any extra space going between the first two components.<br />
<td width="50%"><img src="../../figures/uiswing/QandE/Layout3-1.png" width="123" height="138" align="bottom" alt="Layout3-1.png" /></td><td width="50%"><img src="../../figures/uiswing/QandE/Layout3-2.png" width="186" height="180" align="bottom" alt="Layout3-2.png" /></td>

a. `FlowLayout`<br />
b. `BoxLayout`<br />
c. `GridLayout`<br />
d. `BorderLayout`

4. The container can display three completely different components at different times, depending perhaps on user input or program state. Even if the components&#226;&#128;&#153; sizes differ, switching from one component to the next shouldn&#226;&#128;&#153;t change the amount of space devoted to the component.

<img src="../../figures/uiswing/QandE/Layout4-1.png" width="347" height="132" align="bottom" alt="Layout4-1.png" /> <img src="../../figures/uiswing/QandE/Layout4-2.png" width="347" height="132" align="bottom" alt="Layout4-2.png" />

a. `SpringLayout`<br />
b. `BoxLayout`<br />
c. `CardLayout`<br />
d. `GridBagLayout`

## Exercises

1. Implement the layout described and shown in question 1.

2. Implement the layout described and shown in question 2.

3. Implement the layout described and shown in question 3.

4. Implement the layout described and shown in question 4.

5. By adding a single line of code, make the program you wrote for Exercise 2 display the components from right-to-left, instead of from left-to-right.

<img src="../../figures/uiswing/QandE/Layout2-3.png" width="348" height="60" align="bottom" alt="Layout2-3.png" />
[Check your answers.](answers-ch4.html)
