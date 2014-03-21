# Hexahot

The Hexahot is conceptually simple: a solar closet attachment for a hexayurt, providing hot water and interior heat. 

## Inner Volume

The space is defined, as usual for Hexayurts, by 4 x 8 sheet goods. The back is 4 x 8, the bottom is a prism that is 3 x 8 x 1, and the top is another 4 x 8. 

The bottom volume is of course 24 cubic feet. The volume of the prism is half the volume of the rectangular solid, which, at 3 x 3 x 8, is 72 cubics, meaning an additional 36 ideal cubic feet. 24 + 36 gives 60 cubes of interior volume. 

We fill this interior space to 80% with water, giving a holding capacity of 48 cubic feet. This is quite a bit of water, roughly 375 gallons or 1360 liters. We'll get to precise plumbing strategies later, but we want a mix of plastic and glass 'batteries', metal heat exhangers (olive oil cans work), and 5 gallon buckets plumbed to add and remove water. Most of the water is sealed and used only to hold heat. 

The 20% air is critical to system function; the water must be stacked to provide air gaps at each layer. 

All planes of the HexaHot are made of insulated paneling. The back plane can and should be a wall of a Hexayurt. The HexaHot requires 4 panels, one of which may also be a Hexayurt wall. 

If you think carefully, you'll account for 2.5/3.5 of the panels. The remaining half panels are used to make small boxes, described below, on the interior. These account for part of the 20% air volume. 

The gaps between panels must be insulated and carefully sealed. The interior and exterior seams should both be taped, and the overall shape constructed so that the panels are gapped on the outside. These gaps must be filled: styrofoam, 
perlite, rolled bubble wrap, anything that provides reasonable insulation. 

More insulation is dramatically better here: use thick panels. The bottom panel has to bear the weight of almost 3000lbs of water, unevenly distributed, which may require reinforcement along the back. It may be necessary to use 3x3 shipping pallets with insulation packed into the gaps as the bottom layer: these are rated to support the necessary weight. The other panels bear only their own load. The exterior volume being closer to 9 feet in length, the 3 x 9 footprint is useful, and we waste little to no paneling if we're building HexaHots in lots of four, since we can now cut 4 1x8 strips for the front. 

## Heat exchange.

Sealed to the plane facing towards maximum solar gain is a 4 x 8 sheet of clear material. This may be double-wall or otherwise insulating, but this is not critical. This plane (the insulation) must be painted a matte black, and there are advantages to using a relatively deep layer of paint or some few millimeters of plaster here. The reason is that significant radiation wwould reach the reflective layer and we want to capture it on the bounce. A stucco texture is best, for the same reason. A black primer layer of paint to texture the Mylar, followed by several coatings of flour and charcoal, with perhaps a little chalk, ought to do the trick, or just mix large amounts of charcoal into stucco. You've won when it looks the same dull, light-stealing black from any angle. 

This is built with a small air gap. The easiest way is to use 2 x 4s and tape or other sealant. For maximum gain in northerly climates, it may be worth adjusting the angle of the transparent pane to minimize reflection in winter. 

On either side, we install small fans, the kind used in a computer power supply should suffice. The intake blows into a small box with a passive damper made of sheet plastic, while the exhaust reverses this arrangement: the fan is on the interior and the damper is on the surface of the pane. These boxes, constructed from the insulation, provide a barrier to prevent heat loss when the sun isn't shining. 

We need to sense the sun and turn on the fans. Conveniently, we can combine these functions into a small solar panel. When the panel is generating current, it drives the fans. We benefit greatly from a cheap microcontroller between panel and fans, and a small battery store to simplify power management. 

In a refugee scenario this can be replaced with a small battery and either manual attention or a very cheap mass-produced sensor. The power involved is minimal, in the tens of watts while the system is running. 

The air gap must be completely airtight, as must be the HexaHot in general. Air leaks will devastate performance: the heat needs time to transfer into the water, and would rather escape under the differential pressures of the system. Smoking it out a couple times to find gaps is a must, and maintenance should repeat this process. 

### Mechanism

Everyone is familiar with the alarming heat which can build up within a car in the sun. The reaction panel of the HexaHot is an idealized version of this greenhouse effect: it will heat rapidly to the point where it would cause a plastic transparent pane to warp. Glass is actually a better choice if available: as long as the fans function, a plastic layer will work. Flexible roll sheeting is not recommended here. 

