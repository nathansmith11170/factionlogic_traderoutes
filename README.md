# factionlogic_traderoutes

This mod intends to add trade routes to the game's faction logic, which should hopefully make freight distribution more predictable.
An ancillary benefit is that this should nearly eliminate AI ships which are running expensive autotrader scripts.

Note: Other mods that add traders to jobs.xml may still have their traders running, this mod only eliminates autotraders and middlemen from vanilla jobs

# technical summary

This mod collates a bunch of data about production and consumption rates into tables so that the faction can determine routes that will supply demand.
Routes can be two-way. They are defined in a table with string keys that has the following data:

<blockquote>
  <p>SectorA</p>
  <p>SectorB</p>
  <p>WareMovedFromAToB</p>
  <p>AmountMovedFromAToB</p>
  <p>WareMovedFromBToA</p>
  <p>AmountMovedFromBToA</p>
</blockQuote>
  
The factionlogic will determine which routes are necessary to supply demand in a sector by creating new routes, effectively 'contracting' the production of the supplier. Then,
freighters are assigned to the route that attempt to move the specified amount of goods between the sectors. Routes are evaluated every 20 minutes to determine if there are too few or too many freighters moving the specified wares.

Freighter behavior while on a route is basically: buy ware in SectorB, sell ware in SectorA, buy ware in SectorA, sell ware in SectorB, on repeat
Freighters for trade routes which have no assigned route will find nearest owner station and enter internal storage to await reassignment.

Factionlogic will only attempt to purchase wares from other factions in the following scenario:
<blockquote>
  <list>
    <li>There is no more supply of the ware available in-faction</li>
    <li>There is a sector that is of neutral or greater relation within 2 gate jumps of a friendly sector</li>
    <li>That sector produces the needed ware and has not already contracted those wares for another route</li>
  </list>
</blockQuote>
