# Notes on "The map is not the territory"

Reading through Mark Monmonier's *How to Lie with Maps* this week. It's a slim book but dense with examples of how cartographic choices — projection, scale, color ramp, symbol size — all embed assumptions that shape what readers take away.

The Mercator projection is the classic example: it inflates polar landmasses to preserve angle fidelity, which made it useful for maritime navigation but terrible for communicating relative country sizes. Greenland appears roughly the same size as Africa, when Africa is actually about fourteen times larger. Monmonier shows how this wasn't a conspiracy, just a tool adopted in a context where it wasn't designed to be the default world map.

More interesting to me are the chapters on thematic maps — choropleth maps in particular. When you shade counties by income, the choice of how to bin the data (equal interval vs. quantile vs. natural breaks) dramatically changes the visual story. Quantile binning forces an equal number of counties into each color band regardless of the actual distribution, which can make modest variation look dramatic.

The chapter on road maps and their selective omissions is also good. Every map is a model. The act of leaving things out is a design decision, not a neutral reduction.

Companion reading: [Axis Maps guide to thematic mapping](https://www.axismaps.com/guide) covers similar terrain with more interactive examples.

Will probably follow this with J.B. Harley's essays on cartography and power — his argument that maps are rhetorical documents, not transparent windows onto reality, seems worth sitting with longer.
