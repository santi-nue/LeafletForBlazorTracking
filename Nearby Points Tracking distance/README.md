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
