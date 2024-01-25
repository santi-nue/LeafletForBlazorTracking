# Nearby Points Tracking

## Setting up the Blazor WebAssembly App project

Adding the nugget:
  
    dotnet add package LeafletForBlazor --version 2.0.2.2

We import the namespace provided by the package. In the _Import.razor page, add @using statement:

    @using LeafletForBlazor

## Configuring the **RealTimeMap** control in the blazor page

We declare a variable reference to the RealTimeMap blazor control:

    @code {
        RealTimeMap? realTimeMap;
          }

We add the RealTimeMap blazor control:

    <RealTimeMap @ref="realTimeMap" height="620px" width="820px"></RealTimeMap>

## Default symbolization of points

We will use the Appearance() method offered by the Points class. For each category of points (vehicles) we will configure a distinct symbol:


       if (realTimeMap != null)
       {
           realTimeMap.Geometric.Points.Appearance(item => item.type == "intervention crew", true).pattern = new RealTimeMap.PointSymbol()
               {
                   color = " #002e61",
                   fillColor = "#00428c",
                   fillOpacity = 0.8,
                   weight = 2,
                   opacity = 1,
                   radius = 8
               };
           realTimeMap.Geometric.Points.Appearance(item => item.type == "suspicious vehicle").pattern = new RealTimeMap.PointSymbol()
               {
                   color = "#8f1204",
                   fillColor = "#f6322b",
                   fillOpacity = 0.5,
                   weight = 2,
                   opacity = 1,
                   radius = 8
               };
          //and so one
       }

## Setting up the Nearby Points Tracking tool

We configure the instrument parameters, the threshold and the unit of measure:

           var analysis = realTimeMap.Geometric.Points.Analysis(item =>
               item.type == "suspicious vehicle" ||
               item.type == "intervention crew" ||
               item.type == "police crew");
               
           analysis.nearby = new RealTimeMap.NearbyAnalysis()
               {
                   threshold = 35,
                   unit = RealTimeMap.UnitOfMeasure.meters
               };
               
We add the event methods of the analysis tool (without them, the analysis will not work in the background):

           analysis.OnNearbyThresholdFired += onNearbyThresholdTrigger;
           analysis.OnNearbyThresholdClosed += nearbyThresholdTriggerClosed;

events method:

1. the method will be triggered when two or more points reach the neaby condition (proximity threshold):

       public async void onNearbyThresholdTrigger(object sender, RealTimeMap.NearbyThresholdArgs args)
       {
       }

3. the method will be triggered when there are not even two nearby points (proximity threshold):

       public void nearbyThresholdTriggerClosed(object sender)
       {
       }
