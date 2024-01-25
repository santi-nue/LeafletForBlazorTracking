# LeafletForBlazor Tracking and Monitoring points position
_LeafletForBlazor nuget - Tracking and Monitoring points position (for IoT App)_
## Project description
One of the first tracking tools is Nearby Points Tracking.
## Nearby Points Tracking

Nearby Points Tracking is a tool with which the user can be warned when two or more positions (vehicles) come close to each other. The approach of two or more positions (vehicles) has the effect of triggering a method that can issue alerts or highlight the positions (vehicles) on the map.
In the image below you can see such an example:

![nearbyTrackingIoT22](https://github.com/ichim/LeafletForBlazorTracking/assets/8348463/46f3aac5-0141-47dd-b155-08f362408626)

In the example, it was chosen that the positions (vehicles) that are close to each other should be highlighted in yellow and the distance between the positions (vehicles) should be displayed. 
The distances (meters) between the points are shown by a label displayed in the middle of the measurement line. 

### Writing code

The Points class offers you the **Analysis()** method, which allows configuring all tools for tracking and/or monitoring. The Analysis method creates a configurable object through properties.In addition, the **Analisys()** method provides the possibility to limit the collection of tracked or monitored points by means of a predicate:



#### Nearby Points Tracking

Configuring the Nearby Points Tracking tool is done by configuring the **nearby** property and implementing the corresponding event methods: **** and ****.

---

### Setting up the Blazor WebAssembly App project

Adding the nugget:
  
    dotnet add package LeafletForBlazor --version 2.0.2.2

We import the namespace provided by the package. In the _Import.razor page, add @using statement:

    @using LeafletForBlazor

### Configuring the **RealTimeMap** control in the blazor page