The fans are intended to kick in before this happens. This draws the hot air into the battery, and flushes merely warm air into the reactor, where the sun warms it further.

We put smaller metal bottles of water towards the top of the battery, for maximum absorption. Entropy does the rest: the heat travels into the water, bringing the battery up to a basically even temperature.

When the sun isn't doing the job anymore, we shut off the fans. The small boxes prevent the fan holes from leaking significant heat. The insulation retains as much of the heat as it can: the water is optimal at holding heat per unit volume. 

The temperature we can achieve and hold varies dramatically on climate. I want to stress that the insulation provided by the panels is pretty good, and that 375 gallons of water is a *lot* of water. Getting this to a temperature too hot to comfortably wash with is achievable: in some climates, we'll need to detect a maximum heat and vent excess when this is reached. 


## Extraction

We interface in two ways. The simplest is a round hole, perhaps six inches diameter, cut into the Hexayurt plane. To heat the space, pull the plug out: air will exchange and the Hexayurt will become dramatically warmer. More sophisticated variations of this will readily occur to the adventurous reader. 

The other use is hot water, as the battery can easily get hot enough for washing, indeed warmer than this. For a HexaHot, we want a row of 5 gallon buckets as the middle layer: these are plumbed together to provide hot water on demand. It's probaby best practice to fill from an outside barrel with sun exposure, when that barrel is already in a warm mood. 

### Inner Structure

The bottom layer is composed of tightly packed 2 liter bottles, filled and capped. This water is never consumed but may as well be potable for emergency use. These must be bound laterally, because they will experience considerable pressure from the weight of the higher levels. 

The second layer is the plumbing layer and consists of two rows of 5 gallon buckets with small soda bottles tucket into the gaps. These are plumbed together with tubing, such that when you tap bucket n, bucket 0 will go completely dry, then bucket 1, etc. This means in practice they may be refilled one bucket at a time, with each bucket volume traveling over to the next: the resulting effect is easy to hear and makes the system manageable.

All water above this layer should be in metal. Sealed olive oil cans are good, with slight air gaps. A certain amount of unopened aluminum cans really come in handy here; best to use something that isn't carbonated. There are a few drinks that come in an aluminum bottle with a screw-top lid: these are gold. 

The output fan takes air from the bottom layer, and the top layer needs reasonably good air exchange. There should never be a reason to open the battery, though building one side to be openable is only sensible. 

## Construction

As always, this is the rub.

Building a solar closet is art. The HexaHot follows fairly standard HexaYurt practices: insulated paneling and lots of tape. In this case we use a bit of wood, and as little metal as possible, mostly because the properties are all wrong for the application. 

The first step is to build the reactor. We start with a 4x8 insulated panel. The shinier side goes down, the printed side gets a coat of paint. We then apply any sort of coating that will stay on in hot, moist conditions, and provide the dull-black-at-all-angles trait of a good black body. Building it up to the point of thermal mass is counterproductive. 

Some mixture of chalks, charcoal, and glue is likely to do the job. Charcoal and marine wood glue, with a bit of zinc oxide paint, is what we're going for, and the charcoal shouldn't be too finely divided. 

We cut small holes on both sides: one gets a fan and one gets a plastic damper sheet. We then build a 2x4 frame that covers the top and both sides. We lay this over, add the transparent panel, and then make a tape sandwich with really burly, large, expensive tape. The bottom is still open. 

Assuming the yurt is up, we lay 3 pallets down and tape them to the bottom of the wall. They need to be insulation filled and should probably have a thin tarp over them for airtightness. This means being careful when populating the bottom layer!

One side is built up, the front strip attached, and the intake fan box built. This should suffice to attach the reactor panel, with a lot of tape and about 4 people. This tape is almost uncomfortably strong and the whole structure should hold both air and its own weight with full assembly. 

The last wall has to be built and attached and the whole thing checked for leaks. It really should be able to handle a couple pounds of overpressure: I can't stress this enough. We then remove the wall and add the inner structure. It's cramped, but this can't be helped. 

The layers probably have to go in incrementally. The buckets can be laid empty and should be, since leaks are bad and this lets us check. This part is art. At the end, the last wall goes in, and Bob is your proverbial uncle. 



