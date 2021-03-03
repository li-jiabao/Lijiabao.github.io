---
title: A Button is a Bean
author: lijiabao
date: 2020-12-06 01:47:02.614347100 +0800
category: JavaBeans(TM)
categories: JavaBeans(TM)
tags: JavaBeans(TM)
---

# A Button is a Bean

Take a closer look at the **Palette**. All of the components listed are beans. The components are grouped by function. Scroll to find the **Swing Controls** group, then click on **Button** and drag it over into the visual designer. The button is a bean!

Under the palette on the right side of NetBeans is an inspector pane that you can use to examine and manipulate the button. Try closing the output window at the bottom to give the inspector pane more space.

## Properties

The properties of a bean are the things you can change that affect its appearance or internal state. For the button in this example, the properties include the foreground color, the font, and the text that appears on the button. The properties are shown in two groups. **Properties** lists the most frequently used properties, while **Other Properties** shows less commonly used properties.

Go ahead and edit the button's properties. For some properties, you can type values directly into the table. For others, click on the **...** button to edit the value. For example, click on **...** to the right of the **foreground** property. A color chooser dialog pops up and you can choose a new color for the foreground text on the button. Try some other properties to see what happens. Notice you are not writing any code.

## Events

Beans can also fire events. Click on the **Events** button in the bean properties pane. You'll see a list of every event that the button is capable of firing.

You can use NetBeans to hook up beans using their events and properties. To see how this works, drag a **Label** out of the palette into the visual designer for `SnapFrame`.

Edit the label's properties until it looks just perfect.
