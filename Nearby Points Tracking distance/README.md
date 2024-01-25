# Nearby Points Tracking distance

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

## Highlighting nearby points (vehicles).

To highlight nearby points we will use two LeafletForBlazor classes: **DisplayPointsFromArray** and **DisplayPolylinesFromArray**.
We will use the **DisplayPointsFromArray** class to highlight points located near each other.
We will use the **Display Polyline From Array** class to display the distance between the points (which must be less than or equal to the threshold).

    public async void onNearbyThresholdTrigger(object sender, RealTimeMap.NearbyThresholdArgs args)
    {
        if (realTimeMap == null)
            return;
        await realTimeMap.Geometric.DisplayPolylinesFromArray.deleteMeasure();  //we delete the measurement lines
        await realTimeMap.Geometric.DisplayPointsFromArray.deleteAll();          //delete the Point (PointSymbol) displayed in the map for highlighting
        if (args.tuples == null)
            return;
        foreach (var item in args.tuples)
        {
            if (item.tuple == null)
                return;
            //we highlight the points that are close (to each other)
            await realTimeMap.Geometric.DisplayPointsFromArray.add(new double[2] { item.tuple.Item1.latitude, item.tuple.Item1.longitude }, new RealTimeMap.PointSymbol()
                {
                    color = "yellow",
                    fillColor = "yellow",
                    opacity = 0.8,
                    fillOpacity = 0,
                    radius = 12,
                    weight = 2

                });
            //we add a measuring line with the distance
            await realTimeMap.Geometric.DisplayPolylinesFromArray.addMeasure(new RealTimeMap.MeasureLine()
                {
                    start = new double[2] { item.tuple.Item1.latitude, item.tuple.Item1.longitude },
                    end = new double[2] { item.tuple.Item2.latitude, item.tuple.Item2.longitude },
                    text = $"{Math.Round(item.distance, 1).ToString()} m",
                    textAnchor = new double[2] { -2, 8 },
                    labelStyle = "min-width:40px;height:100%;background-color:#084886;border-radius:6px;color:#d2efff;text-align:center;font-size:10px;"

                });
        }
    }

## Remove highlight from the map

    public void nearbyThresholdTriggerClosed(object sender)
    {
        if (realTimeMap == null)
            return;
        Task.Run(async () =>
        {
            await realTimeMap.Geometric.DisplayPointsFromArray.deleteAll();
        });
        defaultAppearace(); //default appearance
        Task.Run(async () =>
        {//we delete the measurement lines
            await realTimeMap.Geometric.DisplayPolylinesFromArray.deleteMeasure();
        });

    }

