# LeafletForBlazor - Tracking and Monitoring points position
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

          var analysis = realTimeMap.Geometric.Points.Analysis(
                                item => item.type != "ambulance");

The previous expression will remove "ambulances" from the Nearby Points Analysis.

#### Nearby Points Tracking

Configuring the Nearby Points Tracking tool is done by configuring the **nearby** property and implementing the corresponding event methods: **OnNearbyThresholdFired** and **OnNearbyThresholdClosed**.

           analysis.nearby = new RealTimeMap.NearbyAnalysis()
               {
                   threshold = 35,
                   unit = RealTimeMap.UnitOfMeasure.meters
               };
           analysis.OnNearbyThresholdFired += onNearbyThresholdTrigger;
           analysis.OnNearbyThresholdClosed += nearbyThresholdTriggerClosed;

and event methods:

1. When two or more points meet the condition:

                    public async void onNearbyThresholdTrigger(object sender, RealTimeMap.NearbyThresholdArgs args)
                    {
                    }

**RealTimeMap.NearbyThresholdArgs args** will return tuples of nearby points and the distance between them. 
   
3. When no point meets the condition:

                    public void nearbyThresholdTriggerClosed(object sender)
                    {
                    }

[more about Nearby Points Tracking distance](https://github.com/ichim/LeafletForBlazorTracking/tree/main/Nearby%20Points%20Tracking%20distance)


---

